# ISS-052 — Testimoni (Panel + Section Home)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-051
- **Perkiraan**: 0.5 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Admin/`, `resources/views/public/sections/`, `tests/`

## Konteks

Section Testimoni di Home (plan #9). MVP: input manual oleh owner (tidak ada submit publik — hindari spam).

## Tasks

- [ ] Migration `create_testimonials_table`: `id`, `tenant_id`, `customer_name`, `message` (text, maks 500 char), `rating` tinyint 1–5 nullable, `photo_path` nullable (foto pelanggan opsional), `is_visible` bool default true, `sort_order`, timestamps + Model `Testimonial` (trait `BelongsToTenant` — CONVENTIONS §2.2).
- [ ] Panel: CRUD sederhana + toggle visible + urutan.
- [ ] Section Home "Testimoni": card dengan nama, bintang (jika ada), pesan; carousel geser sederhana di mobile (Alpine) atau grid statis di desktop.
- [ ] Section hanya render jika aktif di `websites.sections` DAN ada testimoni visible.
- [ ] Test: CRUD; invisible tidak tampil; isolasi; section kosong tidak merender wrapper kosong.

## Acceptance Criteria

- [ ] `php artisan test --filter=Testimonial` lulus.
- [ ] Manual: tambah 3 testimoni → tampil di home tenant demo.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat form submit testimoni publik.
- Jangan moderasi kompleks (cukup toggle).
