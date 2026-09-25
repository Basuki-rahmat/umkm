# ISS-002 — QA Tooling, Storage per Tenant & Smoke Test

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-001
- **Perkiraan**: 0.5–1 hari
- **Area**: `composer.json`, `phpstan.neon`, `storage/`, `tests/`, `.github/workflows/` (opsional)

## Konteks

Menyiapkan alat kualitas yang dipakai semua issue berikutnya (pint, larastan, test) dan struktur storage per tenant. Setelah issue ini, "Definisi Selesai" di CONVENTIONS.md menjadi bisa dipenuhi.

## Tasks

- [ ] Install: `laravel/pint`, `larastan/larastan` (set level 5 di `phpstan.neon`), dan PHPUnit/Pest bawaan (pilih satu; catat pilihannya di laporan).
- [ ] Buat helper `TenantStorage` (class sederhana, mis. `app/Support/TenantStorage.php`) dengan method `path(int $tenantId, string $relative = ''): string` yang mengembalikan `storage/app/public/tenant-{id}/<relative>`.
- [ ] Jalankan `php artisan storage:link`.
- [ ] Buat 1 smoke test (`tests/Feature/SmokeTest.php`): homepage mengembalikan status 200/redirect, dan `TenantStorage::path(1, 'logo')` mengembalikan string berakhiran `tenant-1/logo`.
- [ ] (Opsional) Workflow GitHub Actions: `pint --test`, `phpstan`, `php artisan test`.

## Acceptance Criteria

- [ ] `vendor/bin/pint --test` lolos.
- [ ] `vendor/bin/phpstan` lolos (level 5).
- [ ] `php artisan test` lulus (minimal smoke test).
- [ ] Folder `storage/app/public/tenant-1/logo` dapat dibuat via helper (test membuktikannya).

## Jangan

- Jangan mengubah konfigurasi aplikasi lain.
- Jangan menambah paket di luar daftar task.
