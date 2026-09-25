# ISS-062 — Quotation: Migrasi & Model

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-061
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Models/`, `tests/`

## Konteks

Penawaran harga (plan #11): sebelum jadi pesanan, percetakan sering kirim quotation. Nomor `QTN/202609/0001` per tenant (DocumentNumber prefix `QTN`).

## Tasks

- [ ] Migration `create_quotations_table`: `id`, `tenant_id`, `number` unique `(tenant_id, number)`, `customer_id` nullable FK, `status` (enum `DRAFT`, `SENT`, `ACCEPTED`, `REJECTED`, `EXPIRED`; default `DRAFT`), `total` decimal(16,2) default 0, `valid_until` date nullable, `sent_at` datetime nullable (ditulis saat kirim, ISS-063), `note` nullable, `created_by`, timestamps, softDeletes.
- [ ] Migration `create_quotation_items_table`: `id`, `tenant_id`, `quotation_id` FK cascade, `name_snapshot`, `qty` decimal(12,2), `unit_price` decimal(16,2), `subtotal` decimal(16,2), `detail` json nullable (rincian bahan/ukuran/finishing dari PriceResult), timestamps.
- [ ] Model + trait + relasi + factory.
- [ ] Test: nomor per tenant berurutan; relasi items; status default.

## Acceptance Criteria

- [ ] `php artisan migrate` sukses; test schema lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat UI (ISS-063).
- Jangan relasi ke transaksi dulu (ISS-064).
