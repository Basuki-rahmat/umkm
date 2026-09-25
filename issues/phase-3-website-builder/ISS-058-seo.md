# ISS-058 — SEO Dasar per Tenant

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-057
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Public/`, `resources/views/public/`, `routes/`, `tests/`

## Konteks

SEO dasar plan (#10): title/description per halaman, sitemap, robots, Open Graph. Target: website tenant mudah ditemukan & share rapi di WhatsApp (OG image penting — link WA adalah jalur utama visitor).

## Tasks

- [ ] Meta per halaman: title pattern `{halaman} — {nama usaha} | {kota}`; description: settings `meta_description` fallback deskripsi usaha; halaman produk/artikel pakai excerpt.
- [ ] Open Graph semua halaman: og:title, og:description, og:image (logo/featured), og:type, og:url; khusus produk & artikel og:image dari fotonya.
- [ ] `sitemap.xml` dinamis per tenant (home, produk, detail produk, artikel, galeri, kontak — hanya published/aktif) + `robots.txt` (allow semua, disallow /admin, /platform; Sitemap: URL lengkap).
- [ ] Canonical URL di semua halaman; URL bersih (slug, tanpa query redundant).
- [ ] Setting tenant: `meta_description` (maks 160 char, counter di form), `og_image` opsional (upload).
- [ ] Struktur heading benar (satu h1 per halaman, h2 section) — audit semua view publik.
- [ ] Test: sitemap berisi URL yang benar & hanya konten aktif; robots benar; OG tag lengkap dengan image benar; title pattern; draft tidak di sitemap.

## Acceptance Criteria

- [ ] `php artisan test --filter=Seo` lulus.
- [ ] Manual: view-source halaman home & produk → meta lengkap; `/sitemap.xml` & `/robots.txt` accessible.
- [ ] Preview share link di WA (manual) menampilkan gambar & judul rapi.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan structured data schema.org kompleks (opsional nanti).
- Jangan submit ke Search Console otomatis (manual oleh owner, dokumentasikan di docs).
