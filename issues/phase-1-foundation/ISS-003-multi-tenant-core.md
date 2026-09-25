# ISS-003 — Multi-Tenant Core: Model Tenant & Trait BelongsToTenant

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-002
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Tenancy/`, `tests/`

## Konteks

Ini jantung arsitektur. Setiap data milik tenant wajib terisolasi otomatis. Pola: trait `BelongsToTenant` memasang **global scope** (filter semua query by `tenant_id`) dan mengisi `tenant_id` otomatis saat `create`. Referensi mekanisme ada di plan.md bagian 7, tapi cukup ikuti task di bawah.

## Tasks

- [ ] Migration `create_tenants_table`: `id`, `name`, `slug` (unique), `status` (enum: `ACTIVE`, `SUSPENDED`; default `ACTIVE`), `plan` (string, nullable — placeholder paket), `timestamps`.
- [ ] Model `Tenant` + cast yang sesuai.
- [ ] Migration: tambah kolom `tenant_id` (nullable foreignId, index, FK ke `tenants.id` nullOnDelete) ke tabel `users`.
- [ ] Buat trait `App\Tenancy\BelongsToTenant`:
  - `bootBelongsToTenant`: global scope `where tenant_id = currentTenantId()`.
  - `creating` event: isi `tenant_id` otomatis dari `currentTenantId()` bila belum diisi.
  - Relasi `tenant()`.
- [ ] Buat helper `currentTenantId()` (mis. di `App\Tenancy\CurrentTenant`) yang membaca dari container/session; untuk sekarang boleh mengembalikan `null` (akan diisi oleh middleware di ISS-004) — trait TIDAK boleh error saat `null` (scoped query dilewati; create tetap mengisi bila ada).
- [ ] Terapkan trait ke model `User`.
- [ ] Test `tests/Feature/TenancyTest.php`:
  - Dua tenant dengan masing-masing 1 user; saat `currentTenantId()` = tenant A, `User::all()` hanya berisi user tenant A.
  - `User::factory()->create()` saat `currentTenantId()` = A otomatis punya `tenant_id = A`.
  - Query eksplisit tenant B lewat `withoutGlobalScope` tetap bisa (untuk super admin nanti) — cukup assert scope aktif by default.

## Acceptance Criteria

- [ ] `php artisan migrate` sukses.
- [ ] `php artisan test --filter=TenancyTest` lulus semua.
- [ ] `vendor/bin/pint --dirty` dan `vendor/bin/phpstan` lolos.

## Jangan

- Jangan membuat middleware (ISS-004).
- Jangan menambah kolom lain di tenants (slug/status/plan sudah cukup).
