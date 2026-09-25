# ISS-001 — Project Setup Laravel

- **Tag**: `[AI-friendly]`
- **Depends on**: — (issue pertama)
- **Perkiraan**: 0.5–1 hari
- **Area**: `./` (repo root)

## Konteks

Proyek baru "Sistem Digitalisasi UMKM" — platform multi-tenant Laravel. Issue ini menyiapkan fondasi proyek. Eksekutor tidak perlu konteks lain; ikuti task persis.

## Tasks

- [ ] Buat proyek Laravel baru (versi 13) di root repo dengan nama aplikasi `"UMKM Digital"`.
- [ ] Konfigurasi `.env`: nama aplikasi, koneksi `mysql` (host `127.0.0.1`, db `umkm_digital`, user/password mengikuti environment lokal), `APP_TIMEZONE=Asia/Jakarta`, `APP_LOCALE=id`, `APP_FAKER_LOCALE=id_ID`.
- [ ] Buat `.env.example` yang sinkron dengan `.env` (tanpa nilai rahasia).
- [ ] Pastikan `.gitignore` standar Laravel sudah menutup `.env`, `/storage/*.key`, `/node_modules`.
- [ ] Jalankan migrasi bawaan (`users`, `cache`, `jobs`) dan pastikan sukses.
- [ ] Buat sketsa ERD core versi awal di `docs/erd.md` (tenants, users, roles, permissions, settings, products, transactions, dst sesuai plan §23) — diperbarui tiap kali schema bertambah.
- [ ] Git: `git init` (jika belum), commit awal.

## Acceptance Criteria

- [ ] `php artisan about` menunjukkan PHP 8.3+, timezone `Asia/Jakarta`, locale `id`.
- [ ] `php artisan migrate` sukses tanpa error.
- [ ] `git log` menunjukkan commit awal berisi seluruh skeleton.

## Jangan

- Jangan install package tambahan apa pun (ada di ISS-002).
- Jangan membuat model/migration custom.
