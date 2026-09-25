# ISS-017 — Stabilisasi Akhir Phase 1

- **Tag**: `[AI+riviu]` — keputusan siap/lanjut perlu manusia
- **Depends on**: ISS-016
- **Perkiraan**: 1 hari
- **Area**: seluruh repo (hanya perbaikan, tanpa fitur baru)

## Konteks

Gerbang penutup Phase 1. Target: codebase bersih, semua test hijau, siap masuk Phase 2. Jika ada item gagal, perbaiki dalam issue ini.

## Tasks

- [ ] Jalankan `vendor/bin/pint --dirty` dan `vendor/bin/phpstan` → 0 error (naikkan level phpstan +1 bila mudah, opsional).
- [ ] `php artisan test` → seluruh suite hijau. Test yang flaky diperbaiki, bukan di-skip.
- [ ] Review `git status` → tidak ada file stray (console.log, dump, debug leftover).
- [ ] Cek manual cepat (checklist di bawah).
- [ ] Buat/refresh seeder "demo lengkap": 2 tenant (1 percetakan demo "Percetakan Lampung", 1 kuliner "Warung Maju Jaya") + user OWNER/KASIR/STAFF + settings terisi + logo placeholder. Perintah: `php artisan migrate:fresh --seed` harus menghasilkan state demo yang usable.
- [ ] Update status di `issues/README.md` → `Phase 1: 17/17 selesai`.
- [ ] Tulis ringkasan Phase 1 di `docs/phase-1-summary.md`: apa yang sudah ada, keputusan arsitektur yang diambil (Filament vs Blade), kendala, rekomendasi Phase 2.

## Acceptance Criteria (checklist manual)

- [ ] `php artisan migrate:fresh --seed` berjalan bersih dari nol.
- [ ] Login sbg OWNER percetakan → dashboard, user, settings, logo tampil.
- [ ] Login sbg KASIR → menu terbatas sesuai role.
- [ ] Akses tenant lain via ID langsung → 403/404.
- [ ] Tenant suspended → login diblokir.
- [ ] Audit log mencatat login & perubahan.
- [ ] `backup:run` + `backup:clean` jalan.
- [ ] Semua test + static analysis hijau.

## Jangan

- Jangan menambah fitur baru apa pun.
- Jangan refactor besar — hanya perbaikan yang diperlukan untuk lolos kriteria.
