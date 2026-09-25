# ISS-018 — Master Data: Migrasi & Model Pelanggan + Kategori

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-017 (Phase 1 selesai)
- **Perkiraan**: 0.5 hari
- **Area**: `database/migrations/`, `app/Models/`, `database/factories/`

## Konteks

Fondasi master data Phase 2. Tabel `customers` dan `categories` wajib memakai pola tenancy dari Phase 1 (trait `BelongsToTenant`).

## Tasks

- [ ] Migration `create_customers_table`: `id`, `tenant_id` (FK, wajib, index), `name`, `phone` (nullable, index), `email` (nullable), `address` (text nullable), `note` (text nullable), `timestamps`, `softDeletes`.
- [ ] Migration `create_categories_table`: `id`, `tenant_id`, `name`, `type` (enum: `PRODUCT`, `SERVICE`; default `PRODUCT`), `timestamps`, `softDeletes`; unique index `(tenant_id, name, type)`.
- [ ] Model `Customer`, `Category` + trait `BelongsToTenant` + fillable + relasi (Customer hasMany Transaction nanti; Category hasMany Product).
- [ ] Factory untuk keduanya (otomatis terisi tenant saat trait aktif).
- [ ] Test model dasar: create dengan trait mengisi tenant; unique constraint per tenant tidak bentrok antar tenant (2 tenant boleh punya kategori bernama sama).

## Acceptance Criteria

- [ ] `php artisan migrate` sukses.
- [ ] `php artisan test --filter=MasterData` lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat controller/UI (ISS-019).
- Jangan tambah kolom di luar daftar.
