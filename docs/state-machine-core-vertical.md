# State Machine: Core Transaction ⟷ Workflow Vertical

> **Status: KEPUTUSAN DESAIN (mengikat).** Dokumen ini mengunci aturan hubungan antara transaksi core (Phase 2) dan workflow vertical (Phase 4–6) **sebelum kode ditulis**. Semua issue di bawah harus mengikuti aturan ini. Jika ada konflik antara issue dan dokumen ini, **dokumen ini yang menang** — amandemen issue tercantum di §11.

---

## 1. Mengapa Dokumen Ini Ada

Ambiguitas paling mahal di proyek ini adalah makna status `COMPLETED` pada transaksi core. Masalah konkret yang sudah terdeteksi sejak Phase 2:

| Issue | Pernyataan | Masalah |
| --- | --- | --- |
| ISS-026 | Tombol manual "Selesaikan" → `COMPLETED` | Makna *operasional* (order selesai) |
| ISS-027 | "Status transaksi otomatis COMPLETED saat lunas" | Makna *finansial* (sudah dibayar) |
| ISS-028 | Receivable dibuat "saat transaksi COMPLETED & belum lunas" | **Mustahil** — bila COMPLETED = lunas, piutang tak akan pernah tercipta |
| ISS-030 | Invoice terbit "saat COMPLETED" | Bila COMPLETED = lunas, invoice terlambat/keliru untuk kasus piutang |

Jika dibiarkan, percetakan DP-50% (ISS-068: DP saat order, pelunasan saat ambil) **tidak akan pernah menghasilkan piutang** dan invoice baru terbit setelah lunas. Dokumen ini menyelesaikan kontradiksi itu dengan satu keputusan inti di §3.

---

## 2. Fondasi: Dua Sumbu yang Terpisah

```
┌─ SUMBU OPERASIONAL (vertical) ─────────────┐   ┌─ SUMBU FINANSIAL (core) ──────────────┐
│ production_orders / kitchen_orders /       │   │ transactions                          │
│ service_orders                             │   │ payments / receivables / invoices      │
│                                            │   │ laporan & dashboard                   │
│ Sumber kebenaran: KEMAJUAN layanan         │   │ Sumber kebenaran: UANG                │
└───────────────▲────────────────────────────┘   └────────────▲─────────────────────────┘
                │                                            │
                └───────────── sync 1 arah ──────────────────┘
                        (vertical → core, lewat Action)
```

**Rule A — Satu sumber uang:** semua nilai uang diikat ke core `transactions`. Vertikal menyimpan snapshot harga untuk konteks cetak, tapi **invoice, payment, receivable, dan laporan wajib lewat core.** (Sudah ditegaskan ISS-083: "Jangan dua sumber uang".)

**Rule B — Sync satu arah:** tabel vertical adalah *source of truth* kemajuan; core transaksi adalah *sink* uangnya. Perubahan status vertical memicu Action core, **bukan** sebaliknya, dan tidak pernah lewat observer implisit (harus eksplisit di Action agar bisa ditest).

---

## 3. Makna Status `transactions.status` (RESOLUSI KONTRADIKSI)

### Keputusan

**`transactions.status` adalah siklus hidup ORDER/FULFILLMENT — bukan status pembayaran.**

| Status | Makna | Cara masuk |
| --- | --- | --- |
| `PENDING` | Order diterima, proses berjalan | Saat transaksi dibuat (create / konversi / stage vertical) |
| `COMPLETED` | Order **ditutup** (fulfillment final / diselesaikan kasir). `completed_at` terisi | Action `CloseTransaction` (lihat §5) |
| `CANCELLED` | Dibatalkan. Wajib `cancel_reason`. Hanya boleh dari `PENDING` (dan `paid_amount = 0`, lihat §7). **Pengecualian:** `COMPLETED → CANCELLED` diizinkan bila dipicu **refund penuh** (paid_amount sudah dinolkan lebih dulu — lihat §7) | Action `CancelTransaction` |

### Status pembayaran adalah SUMPAH TERPISAH (derived, bukan kolom status)

Status bayar dihitung dari `payments` (CONFIRMED) + kolom `transactions.paid_amount`:

```
is_paid = (paid_amount >= total)
sisa    = total - paid_amount            → menggerakkan receivable
```

Implementasi turunan (accessor di model `Transaction`, bukan kolom DB baru):
`is_paid`, `is_partially_paid`, `remaining_amount`, `is_settle_ready` (read `settle_ready` kolom, §5).

### Akibat langsung

- `COMPLETED` **tidak** berarti lunas. Order dicetak & diambil tapi sisa belum dibayar → `COMPLETED` dengan receivable `OPEN`. Ini perilaku percetakan yang benar.
- `PENDING` **boleh** sudah dibayar DP / penuh (percetakan DP 50%, kuliner bayar di muka). Pembayaran tidak mengubah status transaksi.
- Invoice diterbitkan pada `COMPLETED` — konsisten dengan ISS-030, dan piutang bisa berjalan berdampingan sekarang.

### Amandemen yang diakibatkan (detail di §11)

- **ISS-027**: hapus "status transaksi otomatis COMPLETED saat lunas".
- **ISS-028**: receivable digerakkan oleh `paid_amount`, bukan oleh `COMPLETED`.

---

## 4. Titik Masuk Uang (Money Point) per Vertical

Setiap vertical menentukan kapan core transaction mulai ada. Sebelum titik itu, alur berjalan **tanpa** transaksi (data operasional murni).

| Vertical | Pra-uang (tanpa core transaction) | Money point → transaksi dibuat | Status awal transaksi |
| --- | --- | --- | --- |
| Percetakan | `quotations` DRAFT → SENT → ACCEPTED | Konversi quotation ACCEPTED (ISS-064) atau order kilat | `PENDING` |
| Kuliner | — (langsung dibuat) | Saat simpan order cepat (ISS-074) | `PENDING` |
| Bengkel | `service_orders` BOOKING → … → PERSETUJUAN | Stage `PEMBAYARAN` (ISS-083): item service order → `transaction_items` (snapshot) + potong stok | `PENDING` |

**Catatan bengkel:** seluruh alur diagnostik (BOOKING hingga QC) berjalan TANPA transaksi. Yang masuk `transaction_items` adalah snapshot saat `PEMBAYARAN` (versi yang sudah di-approve). Edit items setelah PERSETUJUAN tetap dilarang (ISS-083).

---

## 5. Peta Status & Event Sync (TABEL KUNCI)

### 5.1 Kolom tambahan di `transactions` (amandemen migrasi ISS-025)

```text
settle_ready   boolean  default false     ← "proses selesai, tinggal bayar/ambil" (penanda UI)
completed_at   datetime nullable
cancelled_at   datetime nullable
```

`settle_ready` di-*set* oleh event terminal vertical dan dibaca dashboard/laporan (denormalisasi demi query cepat; ditulis oleh Action, bukan turunan).

### 5.2 Percetakan — `production_orders`

```text
MENUNGGU → DICETAK → FINISHING → QC → SIAP → SELESAI
     auto-create saat transaksi percetakan dibuat (ISS-065)
```

| Event | Action core yang dipanggil | Efek |
| --- | --- | --- |
| production SELESAI | `SetSettleReady` | `transactions.settle_ready = true` (+ audit). **Tidak** menyentuh status pembayaran |
| Kasir "Selesaikan" / bayar sisa saat ambil | `CloseTransaction` | `PENDING → COMPLETED`, `completed_at`; issue invoice + refresh receivable |

Guard: item `SIAP` dengan sisa belum lunas → warning kuning (ISS-068), tidak diblok.

### 5.3 Kuliner — `kitchen_orders`

```text
MENUNGGU → DIMASAK → SIAP → DIANTAR/DIAMBIL → SELESAI
     auto-create saat culinary order dibuat (ISS-075); DIANTAR khusus tipe ANTAR
```

| Event | Action core yang dipanggil | Efek |
| --- | --- | --- |
| kitchen SELESAI | `SetSettleReady` | `settle_ready = true` (+ audit) |
| Kasir bayar (bayar di muka/saat selesai) | `RecordPayment` lalu `CloseTransaction` (satu tombol "Bayar & Selesai") | `COMPLETED`, invoice & receivable di-refresh |

### 5.4 Bengkel — `service_orders`

```text
BOOKING → CHECK-IN → PEMERIKSAAN → ESTIMASI → PERSETUJUAN → SERVIS → QC → PEMBAYARAN → SELESAI
     (PERSETUJUAN wajib sebelum SERVIS — guard ISS-083/084)
```

| Event | Action core yang dipanggil | Efek |
| --- | --- | --- |
| Stage `PEMBAYARAN` | `CreateTransactionFromServiceOrder` | Transaksi `PENDING` dari snapshot items, sparepart potong stok |
| Stage `SELESAI` (mobil diambil) | `CloseTransaction` | `COMPLETED`, `done_at` juga tercatat di service order |

### 5.5 Retail/SALE (tanpa fulfillment — toko, POS)

- Transaksi `type = SALE` dibuat langsung selesai: kasir bayar → `RecordPayment` lalu `CloseTransaction` segera.

### Action baru yang diperkenalkan

| Action | Dibuat di | Dipanggil oleh | Tanggung jawab |
| --- | --- | --- | --- |
| `Transactions\CloseTransaction` | ISS-026 (core) | Kasir/OWNER, atau event terminal vertical | `PENDING → COMPLETED` + `completed_at`; panggil `IssueInvoice` bila belum ada + refresh receivable. Idempotent (punya `completed_at` → no-op) |
| `Transactions\SetSettleReady` | ISS-026 (core) | Event terminal vertical (production/kitchen SELESAI, bengkel SERVIS tuntas) | `settle_ready = true` + audit. Tidak menyentuh status/payment |
| `Transactions\CancelTransaction` | ISS-026 (core) | Kasir/OWNER | Validasi hanya `PENDING` & `paid_amount = 0` (pengecualian full-refund lihat §7); stok kembali via `RecordMovement` IN; receivable dihapus; `CANCELLED` + `cancel_reason` + `cancelled_at` |
| `Workshop\CreateTransactionFromServiceOrder` | ISS-083 (vertical bengkel) | Stage `PEMBAYARAN` service order | Buat transaksi core `PENDING` dari snapshot `service_order_items`; sparepart potong stok; isi `service_orders.transaction_id` |

Semua Action berjalan dalam `DB::transaction` (atomik) dan memakai model bertrait `BelongsToTenant` (isolasi terjamin).

---

## 6. Aturan Sinkronisasi (injection points)

1. Setiap transisi status vertical → **Action core dipanggil secara eksplisit di Action vertical yang sama**, dalam satu `DB::transaction`. Jangan menaruh logika uang di observer/model event — sulit ditest & rentan tidak konsisten.
2. Event terminal vertical **tidak pernah** mengubah status pembayaran/`paid_amount`. Pembayaran hanya digerakkan `RecordPayment`.
3. Pengecekan `settle_ready`/`completed_at` hanyalah baca; penulisan hanya lewat Action §5.5.
4. Semua query uang & laporan memakai model core bertrait → isolasi tenant otomatis. Join ke vertical hanya untuk dimensi (mis. per mekanik, per produk cetak) dan tetap dalam scope tenant.

---

## 7. Pembatalan

| Skenario | Yang terjadi |
| --- | --- |
| Quotation DITOLAK (belum ada transaksi) | Tidak ada efek ke core |
| Work order batal sebelum `PEMBAYARAN` (belum ada transaksi) | Hanya `service_orders.status = CANCELLED` |
| Order kuliner/percetakan dibatalkan setelah transaksi ada | `CancelTransaction`: hanya boleh bila `paid_amount = 0`; stok kembali; vertical status `CANCELLED` |
| Sudah ada pembayaran masuk, order dibatalkan — **refund penuh** | Diizinkan: semua payment `CONFIRMED` → `REFUNDED` (ISS-027), `paid_amount = 0`, stok kembali via RecordMovement IN (reference refund), transaksi `COMPLETED → CANCELLED` + `cancel_reason = REFUND` (pengecualian pada §3), invoice `CANCELLED`, receivable `SETTLED`. Seluruh langkah atomik dalam action refund (ISS-043) |
| Sudah ada pembayaran masuk, order dibatalkan — **refund parsial** | TIDAK boleh cancel langsung. Catat pada tabel `refunds` (amount decimal, pengurang `paid_amount`), receivable disesuaikan; transaksi tetap berstatus (bila `COMPLETED`, tetap COMPLETED dengan sisa dikoreksi) |

Enum vertical ditambah `CANCELLED` pada `production_orders.status`, `kitchen_orders.status`, `service_orders.status`.

---

## 8. Pembayaran & Piutang (resolusi dual-FK)

- `payments` memakai **satu FK `transaction_id`** (sudah benar di ISS-027) — invoice bukan sumber pembayaran.
- Invoice (ISS-030) adalah **dokumen** yang mereferensikan `transaction_id` (unique). Tidak ada `invoice_id` di payments.
- Billing langganan Phase 7 (subscription_invoices) memakai tabel sendiri dan **tidak** menyentuh `payments` tenant.
- `receivables`: satu baris per transaksi (`transaction_id` unique), di-refresh oleh `RecordPayment`, `CloseTransaction`, dan `CancelTransaction`:

```text
sisa = total - paid_amount
receivable.status = OPEN  bila sisa > 0  (bukan mensyaratkan COMPLETED)
receivable.status = SETTLED bila sisa == 0 (settled_at terisi)
```

- DP percetakan (ISS-068) = `RecordPayment` biasa saat transaksi `PENDING`; sisa → receivable otomatis terbuka. Tidak perlu logika baru.

---

## 9. Dokumen — semua dari core

Invoice (ISS-030), nota/kwitansi (ISS-031), surat jalan (ISS-067), struk thermal (ISS-076) **wajib mereferensikan transaksi core dan render dari `transaction_items`**. Tidak ada dokumen uang yang digenerate dari tabel vertical.

---

## 10. Laporan — semua dari core

- Sumber angka: `transactions`, `payments`, `receivables` (filter `tenant_id` dari context).
- Vertical hanya menyediakan **dimensi** lewat relasi (contoh: laporan percetakan per jenis produksi, laporan bengkel per mekanik) — join tetap dalam scope tenant.
- Rule praktis: jika sebuah angka bisa diduplikasi oleh dua tabel yang berbeda → salah; satu sumber uang wajib core.

---

## 11. Amandemen Issue yang Diperlukan

| Issue | Perubahan |
| --- | --- |
| ISS-025 | Tambah kolom `settle_ready`, `completed_at`, `cancelled_at` di migrasi `transactions`; tambah accessor `is_paid`, `remaining_amount` |
| ISS-026 | Perjelas: "Selesaikan" memanggil `CloseTransaction` (set `completed_at`); cancel hanya bila `paid_amount = 0` |
| ISS-027 | **Hapus** "status transaksi otomatis COMPLETED saat lunas". Ganti: `RecordPayment` hanya update `paid_amount` + receivable. Tambahan: untuk `type = SALE` boleh auto-`CloseTransaction` bila kasir bayar lunas (konvenien POS) |
| ISS-028 | Receivable digerakkan `paid_amount`/`RecordPayment`/`CloseTransaction`/`CancelTransaction`, BUKAN hanya saat `COMPLETED` (lihat §8) |
| ISS-030 | Perjelas konteks: invoice terbit pada `CloseTransaction` (order ditutup), bukan pada "lunas" |
| ISS-065 | Ganti "penanda siap disettle" → set `transactions.settle_ready = true` via `SetSettleReady`; tambah status `CANCELLED` |
| ISS-068 | Tidak perlu "ada koreksi" — berjalan di atas Rule §5/§8; pastikan test DP → receivable terbuka pada `PENDING` |
| ISS-074 | Perjelas: `COMPLETED` dipicu alur kasir "Bayar & Selesai" (`RecordPayment` + `CloseTransaction`), bukan otomatis oleh `RecordPayment` |
| ISS-075 | Ganti "penanda siap-disettle" → `settle_ready` via `SetSettleReady`; tambah status `CANCELLED` |
| ISS-083 | Tambah status `CANCELLED` ke `service_orders`; perjelas transaksi dibuat utamanya di stage `PEMBAYARAN` (snapshot) & `CloseTransaction` di `SELESAI` |
| ISS-026 | Menjadi pemilik & lokasi implementasi `Transactions\SetSettleReady` (baris action table §5.5) |
| ISS-083 | Menjadi pemilik & lokasi implementasi `Workshop\CreateTransactionFromServiceOrder` (baris action table §5.5) |
| ISS-027 | Tambah enum `REFUNDED` pada `payments.status` (dipakai refund penuh §7/ISS-043) |
| ISS-043 | Definisikan tabel `refunds` (amount decimal(16,2)) + action refund; izinkan `COMPLETED → CANCELLED` hanya lewat full refund (paid_amount dinolkan dulu) |

---

## 12. Test Wajib (isolasi & regresi)

Berlaku untuk semua modul yang menyentuh status:

1. **Satu sumber uang**: tidak ada tabel selain core yang menyimpan jumlah yang dipakai laporan (inspeksi kode + test laporan).
2. **Kontradiksi lama hilang**: transaksi `PENDING` + DP 50% → receivable `OPEN` benar (sisa 50%). Lunas → `SETTLED`.
3. **Invoice** terbit tepat satu kali saat `CloseTransaction`; idempotent.
4. **`settle_ready`**: terminal vertical set true; tidak mengubah `paid_amount`/status pembayaran.
5. **Cancel** hanya `PENDING` & `paid_amount = 0`; stok kembali; receivable dihapus.
6. **Bengkel**: stage `PEMBAYARAN` membuat transaksi dari snapshot; tanpa approve tidak bisa lanjut `SERVIS`; `SELESAI` → `COMPLETED`.
7. **Isolasi**: setiap skenario di atas diuji dengan tenant B → `403/404` (aksi core memakai trait scope, bukan id mentah).

---

## 13. Ringkasan Visual

```text
PERCETAKAN
quotation ──ACCEPTED──▶ ┌─ transactions (PENDING) ─┐
                       │   production_orders:      │
                       │   MENUNGGU→…→SELESAI ──✅──▶ settle_ready=true
                       │   kasir bayar sisa ──▶ CloseTransaction ─▶ COMPLETED
                       └──▶ invoice + receivable (sisa otomatis)

KULINER
order ──▶ transactions (PENDING) + culinary_orders + kitchen_orders
                             kitchen MENUNGGU→…→SELESAI ──✅──▶ settle_ready=true
                             kasir "Bayar & Selesai" ──▶ COMPLETED

BENGKEL
service_orders BOOKING…PERSETUJUAN (tanpa transaksi)
    └─ PEMBAYARAN ──▶ transactions dibuat (PENDING, snapshot items, stok)
    └─ SELESAI ──▶ CloseTransaction ──▶ COMPLETED (invoice + receivable)
```