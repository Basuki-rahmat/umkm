# ISS-069 — Laporan Khusus Percetakan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-068
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/Print/`, `resources/views/admin/reports/print/`, `tests/`

## Konteks

Laporan spesifik vertical (plan #11 Output): penjualan & produksi per periode/jenis produk. Menambah menu Laporan hanya untuk tenant percetakan.

## Tasks

- [ ] "Laporan → Produksi Percetakan": per periode — jumlah pesanan, status akhir masing-masing, rata-rata waktu produksi (dari production_logs: MENUNGGU→SELESAI), pesanan telat count, per jenis produk cetak (dari detail json/kategori).
- [ ] "Laporan → Penjualan Percetakan": reuse laporan penjualan core + kolom tambahan (DP terkumpul vs sisa, metode ambil/kirim).
- [ ] Export Excel kedua laporan (reuse pola ISS-039).
- [ ] Menu laporan vertical hanya muncul di tenant PERCETAKAN (pattern menu conditional).
- [ ] Test: rata-rata waktu produksi benar (seeded log); pesanan telat akurat; angka DP/sisa cocok dengan pembayaran; isolasi; menu tersembunyi untuk tenant lain.

## Acceptance Criteria

- [ ] `php artisan test --filter=PrintReport` lulus.
- [ ] Manual: 2 laporan tampil dengan filter periode & export Excel bekerja.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan duplikasi query core — agregasi dari tabel production_*, payments, transactions.
- Jangan laporan lintas tenant.
