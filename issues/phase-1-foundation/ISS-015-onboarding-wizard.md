# ISS-015 — Onboarding Tenant (Alur Manual MVP)

- **Tag**: `[AI+riviu]` — UX alur pendaftaran perlu review manusia
- **Depends on**: ISS-014
- **Perkiraan**: 1 hari
- **Area**: `resources/views/onboarding/`, `app/Http/Controllers/Onboarding/`, `routes/`, `tests/`

## Konteks

Versi manual dari alur onboarding plan (#28): super admin membuat tenant via form terpandu (bukan onboarding mandiri — itu Phase 7). Hasil akhir: tenant + owner + settings dasar + siap login.

## Tasks

- [ ] Form `/platform/tenants/create` diperluas jadi wizard 2 langkah (1 form, section terpisah):
  - Langkah 1: nama usaha, slug, jenis usaha (KULINER/PERCETAKAN/BENGKEL — simpan ke `tenants.vertical` string nullable, migration tambahan), paket placeholder, status.
  - Langkah 2: nama owner, email, password (generate & tampil sekali + tombol copy).
- [ ] Setelah create: halaman sukses berisi kredensial owner (sekali render, diberi warning), tombol "Login sebagai owner" (impersonate sederhana: session flag yang hanya boleh dipakai SUPER ADMIN, log audit).
- [ ] Validasi slug otomatis unik + regex `[a-z0-9-]`.
- [ ] Test: wizard membuat tenant+owner+vertical tersimpan; impersonate mencatat audit log & hanya super admin.

## Acceptance Criteria

- [ ] `php artisan test --filter=Onboarding` lulus.
- [ ] Manual: buat tenant via wizard → login sbg owner → dashboard tampil.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat pembayaran/onboarding mandiri publik (Phase 7).
- Jangan buat konten website awal (Phase 3).
