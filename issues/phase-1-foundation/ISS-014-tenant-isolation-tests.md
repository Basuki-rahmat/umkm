# ISS-014 — Uji Isolasi Tenant End-to-End

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-013
- **Perkiraan**: 0.5–1 hari
- **Area**: `tests/Feature/Isolation/`, perbaikan bug yang ditemukan

## Konteks

Persyaratan keamanan utama: **tidak boleh ada tenant melihat data tenant lain**. Issue ini menulis test isolasi menyeluruh pada SEMUA resource yang ada di titik ini (user, setting) — dan akan ditiru polanya di fase berikutnya. Test ini adalah gerbang wajib sebelum Phase 2.

## Tasks

- [ ] Buat trait test `ActsAsTenantA` / `ActsAsTenantB` (atau helper equivalent) yang membuat 2 tenant + user OWNER masing-masing, login sbg tenant A.
- [ ] Test untuk setiap resource tenant yang ada:
  - `GET /admin/users` → hanya user tenant A terlihat (tidak ada email milik tenant B di HTML).
  - Edit user milik tenant B via ID langsung (`/admin/users/{id-b}`) → 403/404.
  - `GET /admin/settings` → menampilkan nilai tenant A, bukan tenant B.
  - API/form update resource tenant B → 403/404 dan data tidak berubah di DB.
- [ ] Test data factory: setiap factory yang membuat model tenant WAJIB otomatis dapat `tenant_id` (assert).
- [ ] Perbaiki setiap kebocoran yang ditemukan (bug fix termasuk scope issue ini).
- [ ] Laporan: daftar resource yang diuji + hasil.

## Acceptance Criteria

- [ ] `php artisan test --filter=Isolation` lulus semua.
- [ ] `php artisan test` (suite penuh) lulus.
- [ ] Dokumen `docs/isolation-checklist.md` berisi tabel resource vs status test.

## Jangan

- Jangan skip test dengan alasan "nanti diperbaiki" — kebocoran = bug P0, perbaiki sekarang.
