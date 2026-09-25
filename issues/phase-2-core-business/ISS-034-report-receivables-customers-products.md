# ISS-034 — Laporan Piutang, Pelanggan & Produk

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-033
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/`, `resources/views/admin/reports/`, `tests/`

## Konteks

Tiga laporan lagi memakai pola yang sama dengan ISS-033 (pola sudah matang — tinggal meniru).

## Tasks

- [ ] "Laporan → Piutang": daftar OPEN per pelanggan (nama, nomor, sisa, umur, due date), ringkasan total + grafik umur (0–7, 8–30, 31–60, >60 hari), tombol WA pengingat per baris (reuse ISS-032).
- [ ] "Laporan → Pelanggan": per pelanggan: jumlah transaksi, total belanja, terakhir transaksi; sort total terbesar; klik → riwayat transaksinya.
- [ ] "Laporan → Produk": per produk: qty terjual, total omzet (dari transaction_items item_type PRODUCT), jasa omzet terpisah; sort omzet terbesar.
- [ ] Test: angka agregasi benar vs data seeded; isolasi tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Report` lulus.
- [ ] Manual: 3 laporan tampil dengan filter & print.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan duplikasi kode filter antar laporan — buat trait/shared controller helper.
