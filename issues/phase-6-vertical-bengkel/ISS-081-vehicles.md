# ISS-081 — Bengkel: Master Kendaraan Pelanggan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-080 (Phase 5 selesai; core tenant bengkel)
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/Workshop/`, `resources/views/admin/workshop/vehicles/`, `tests/`

## Konteks

Vertical ketiga: bengkel (plan #12). Riwayat servis terikat KENDARAAN, bukan cuma pelanggan. Menu "Bengkel" hanya tenant `template_type = BENGKEL`.

## Tasks

- [ ] Migration `create_vehicles_table`: `id`, `tenant_id`, `customer_id` FK (pelanggan core), `plate_number` (string, uppercase), `brand`, `model` nullable, `year` int nullable, `color` nullable, `notes` nullable, timestamps, softDeletes; unique `(tenant_id, plate_number)`.
- [ ] Panel CRUD kendaraan: list (search plat/nama pelanggan), create/edit — pilih pelanggan (searchable) atau buat baru inline; form memformat plat otomatis uppercase.
- [ ] Halaman detail kendaraan: data + riwayat servis (kosong dulu, diisi ISS-085) + tombol WA ke pemilik.
- [ ] Seeder bengkel demo: 8 pelanggan + 10 kendaraan (plat realistis format Indonesia).
- [ ] Test: unique plat per tenant (tenant lain boleh plat sama — beda daerah/kasus khusus); uppercase otomatis; search; isolasi; menu tersembunyi tenant lain.

## Acceptance Criteria

- [ ] `php artisan test --filter=Vehicle` lulus.
- [ ] Manual: tambah kendaraan → muncul di detail pelanggan juga (relasi dua arah).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan history servis di issue ini (ISS-085).
- Jangan tipe kendaraan kompleks (motor/mobil flag = cukup field notes/brand).
