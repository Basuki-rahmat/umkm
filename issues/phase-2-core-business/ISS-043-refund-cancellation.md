# ISS-043 — Refund & Edge Case Pembatalan

- **Tag**: `[AI+riviu]` — kebijakan bisnis refund perlu keputusan manusia
- **Depends on**: ISS-042
- **Perkiraan**: 1 hari
- **Area**: `app/Actions/Payments/`, `app/Actions/Transactions/`, `resources/views/admin/`, `tests/`

## Konteks

Status `REFUNDED` ada di plan (#19) tapi belum diimplementasi. Menutup celah kasus sulit: refund, transaksi COMPLETED yang harus dibatalkan, stok yang sudah berubah sejak transaksi.

## Tasks

- [ ] Kebijakan (implement sesuai ini): transaksi COMPLETED **tidak bisa dibatalkan langsung** — harus lewat Refund. Pengecualian: `CANCELLED` dari `COMPLETED` hanya diizinkan bila dipicu **refund penuh** (`paid_amount` dinolkan dulu lewat langkah lintas di bawah) — sesuai `docs/state-machine-core-vertical.md` §7.
- [ ] Refund penuh: aksi OWNER di detail transaksi → semua pembayaran CONFIRMED berstatus `REFUNDED` (enum di ISS-027), `paid_amount = 0`, stok item produk dikembalikan (RecordMovement IN reference refund), transaksi berstatus `CANCELLED` + cancel_reason `REFUND`, invoice berstatus `CANCELLED`, receivable SETTLED (jika ada).
- [ ] Refund parsial: input jumlah ≤ total dibayar → buat payment NEGATIF dilarang; sebagai gantinya tabel `refunds` (`tenant_id`, transaction_id, `amount` decimal(16,2), reason, created_by, timestamps) yang mengurangi paid_amount; receivable disesuaikan.
- [ ] Batalkan transaksi PENDING dengan stok sudah berubah: pengembalian stok tetap dicatat apa adanya (bisa minus di catatan) — jangan crash; tampilkan warning di UI.
- [ ] Semua aksi refund = audit log REFUND dengan detail.
- [ ] Test: refund penuh (status semua entitas benar); refund parsial (paid_amount & receivable benar); pembayaran > sisa setelah refund ditolak; alur batal PENDING stok berubah tidak error.

## Acceptance Criteria

- [ ] `php artisan test --filter=Refund|Cancel` lulus.
- [ ] Manual: refund penuh 1 transaksi → status & stok & invoice & piutang konsisten.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan izinkan refund tanpa reason.
- Jangan hard-delete data apa pun.
