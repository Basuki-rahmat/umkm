# ISS-047 — Layout Template Publik (Mobile-First)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-046
- **Perkiraan**: 1 hari
- **Area**: `resources/views/public/`, `public/css/` (atau build Tailwind), `tests/`

## Konteks

Layout dasar semua halaman publik tenant: header (logo, menu), konten, footer (alamat, kontak, sosmed dari settings), mobile-first sesuai plan (#9, #36).

## Tasks

- [ ] Layout blade `layouts/public.blade.php`: header sticky dengan logo tenant + menu (beranda, produk, galeri, artikel, kontak — item difilter oleh section yang aktif dari issue template nanti; sementara semua), tombol "Hubungi Kami" (wa.me), footer lengkap dari settings.
- [ ] Font & warna dari CSS variables (akan di-overwrite tema per tenant di ISS-050).
- [ ] Meta dasar per halaman: title `{halaman} — {nama usaha}`, description dari settings, favicon dari logo.
- [ ] Navigasi mobile: hamburger drawer sederhana (Alpine.js), tombol WA mengapung (placeholder, diisi ISS-057).
- [ ] Performa: font system stack dulu (tanpa webfont eksternal), gambar lazy-load.
- [ ] Test: halaman home (placeholder) merender layout dengan nama usaha & footer settings; menu mobile ada di HTML.

## Acceptance Criteria

- [ ] `php artisan test --filter=PublicLayout` lulus.
- [ ] Manual di HP 360px: header/footer rapi, drawer terbuka, tanpa scroll horizontal.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat halaman konten (issue berikutnya).
- Jangan hardcode warna/teks tenant di layout — semua dari settings/CSS var.
