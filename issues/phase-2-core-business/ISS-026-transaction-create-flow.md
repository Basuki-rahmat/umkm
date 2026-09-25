# ISS-026 — Flow Buat Pesanan/Transaksi

- **Tag**: `[AI+riviu]` — UX form transaksi perlu review manusia
- **Depends on**: ISS-025
- **Perkiraan**: 1.5–2 hari
- **Area**: `app/Actions/Transactions/`, `app/Http/Controllers/Admin/`, `resources/views/admin/transactions/`, `tests/`

## Konteks

Flow inti harian kasir: pilih pelanggan → tambah item (produk/jasa, qty, diskon item) → hitung otomatis → simpan. Simpan wajib atomik (DB transaction) dan memotong stok via `RecordMovement`.

## Tasks

- [ ] Halaman create: pilih pelanggan (searchable dropdown), tambah baris item: pilih produk (search + tampil stok) atau jasa, qty, harga (bisa dioverride), diskon per item; ringkasan subtotal/diskon/pajak (persen, setting per tenant, default 0)/total live-update (Alpine.js).
- [ ] Action `CreateTransaction`: validasi stok cukup untuk semua item produk; DB::transaction { generate nomor, create header, items, RecordMovement OUT per item produk }; hitung ulang total SERVER-SIDE (jangan percaya angka client).
- [ ] Status awal `PENDING`; tombol "Selesaikan" memanggil Action `CloseTransaction` (set `completed_at`; issue invoice + refresh receivable bila belum ada) & "Batalkan" memanggil `CancelTransaction` (alasan wajib → stok dikembalikan via RecordMovement IN; hanya saat `PENDING` **dan** `paid_amount = 0`). Aturan & guard lengkap: `docs/state-machine-core-vertical.md` §5.5 & §7.
- [ ] Action `SetSettleReady` (di `app/Actions/Transactions/`): idempotent, set `settle_ready = true` — dipanggil oleh event terminal fulfillmen vertical (percetakan: quotation SELESAI ISS-065; kuliner: order SELESAI ISS-075; bengkel: servis tuntas ISS-086) sebagai prasyarat `CloseTransaction`; tidak menyentuh `paid_amount`/status. Kepemilikan & guard mengikuti `docs/state-machine-core-vertical.md` §5.5.
- [ ] Halaman index transaksi: filter status/tanggal/rentang/pelanggan, search nomor; detail transaksi (items, status timeline sederhana).
- [ ] Role: KASIR & OWNER boleh buat; batalkan hanya OWNER (atau pembuatnya, jika OWNER).
- [ ] Audit log: CREATE, CANCEL (dengan alasan), COMPLETE.
- [ ] Test: total = Σ item − diskon + pajak (server-side); stok terpotong benar; batalkan → stok kembali + status CANCELLED + alasan tersimpan; cancel ditolak bila `paid_amount > 0`; stok tidak cukup → gagal tanpa data parsial (atomik); isolasi tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Transaction` lulus.
- [ ] Manual dari HP: buat transaksi lengkap 3 item produk + 1 jasa → sukses, stok berkurang sesuai.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat pembayaran (ISS-027).
- Jangan izinkan transaksi tenant lain via hidden id — server wajib resolve dari scope.
