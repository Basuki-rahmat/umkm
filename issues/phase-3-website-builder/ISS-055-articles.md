# ISS-055 — Artikel/Blog (Panel + Halaman Publik)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-054
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Admin/`, `resources/views/public/articles/`, `tests/`

## Konteks

Artikel per tenant (plan #9) untuk SEO & promosi: promo kuliner, tips percetakan, edukasi servis bengkel.

## Tasks

- [ ] Migration `create_articles_table`: `id`, `tenant_id`, `title`, `slug` unique `(tenant_id, slug)`, `excerpt` nullable, `body` (text), `featured_image` nullable, `status` (enum `DRAFT`, `PUBLISHED`), `published_at` nullable, timestamps + Model `Article` (trait `BelongsToTenant` — CONVENTIONS §2.2).
- [ ] Panel: CRUD + editor textarea sederhana (Markdown-ish: newline → paragraph; tanpa WYSIWYG berat), set featured image dari galeri/upload, preview.
- [ ] Slug auto dari judul (unique per tenant, konflik → tambah suffix).
- [ ] Publik: list (kartu: gambar thumb, judul, excerpt, tanggal) + pagination; detail (gambar, isi, artikel terkait 3, CTA WA bawah).
- [ ] Section artikel di Home: 3 terbaru (bila section aktif).
- [ ] Menu "Artikel" muncul hanya bila ada artikel published.
- [ ] Test: CRUD + draft tidak tampil publik; slug unik per tenant; published_at terisi saat publish; isolasi; detail 404 untuk draft.

## Acceptance Criteria

- [ ] `php artisan test --filter=Article` lulus.
- [ ] Manual: buat artikel → tampil di list & detail dengan URL bersih.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan install CMS penuh (cukup CRUD ini).
- Jangan izinkan tag/script di body (escape saat render).
