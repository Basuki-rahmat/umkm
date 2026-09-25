# ISS-009 — Manajemen User Dalam Tenant

- **Tag**: `[ADMIN TENANT/OWNER]`
- **Depends on**: ISS-008
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/`, `resources/views/admin/users/`, `database/seeders/`, `tests/`

## Konteks

OWNER mengelola user di tenant-nya: menambah KASIR/STAFF/OPERATOR, mengubah role, menonaktifkan. User selalu terikat 1 tenant.

## Tasks

- [ ] Halaman list user tenant: kolom (nama, email, role, aktif, dibuat), search nama/email.
- [ ] Create user (nama, email unik, password, pilih role: OWNER/KASIR/STAFF/OPERATOR) — OWNER tidak bisa membuat user dengan role `SUPER ADMIN` atau `ADMIN TENANT`.
- [ ] Edit: nama, role (kecuali user OWNER terakhir di tenant tidak boleh di-turunkan/hapus).
- [ ] Toggle aktif/nonaktif (kolom `is_active` boolean di users) — nonaktif tidak bisa login.
- [ ] Reset password user oleh OWNER (set password baru, tanpa email di MVP).
- [ ] Seeder: user demo OWNER + 1 KASIR + 1 STAFF untuk tenant demo.
- [ ] Test: OWNER buat KASIR → login KASIR ok; nonaktifkan KASIR → login gagal; OWNER terakhir tak bisa diturunkan.

## Acceptance Criteria

- [ ] `php artisan test --filter=UserManagement` lulus.
- [ ] Manual: alur buat→login→nonaktifkan→gagal login berjalan.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan izinkan user lintas tenant (selalu `tenant_id` user login).
- Jangan buat role baru.
