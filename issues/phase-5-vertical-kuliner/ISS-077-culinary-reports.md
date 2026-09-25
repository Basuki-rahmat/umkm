# ISS-077 — Laporan Kuliner (Per Menu/Varian/Tipe/Jam Sibuk)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-076
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/Culinary/`, `resources/views/admin/reports/culinary/`, `tests/`

## Konteks

Laporan khusus kuliner (plan #13): per menu, per varian, per tipe order, jam sibuk — agregasi dari transaction_items (json varian/add-on) + culinary_orders.

## Tasks

- [ ] "Laporan → Penjualan Menu": per menu (qty, omzet) per periode; expand per varian; add-on omzet terpisah; menu terlaris ranking; export Excel (pola ISS-039).
- [ ] "Laporan → Tipe Order": DI TEMPAT vs BUNGKUS vs ANTAR (qty + omzet + rata-rata nilai); per hari grafik.
- [ ] "Laporan → Jam Sibuk": heatmap sederhana (hari × jam, dari transaksi per tenant) — tabel warna CSS, tanpa library grafik berat.
- [ ] Menu laporan kuliner hanya tenant KULINER (pattern guard Phase 4).
- [ ] Parsing detail json item yang konsisten (helper tunggal) — laporan tidak baca raw json di SQL native selain kolom biasa (qty/omzet dari kolom; varian di-parse di PHP setelah query terfilter periode — dokumentasikan trade-off).
- [ ] Test: omzet per menu & varian benar vs data seeded; jam sibuk benar; isolasi; menu tersembunyi tenant lain.

## Acceptance Criteria

- [ ] `php artisan test --filter=CulinaryReport` lulus.
- [ ] Manual: 3 laporan tampil & Excel terunduh.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan query JSON penuh di SQL (portabilitas MySQL/MariaDB berbeda) — filter kolom biasa, parse json di PHP.
- Jangan laporan lintas tenant.
