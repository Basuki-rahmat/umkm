# ISS-051 — Galeri Foto (Panel + Halaman Publik)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-050
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/`, `resources/views/public/gallery/`, `tests/`

## Konteks

Galeri per tenant (plan #9): upload multi-foto, urutan tampil, keterangan. Dipakai halaman Galeri + section galeri di Home + Tentang.

## Tasks

- [ ] Migration `create_galleries_table`: `id`, `tenant_id`, `path`, `caption` nullable, `sort_order` int default 0, `is_featured` bool default false, timestamps + Model `Gallery` (trait `BelongsToTenant` — CONVENTIONS §2.2).
- [ ] Panel: upload multi-file sekaligus (drag area sederhana), daftar thumbnail dengan edit caption, geser urutan (tombol ↑↓ cukup), tandai featured (max 3 featured → dipakai Tentang), hapus.
- [ ] Validasi via `AllowedUpload` (gambar maks 5MB per file — default ISS-012) → simpan `tenant-{id}/galleries/`.
- [ ] Halaman publik `/galeri`: grid responsif (masonry ringan / grid 2–3 kolom), lightbox sederhana (Alpine, tanpa library berat), caption di bawah.
- [ ] Section galeri di Home: 6 foto terbaru.
- [ ] Test: upload multi → semua tersimpan folder tenant benar; urutan berubah sesuai; featured max 3 (yang ke-4 ditolak); isolasi; halaman publik hanya tampil milik tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Gallery` lulus.
- [ ] Manual dari HP: upload 3 foto → tampil di galeri & home.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kompresi gambar di issue ini (ISS-053 khusus).
- Jangan video (gambar saja).
