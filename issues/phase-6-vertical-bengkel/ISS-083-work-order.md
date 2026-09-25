# ISS-083 — Bengkel: Work Order (Schema & Alur Status)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-082
- **Perkiraan**: 1.5 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Actions/Workshop/`, `app/Http/Controllers/Admin/Workshop/`, `resources/views/admin/workshop/orders/`, `tests/`

## Konteks

Work order (plan #12): BOOKING → CHECK-IN → PEMERIKSAAN → ESTIMASI → PERSETUJUAN → SERVIS → QC → PEMBAYARAN → SELESAI. **Persetujuan estimasi wajib sebelum SERVIS** — aturan kunci. Transaksi core dibuat di stage PEMBAYARAN dan ditutup di SELESAI — `docs/state-machine-core-vertical.md` §5.4.

## Tasks

- [ ] Migration `create_service_orders_table`: `id`, `tenant_id`, `number` (DocumentNumber prefix `WO`) unique `(tenant_id, number)`, `vehicle_id` FK, `mechanic_id` nullable FK, `complaint` text, `km` int nullable, `status` (enum `BOOKING`, `CHECK_IN`, `PEMERIKSAAN`, `ESTIMASI`, `PERSETUJUAN`, `SERVIS`, `QC`, `PEMBAYARAN`, `SELESAI`, `CANCELLED`; default `CHECK_IN` — WO manual dari kasir melewati stage BOOKING; WO dari booking online (ISS-086) dibuat dengan status `BOOKING`), `booking_at` nullable, `checked_in_at`, `estimated_cost` decimal(16,2) nullable, `approved_at` nullable, `done_at` nullable, `transaction_id` nullable FK unique (dibuat saat PEMBAYARAN), timestamps.
- [ ] Migration `create_service_order_items_table`: `tenant_id`, service_order_id, `item_type` (enum `SPAREPART`, `SERVICE`), `item_id` (produk core / jasa core), `name_snapshot`, `qty` decimal(12,2), `unit_price` decimal(16,2), `subtotal` decimal(16,2), timestamps.
- [ ] Migration `create_service_order_logs_table`: `id`, `tenant_id`, service_order_id, from_status, to_status, note nullable, user_id, created_at (jejak perpindahan + dasar durasi per status — dibaca laporan ISS-087; pola `production_logs` Phase 4). Ditulis oleh `AdvanceServiceOrderStatus`.
- [ ] Action `CreateTransactionFromServiceOrder` (di `app/Actions/Workshop/`): buat transaksi core `PENDING` (type `ORDER`) dari snapshot `service_order_items` saat stage PEMBAYARAN — mapping `SPAREPART→PRODUCT` (potong stok core), `SERVICE→SERVICE`; isi `service_orders.transaction_id`. Guard & kepemilikan mengikuti aturan sinkronisasi `docs/state-machine-core-vertical.md` §6 dan tabel action §5.5.
- [ ] Action `AdvanceServiceOrderStatus`: validasi transition legal; CHECK-IN→PEMERIKSAAN→ESTIMASI (isi estimasi biaya + items draft) → **PERSETUJUAN (wajib klik approve; mencatat approved_at)** → SERVIS → QC → PEMBAYARAN (memakai `CreateTransactionFromServiceOrder`) → SELESAI (done_at) + panggil `CloseTransaction` (`COMPLETED`). Batal di tengah alur → `CANCELLED`; bila transaksi sudah dibuat, ikuti aturan pembatalan core (docs §7).
- [ ] Halaman list work order (filter status/hari) + detail (items, status timeline, kilometer, mekanik); tombol pindah status sesuai role (STAFF sampai QC; PEMBAYARAN = KASIR/OWNER).
- [ ] Jasa servis: dari `services` core (jasa ganti oli, tune-up) — tenant bengkel cukup punya beberapa jasa.
- [ ] Test: alur lengkap membuat transaksi + stok sparepart terpotong; **lanjut ke SERVIS tanpa approve ditolak**; transition liar ditolak; nomor WO per tenant; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=ServiceOrder` lulus.
- [ ] Manual: 1 work order end-to-end dari HP sampai transaksi terbentuk.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan dua sumber uang (PEMBAYARAN = transaksi core, bukan tabel baru).
- Jangan izinkan edit items setelah PERSETUJUAN (harus batal + work order baru — sederhana untuk MVP).
