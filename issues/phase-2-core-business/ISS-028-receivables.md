# ISS-028 — Piutang (Receivables)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-027
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Actions/Payments/`, `resources/views/admin/receivables/`, `tests/`

## Konteks

Piutang = sisa tagihan transaksi (`total − paid_amount`). Digerakkan oleh `paid_amount`, **bukan** status transaksi. Satu baris per transaksi, dibuat/diperbarui/dihapus oleh Action (RecordPayment / CloseTransaction / CancelTransaction). Lihat `docs/state-machine-core-vertical.md` §8. Umur piutang membantu follow-up WA nanti.

## Tasks

- [ ] Migration `create_receivables_table`: `id`, `tenant_id`, `transaction_id` FK unique, `remaining_amount` decimal(16,2), `due_date` date nullable, `status` (enum `OPEN`, `SETTLED`; default `OPEN`), `settled_at` nullable, timestamps.
- [ ] Integrasi otomatis (di dalam Action yang sama, bukan observer, agar atomik): saat `RecordPayment` / `CloseTransaction` → hitung sisa; sisa > 0 → receivable `OPEN` (dibuat bila belum ada); sisa = 0 → `SETTLED` + `settled_at`; saat `CancelTransaction` → receivable dihapus. Tidak mensyaratkan status `COMPLETED`.
- [ ] Halaman "Piutang": daftar OPEN (pelanggan, nomor transaksi, total, dibayar, sisa, umur hari, due date), ringkasan total piutang di atas, tombol "Bayar" mengarah ke pembayaran transaksi tsb; tab "Lunas" riwayat.
- [ ] Due date bisa diisi saat create transaksi (opsional, default +7 hari).
- [ ] Ringkasan piutang muncul di widget dashboard (update widget placeholder dari ISS-007: angka nyata).
- [ ] Test: DP saat transaksi `PENDING` → receivable OPEN sisa benar (tanpa menunggu COMPLETED); lunaskan → SETTLED; cancel transaksi → receivable ikut dihapus; umur hari dihitung benar; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Receivable` lulus.
- [ ] Widget dashboard menampilkan total piutang OPEN yang benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat pengingat otomatis (fase lanjutan).
