# ISS-008 — Super Admin: Kelola Tenant (Panel Platform)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-007
- **Perkiraan**: 1 hari
- **Area**: `app/Filament/` atau `resources/views/platform/`, `app/Http/Controllers/Platform/`, `tests/`

## Konteks

SUPER ADMIN mengelola platform dari `/platform`. Fungsi pertama: CRUD tenant + aktivasi/suspend. Saat membuat tenant, sistem juga membuat akun owner pertamanya.

## Tasks

- [ ] Halaman list tenant: kolom (nama, slug, domain, status, paket, jumlah user, dibuat), search by nama/slug, filter status, pagination 25.
- [ ] Form create tenant: nama, slug (auto dari nama, bisa diedit, unik), status, paket (STARTER/BUSINESS/PRO placeholder), nama+email owner pertama.
- [ ] Simpan dalam `DB::transaction`: tenant baru + user owner (password acak, ditampilkan sekali setelah create / atau diisi manual) + assign role OWNER.
- [ ] Edit tenant (nama, slug, domain, status, paket) + aksi suspend/activate (konfirmasi).
- [ ] Saat tenant `SUSPENDED`: semua user tenant itu diblokir login (middleware di ISS-004 atau cek di login) → arahkan ke halaman "akun ditangguhkan".
- [ ] Test: create tenant membuat owner; suspend memblokir login user tenant; SUPER ADMIN saja yang bisa akses `/platform`.

## Acceptance Criteria

- [ ] `php artisan test --filter=Platform` lulus.
- [ ] Manual: buat tenant dari panel → login sbg owner tenant tsb → berhasil; suspend tenant → login ditolak.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan implementasi billing/paket nyata (Phase 7).
- Jangan buat onboarding otomatis (Phase 7).
