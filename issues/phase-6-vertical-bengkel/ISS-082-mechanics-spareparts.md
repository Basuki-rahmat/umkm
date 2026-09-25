# ISS-082 — Bengkel: Mekanik & Sparepart

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-081
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/Workshop/`, `tests/`

## Konteks

Mekanik = staf yang mengerjakan servis (bukan user login wajib). Sparepart = produk core dengan detail bengkel — agar pembelian/stok/laporan core terpakai (plan #12).

## Tasks

- [ ] Migration `create_mechanics_table`: `id`, `tenant_id`, `name`, `specialist` nullable (mis. "Mesin", "Kelistrikan"), `is_active` bool default true, `user_id` nullable FK (bila mekanik juga punya akun panel), timestamps.
- [ ] Migration `create_sparepart_details_table`: `id`, `tenant_id`, `product_id` FK unique, `part_number` nullable, `compatibility` nullable (teks: merk/model), timestamps. Sparepart = produk core (stok & harga beli/jual dari core).
- [ ] Panel: CRUD mekanik (list, aktif/nonaktif); CRUD sparepart memakai form produk core + field detail; filter kategori "Sparepart" di produk.
- [ ] Seeder: 4 mekanik + 10 sparepart (oli, kampas rem, busi, filter) dengan stok.
- [ ] Test: CRUD; sparepart stok via mutasi core tetap bekerja; relasi user_id opsional; isolasi; menu guard.

## Acceptance Criteria

- [ ] `php artisan test --filter=Mechanic|Sparepart` lulus.
- [ ] Manual: beli sparepart via Pembelian (Phase 2) → stok naik → tampil di list sparepart.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan komisi mekanik (laporan saja, ISS-087).
- Jangan jadwal shift mekanik (backlog).
