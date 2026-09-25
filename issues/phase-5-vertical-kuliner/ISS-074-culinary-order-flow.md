# ISS-074 — Alur Order Kuliner: Di Tempat / Bungkus / Antar

- **Tag**: `[AI+riviu]` — UX order cepat kasir perlu review manusia
- **Depends on**: ISS-073
- **Perkiraan**: 1.5–2 hari
- **Area**: `database/migrations/`, `app/Actions/Culinary/`, `app/Http/Controllers/Admin/`, `resources/views/admin/culinary/orders/`, `tests/`

## Konteks

Flow order harian restoran/warung (plan #13): pilih menu (varian/add-on) → tipe order → catatan per item → simpan transaksi. Membangun UI order cepat di atas transaksi core. Status transaksi mengikuti `docs/state-machine-core-vertical.md` §5.3.

## Tasks

- [ ] Migration `create_culinary_orders_table`: `id`, `tenant_id`, `transaction_id` FK unique, `order_type` (enum `DI_TEMPAT`, `BUNGKUS`, `ANTAR`), `table_number` nullable (teks; tabel dining nanti), `delivery_address` nullable (wajib bila ANTAR), `customer_name` nullable (walk-in tanpa pelanggan), `source` enum (`KASIR`, `QR`; default `KASIR` — dipakai ISS-079), timestamps.
- [ ] Halaman "Order Cepat" (default kasir kuliner): grid menu dengan foto (klik = tambah), keranjang samping (varian/add-on prompt), pilih tipe order, catatan per item, nama pelanggan walk-in → simpan via transaksi core (item detail json berisi varian/add-on dari ResolveMenuItemPrice).
- [ ] Stok menu terpotong via core (RecordMovement) untuk menu yang memakai stok.
- [ ] Transaksi dimulai `PENDING`; kasir menutup via alur "Bayar & Selesai" (`RecordPayment` + `CloseTransaction`) → `COMPLETED` — BUKAN otomatis oleh `RecordPayment`. Bayar di muka direkam saat `PENDING` (lunas penuh OK); sisa → receivable `OPEN`. Kitchen `SELESAI` → `settle_ready = true` (ISS-075) tanpa menyentuh pembayaran.
- [ ] List order aktif hari ini (PENDING) + tombol selesaikan/batalkan (alasan wajib, stok kembali via core).
- [ ] Test: order ANTAR wajib alamat; varian wajib terpilih; total = menu+varian+addon; stok terpotong; batalkan (hanya bila belum ada pembayaran) → stok kembali; walk-in tanpa customer sukses; bayar muka tetap `PENDING` sampai `CloseTransaction`; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=CulinaryOrder` lulus.
- [ ] Manual dari HP: buat 3 menu (1 dengan varian+addon) tipe BUNGKUS → simpan → bayar → lunas.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan order mandiri pelanggan/QR (buffer ISS-079).
- Jangan meja fisik penuh (table_number teks dulu).
