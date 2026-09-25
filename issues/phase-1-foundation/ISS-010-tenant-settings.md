# ISS-010 — Pengaturan Tenant (Profil Usaha)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-009
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/`, `resources/views/admin/settings/`, `tests/`

## Konteks

Data profil usaha dipakai panel dan (nanti) website publik: nama, logo, kontak, alamat, sosmed, koordinat maps. Disimpan sebagai key-value agar mudah dikembangkan.

## Tasks

- [ ] Migration `settings` (tenant_id, `key` string, `value` text nullable, unique [tenant_id, key]).
- [ ] Model `Setting` **memakai trait `BelongsToTenant`** (CONVENTIONS §2.2 — jangan filter manual di helper) + helper `setting(string $key, $default = null)` scoped ke tenant aktif (baca tulis).
- [ ] Halaman Pengaturan → tab "Profil Usaha": nama usaha, deskripsi singkat, telepon/WA, email, alamat lengkap, kota, instagram, facebook, tiktok, latitude/longitude (opsional — key `map_lat`/`map_lng`), link Google Maps (opsional — key `map_embed_url`; dipakai ISS-049/057).
- [ ] Upload logo (image, maks 2MB, simpan `TenantStorage::path($tenantId, 'logo')`, preview tampil di halaman + favicon opsional).
- [ ] Validasi & simpan otomatis key yang kosong sebagai null.
- [ ] Test: simpan & baca setting per tenant (2 tenant berbeda nilainya), upload logo tersimpan di folder tenant yang benar.

## Acceptance Criteria

- [ ] `php artisan test --filter=Settings` lulus.
- [ ] Manual: isi form → refresh → data tetap; logo tampil.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat tab pengaturan lain (template/theme nanti di Phase 3).
- Jangan hardcode nama usaha di layout — pakai `setting('business_name')` fallback nama tenant.
