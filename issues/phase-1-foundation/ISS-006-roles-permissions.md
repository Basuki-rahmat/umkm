# ISS-006 — Role & Permission (spatie/laravel-permission)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-005
- **Perkiraan**: 1 hari
- **Area**: `config/`, `database/migrations/`, `app/Models/`, `database/seeders/`, `tests/`

## Konteks

Role minimal sesuai plan: `SUPER ADMIN`, `ADMIN TENANT`, `OWNER`, `KASIR`, `STAFF`, `OPERATOR`. Peran penting sekarang: OWNER (pemilik tenant) dan SUPER ADMIN (pengelola platform, tanpa tenant).

## Tasks

- [ ] Install `spatie/laravel-permission`, jalankan migration bawaannya.
- [ ] Guard: guard default `web` untuk user tenant; **guard terpisah `platform`** (kustom guard) pada route `/platform/*` khusus `SUPER ADMIN` — sesuai CONVENTIONS §2.4 (akses lintas tenant hanya di panel SUPER ADMIN).
- [ ] Migration: hapus kolom sementara `role` di users jika ada (dari ISS-005).
- [ ] Seeder `RoleSeeder`: buat semua role di atas (idempotent — pakai `firstOrCreate`).
- [ ] Seeder akun platform: 1 user `SUPER ADMIN` pertama (email & password dari `.env` `PLATFORM_EMAIL`/`PLATFORM_PASSWORD`, idempotent `firstOrCreate`). Tanpa ini `/platform` tidak bisa dibuka setelah `migrate:fresh --seed` (dipakai test & AC manual ISS-008). `tenant_id` = null (user super admin tanpa tenant).
- [ ] Model `User`: trait `HasRoles`; saat register (ISS-005) assign role `OWNER`.
- [ ] Middleware helper: `role:OWNER` dsb. Terapkan pada route `/admin` → hanya user ber-role di luar `SUPER ADMIN` (punya tenant) boleh masuk; route `/platform/*` khusus `SUPER ADMIN`.
- [ ] Test: user OWNER tidak bisa akses `/platform/dashboard` (403); SUPER ADMIN bisa.

## Acceptance Criteria

- [ ] `php artisan db:seed --class=RoleSeeder` idempotent (boleh dijalankan 2x).
- [ ] `php artisan test --filter=Role` lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat UI manajemen role (ISS-009).
- Jangan pakai fitur teams bawaan spatie (over-engineering untuk MVP).
