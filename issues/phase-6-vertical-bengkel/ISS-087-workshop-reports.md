# ISS-087 — Laporan Bengkel (Per Mekanik, Jenis Servis, Sparepart Terlaris)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-086
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/Workshop/`, `resources/views/admin/reports/workshop/`, `tests/`

## Konteks

Laporan khusus bengkel (plan #12 Output): pendapatan per mekanik, per jenis servis, sparepart terlaris — dasar bonus & stok.

## Tasks

- [ ] "Laporan → Kinerja Bengkel": per periode — pendapatan per mekanik (dari service_orders.mechanic_id + transaction), jumlah servis, rata-rata nilai; per jenis servis (dari items SERVICE); sparepart terlaris (qty dari items SPAREPART).
- [ ] Efisiensi: durasi rata-rata SERVIS→QC per mekanik (dari `service_order_logs` — tabel yang dibuat di ISS-083; durasi = selisih created_at antar status).
- [ ] Export Excel (pola ISS-039); menu hanya tenant BENGKEL.
- [ ] Test: angka per mekanik/servis/sparepart benar vs seeded; durasi dihitung dari log; isolasi; menu guard.

## Acceptance Criteria

- [ ] `php artisan test --filter=WorkshopReport` lulus.
- [ ] Manual: laporan tampil & Excel terunduh; angka cocok dengan work order demo.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan sistem komisi/bonus otomatis (data saja, hitung manual oleh owner).
- Jangan laporan lintas tenant.
