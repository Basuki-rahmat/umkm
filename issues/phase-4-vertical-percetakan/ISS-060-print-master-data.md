# ISS-060 — Master Data Percetakan: Bahan, Ukuran, Finishing

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-059 (Phase 3 selesai)
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/Print/`, `resources/views/admin/print/`, `tests/`

## Konteks

Vertical percetakan dimulai. Tiga entitas spesifik percetakan (plan #11): bahan cetak, ukuran, finishing — semua per tenant, hanya tampil di panel tenant dengan `vertical = PERCETAKAN` (sumber kebenaran vertical; `websites.template_type` hanya sinyal website, bukan sumber menu).

## Tasks

- [ ] Migration `create_print_materials_table`: `id`, `tenant_id`, `name` (mis. "Vinyl 280gr", "Art Carton 260gr"), `unit_id` nullable FK units, `base_cost` decimal(16,2), `stock` decimal(12,2) default 0 nullable (bahan roll opsional — INFORMASIONAL, dikelola manual owner, tidak dipotong produksi/transaksi), `is_active`, timestamps, softDeletes; unique `(tenant_id, name)`.
- [ ] Migration `create_print_sizes_table`: `id`, `tenant_id`, `name` (mis. "A3+", "3x1 m"), `length` decimal(10,2) nullable, `width` decimal(10,2) nullable, `dimension_unit` (enum `CM`, `M`, nullable), `is_active`, timestamps; unique `(tenant_id, name)`.
- [ ] Migration `create_print_finishings_table`: `id`, `tenant_id`, `name` (mis. "Laminating Glossy", "Mata ayam"), `additional_price` decimal(16,2) default 0, `description` nullable, `is_active`, timestamps.
- [ ] CRUD ketiganya (menu "Percetakan" di sidebar dengan submenu Bahan/Ukuran/Finishing — hanya visible jika `tenants.vertical = PERCETAKAN`).
- [ ] Seeder demo percetakan: 6 bahan, 8 ukuran, 5 finishing.
- [ ] Test: CRUD + unique per tenant + isolasi + menu tersembunyi untuk tenant non-percetakan.

## Acceptance Criteria

- [ ] `php artisan test --filter=PrintMaster` lulus.
- [ ] Manual: tenant percetakan melihat menu; tenant kuliner tidak.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat kalkulasi harga (ISS-061).
- Jangan ubah modul core.
