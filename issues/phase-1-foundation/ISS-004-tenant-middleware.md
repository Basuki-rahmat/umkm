# ISS-004 — Middleware Resolusi Tenant (Subdomain/Domain)

- **Tag**: `[AI+riviu]` — keputusan konfigurasi host lokal perlu review manusia
- **Depends on**: ISS-003
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Middleware/`, `bootstrap/app.php` (registrasi middleware), `config/`, `tests/`

## Konteks

Sistem mengidentifikasi tenant dari subdomain (mis. `tokomaju.umkmku.id`) atau domain kustom (`tokomaju.com`). Middleware membaca host request, mencari `Tenant` by slug (subdomain) atau domain, lalu menaruhnya di `CurrentTenant` helper (dibuat ISS-003). Development lokal memakai `*.localhost` (Laravel Valet/herd) atau query param fallback untuk testing.

## Tasks

- [ ] Migration `add_domain_to_tenants_table`: kolom `domain` (string, nullable, unique) untuk domain kustom.
- [ ] Middleware `IdentifyTenant`:
  - Ambil host request. Abaikan host utama platform (daftar di `config/tenancy.php` baru: `central_domains` default `['localhost', '127.0.0.1']`).
  - Jika host cocok `tenants.domain` → pakai tenant itu.
  - Jika subdomain dari host utama → cocokkan `tenants.slug`.
  - Jika ditemukan → set ke `CurrentTenant`; jika tenant `SUSPENDED` → tampilkan halaman "akun ditangguhkan" (HTTP 403 + view sederhana) — perilaku disamakan dengan ISS-008, bukan 404.
  - Jika tidak ditemukan pada route non-admin → tetap lanjut tanpa tenant (halaman utama platform nanti).
- [ ] Fallback testing: query param `?tenant={slug}` hanya aktif saat `APP_ENV=local` (memudahkan test tanpa setup subdomain).
- [ ] Registrasikan middleware sebagai group `web`.
- [ ] Test: buat tenant slug `demo` → request dengan `?tenant=demo` → `currentTenantId()` mengembalikan id tenant demo; request tanpa tenant → `currentTenantId()` null, tidak error.

## Acceptance Criteria

- [ ] `php artisan test --filter=IdentifyTenant` (atau suite tenancy) lulus.
- [ ] `vendor/bin/phpstan` & `vendor/bin/pint --dirty` lolos.
- [ ] `config/tenancy.php` berisi `central_domains` yang mudah diubah.

## Jangan

- Jangan membuat routing konten website publik (fase berikutnya).
- Jangan hardcode domain di kode — semua via config/DB.
