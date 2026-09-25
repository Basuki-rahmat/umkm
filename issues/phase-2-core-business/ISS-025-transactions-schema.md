# ISS-025 — Transaksi: Migrasi, Model & Generator Nomor

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-024
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Support/`, `tests/`

## Konteks

Jantung sistem: transaksi penjualan. Nomor per tenant per periode: `TRX/202609/0001`. Item bisa dari produk (potong stok) atau jasa (tanpa stok).

## Tasks

- [ ] Migration `create_transactions_table`: `id`, `tenant_id`, `number` (unique `(tenant_id, number)`), `customer_id` nullable FK, `type` (enum `SALE`, `ORDER`; default `SALE`), `status` (enum `PENDING`, `COMPLETED`, `CANCELLED`; default `PENDING`), `cancel_reason` nullable, `settle_ready` boolean default false, `paid_amount` decimal(16,2) default 0 (diset oleh RecordPayment di ISS-027, bukan di sini), `completed_at` datetime nullable, `cancelled_at` datetime nullable, `subtotal`, `discount`, `tax`, `total` (semua decimal(16,2) default 0), `note` nullable, `created_by` (FK users), `timestamps`, `softDeletes`.
- [ ] Migration `create_transaction_items_table`: `id`, `tenant_id`, `transaction_id` FK cascade, `item_type` (enum `PRODUCT`, `SERVICE`), `item_id`, `name_snapshot` (nama saat jual), `qty` decimal(12,2), `unit_price`, `discount`, `subtotal` (decimal 16,2), `detail` json nullable (snapshot opsi/varian/bahan dari vertical — info: diisi oleh konversi quotation percetakan / order menu vertikal), timestamps. Index `(tenant_id, transaction_id)`.
- [ ] Model `Transaction` + trait + relasi + factory. Accessor turunan: `is_paid`, `is_partially_paid`, `remaining_amount`, `is_settle_ready` (dari `paid_amount` vs `total` dan kolom `settle_ready`).
- [ ] Makna status & aturan sync dipegang `docs/state-machine-core-vertical.md`: `status` = siklus order (bukan status bayar); `COMPLETED` dicapai hanya lewat `CloseTransaction`; payment tidak mengubah status.
- [ ] Class `App\Support\DocumentNumber::next(string $prefix, ?Model $tenantModel = null, array $options = [])` — atomik (lockForUpdate pada counter per tenant+periode; tabel `document_counters`: `tenant_id` nullable, prefix, period string, last_number, timestamps; unique `(tenant_id, prefix, period)` — `tenant_id = null` dipakai counter global/platform, mis. SUB di ISS-094). `period` & `padding` dikonfigurasi per prefix (default `YYYYMM` + padding 4; `INV` memakai `YYYY` + padding 6 → `INV/2026/000001`).
- [ ] Test: nomor berurutan per tenant; dua tenant tidak saling mengganggu counter; atomic test (2 proses tidak dapat nomor sama — cukup loop simulasi).

## Acceptance Criteria

- [ ] `php artisan migrate` sukses.
- [ ] Test nomor lulus (bersih, berurutan, per-tenant).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat UI transaksi (ISS-026).
- Jangan potong stok di issue ini (bagian dari flow create ISS-026).
