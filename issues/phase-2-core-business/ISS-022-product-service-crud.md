# ISS-022 — CRUD Produk & Jasa

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-021
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Http/Controllers/Admin/`, `app/Http/Requests/`, `resources/views/admin/products/`, `tests/`

## Konteks

CRUD produk (dengan foto) dan jasa. Foto disimpan per tenant via `TenantStorage`. Pola sama dengan CRUD pelanggan.

## Tasks

- [ ] Produk: index (search nama/sku, filter kategori, filter is_active, pagination), create/edit via `ProductRequest` (nama required; sku nullable unik per tenant; harga ≥ 0; stok & min-stok integer ≥ 0; foto opsional maks 5MB via rule `AllowedUpload` default ISS-012, simpan ke `tenant-{id}/products/`).
- [ ] Jasa: index + create/edit (nama, kategori, harga, deskripsi, is_active) — tanpa stok.
- [ ] Toggle is_active cepat dari list (switch).
- [ ] Sidebar: menu "Produk" & "Jasa" (role STAFF & OWNER).
- [ ] Audit log CREATE/UPDATE/DELETE.
- [ ] Test: CRUD produk & jasa; upload foto tersimpan di folder tenant benar; sku duplikat dalam 1 tenant ditolak, tenant berbeda diterima; isolasi 403/404.

## Acceptance Criteria

- [ ] `php artisan test --filter=Product|Service` lulus.
- [ ] Manual: buat produk dengan foto → tampil & tersimpan di `storage/app/public/tenant-{id}/products/`.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan implementasi mutasi stok manual (ISS-023).
