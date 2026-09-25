# ISS-048 — Tabel `websites` & Panel Konfigurasi Website

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-047
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Models/`, `resources/views/admin/settings/`, `tests/`

## Konteks

Sesuai plan (#27 Template Engine): tabel `websites` menyimpan konfigurasi website per tenant — template_type, tema, section aktif, teks hero. Ini sumber kebenaran untuk semua halaman publik.

## Tasks

- [ ] Migration `create_websites_table`: `id`, `tenant_id` unique FK, `template_type` (enum `KULINER`, `PERCETAKAN`, `BENGKEL`), `theme` (string default `default`), `hero_title`, `hero_subtitle` nullable, `hero_cta_text` nullable, `sections` (json: array section aktif + urutan), `is_published` bool default false, timestamps.
- [ ] Migration `add_vertical_to_tenants` (jika belum ada dari ISS-015): sinkronkan default `template_type` dari `tenants.vertical` saat create website.
- [ ] Model `Website` + trait `BelongsToTenant` (CONVENTIONS §2.2) + relasi tenant + helper `website()` di Tenant.
- [ ] Panel admin → Pengaturan → tab "Website": pilih template, isi hero, centang section aktif (checkbox berurut), toggle `is_published` (OFF = halaman "segera hadir" dari ISS-046).
- [ ] Halaman publik membaca konfigurasi ini (hero diterapkan di home placeholder; section yang tidak aktif tidak dirender). Section yang aktif tapi tanpa data/implementasi juga **tidak merender wrapper kosong** — aturan global di ISS-050.
- [ ] Test: section nonaktif tidak muncul di HTML; `is_published=false` → halaman segera hadir; perubahan template_type mengubah daftar section yang tersedia; section aktif tanpa data tidak merender wrapper.

## Acceptance Criteria

- [ ] `php artisan test --filter=WebsiteSettings` lulus.
- [ ] Manual: centang/hilangkan section → website publik berubah sesuai.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat editor drag-drop (di luar MVP).
- Jangan render section khusus vertical (ISS-049).
