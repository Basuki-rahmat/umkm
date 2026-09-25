# ISS-005 — Authentication (Login, Register, Reset Password)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-004
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Http/Controllers/Auth/`, `resources/views/auth/`, `routes/`, `tests/`

## Konteks

Panel admin butuh autentikasi standar. User sudah terikat tenant (kolom `tenant_id` dari ISS-003). Register di MVP ini khusus membuat **akun owner untuk tenant baru** (pendaftaran tenant manual oleh super admin datang di ISS-008; register mandiri sederhana cukup sekarang).

## Tasks

- [ ] Flow lengkap: Login, Logout, Register (nama, email, password, nama usaha → membuat `Tenant` + user OWNER), Forgot/Reset password. Boleh pakai Laravel Breeze (blade) atau buat manual — konsisten dengan CONVENTIONS (Blade + Tailwind).
- [ ] Halaman auth memakai layout minimal rapi (Tailwind), teks Indonesia.
- [ ] Saat register: bungkus `DB::transaction` — buat tenant (slug unik dari nama usaha) + user dengan `tenant_id` tenant baru, role OWNER (role diisi sementara via kolom string `role` di users, diperbaiki di ISS-006; skip jika ISS-006 sudah selesai lebih dulu → pakai spatie langsung).
- [ ] Redirect setelah login ke `/admin` (dashboard dummy dulu).
- [ ] Test:
  - Register membuat tenant + user + `tenant_id` terisi.
  - Login sukses → redirect `/admin`.
  - Reset password mengirim notifikasi (pakai `Notification::fake`).

## Acceptance Criteria

- [ ] `php artisan test --filter=Auth` lulus.
- [ ] Manual: daftar → login → logout → reset password berjalan di browser.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat verifikasi email (tidak dipakai MVP).
- Jangan ubah logika tenancy dari ISS-003.
