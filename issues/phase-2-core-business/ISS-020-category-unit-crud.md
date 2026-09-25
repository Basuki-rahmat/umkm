# ISS-020 — CRUD Kategori + Satuan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-019
- **Perkiraan**: 0.5 hari
- **Area**: `app/Http/Controllers/Admin/`, `resources/views/admin/categories/`, `database/migrations/`, `tests/`

## Konteks

Kategori dipakai produk & jasa (type PRODUCT/SERVICE). Satuan (pcs, kg, box, rim, dll) dipakai produk & item transaksi.

## Tasks

- [ ] Migration `create_units_table`: `id`, `tenant_id`, `name` (mis. "pcs"), unique `(tenant_id, name)`, timestamps.
- [ ] CRUD kategori (dipilah per type PRODUCT/SERVICE, dua tab) — pola sama dengan ISS-019.
- [ ] CRUD satuan (list sederhana + create + delete; tanpa edit nama agar konsistensi data aman, boleh hapus lalu buat baru).
- [ ] Seeder: kategori & satuan default untuk tenant demo (pcs, kg, box, rim, lembar).
- [ ] Test: CRUD + unique per tenant + isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Category|Unit` lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kaitkan ke produk (ISS-021).
