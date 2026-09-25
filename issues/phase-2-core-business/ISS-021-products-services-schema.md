# ISS-021 — Master Data: Migrasi & Model Produk, Jasa

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-020
- **Perkiraan**: 0.5 hari
- **Area**: `database/migrations/`, `app/Models/`, `database/factories/`

## Konteks

Schema produk & jasa sesuai plan (#23 Database Core). Produk punya stok + min-stok (untuk halaman stok menipis nanti), harga beli & jual.

## Tasks

- [ ] Migration `create_products_table`: `id`, `tenant_id`, `category_id` (nullable FK), `unit_id` (nullable FK), `name`, `slug` (string nullable, unique `(tenant_id, slug)` — dipakai public website ISS-054/058), `sku` (nullable, unique `(tenant_id, sku)`), `cost_price` decimal(16,2) default 0, `sell_price` decimal(16,2) default 0, `stock` integer default 0, `min_stock` integer default 0, `photo` (string nullable), `description` text nullable, `is_active` bool default true, `timestamps`, `softDeletes`.
- [ ] Migration `create_services_table`: `id`, `tenant_id`, `category_id` nullable, `name`, `slug` (string nullable, unique `(tenant_id, slug)`), `price` decimal(16,2), `description` nullable, `is_active` default true, timestamps, softDeletes.
- [ ] Migration `create_product_photos` TIDAK perlu — foto tunggal di kolom `photo` (multi-foto nanti bila perlu).
- [ ] Model + trait `BelongsToTenant` + relasi `belongsTo` ke Category & Unit (bukan belongsToMany) + factory; helper generate slug unik per tenant (mis. `Str::slug($name)` + suffix angka bila tabrakan).
- [ ] Test schema: create produk dengan kategori/satuan; unique sku per tenant (tenant lain boleh sku sama).

## Acceptance Criteria

- [ ] `php artisan migrate` sukses.
- [ ] Test schema lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat UI (ISS-022).
- Jangan buat tabel stock_movements (ISS-023).
