# ISS-101 — Onboarding Mandiri (Daftar → Paket → Domain → Bayar → Aktif)

- **Tag**: `[AI+riviu]` — UX pendaftaran publik perlu review manusia
- **Depends on**: ISS-100
- **Perkiraan**: 1.5–2 hari
- **Area**: `app/Http/Controllers/Saas/Onboarding/`, `resources/views/onboarding/`, `routes/`, `tests/`

## Konteks

Alur mandiri plan (#28): DAFTAR → pilih jenis usaha → pilih template → isi data usaha → pilih domain/subdomain → bayar → aktivasi. Website online tanpa campur tangan platform. Layanan mandiri ini dilindungi rate limit & verifikasi email (fitur pertama yang memakai email verifikasi — pakai Laravel bawaan).

## Tasks

- [ ] Wizard multi-step (session state, 5 langkah, dapat dilanjutkan):
  1. Akun: nama, email (verifikasi link), password, telepon.
  2. Usaha: nama usaha, jenis (KULINER/PERCETAKAN/BENGKEL), deskripsi singkat, alamat, telepon usaha.
  3. Paket: kartu 3 paket + trial 14 hari (default) atau langsung bayar setup.
  4. Domain: pilih subdomain (AJAX availability — ISS-099) atau domain kustom (hanya PRO → instruksi).
  5. Review & bayar: ringkasan → (a) mulai TRIAL (langsung provision) atau (b) bayar via gateway (invoice setup → provision otomatis via webhook ISS-098).
- [ ] Provisioning memakai action ISS-098 (jangan duplikasi); email selamat datang + panduan singkat (mailable, antre).
- [ ] Rate limit pendaftaran per IP + honeypot + verifikasi email sebelum provision (guard di webhook/provision: email verified).
- [ ] Landing page (ISS-016) tombol CTA → onboarding.
- [ ] Test: alur trial sampai provision lengkap (email verified dulu); alur gateway (webhook mock) → provision; wizard resume; rate limit; slug/domain flow; tenant aktif punya konten awal.

## Acceptance Criteria

- [ ] `php artisan test --filter=Onboarding` lulus.
- [ ] Manual dari HP: daftar → trial → tenant & website live dengan konten awal, tanpa intervensi platform.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan duplikasi logic provisioning (harus lewat action ISS-098).
- Jangan provision tanpa email verified.
- Jangan hapus onboarding manual super admin (tetap untuk bantu pelanggan offline).
