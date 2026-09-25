# ISS-033 — Laporan Penjualan & Pembayaran

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-032
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/`, `resources/views/admin/reports/`, `app/Actions/Reports/`, `tests/`

## Konteks

Laporan pertama dari 7 laporan inti plan (#20). Pola laporan standar yang akan dipakai semua laporan lain: **filter periode (dari–sampai) + ringkasan atas + tabel + tombol Print**. Query wajib memakai agregasi DB (bukan koleksi PHP) agar scalable.

## Tasks

- [ ] Halaman "Laporan → Penjualan": filter rentang tanggal (default bulan ini) + pelanggan opsional.
  - Ringkasan: jumlah transaksi, total penjualan, total diskon, rata-rata per transaksi.
  - Tabel per hari: tanggal, jumlah transaksi, total; tabel detail opsional per transaksi.
  - Grafik bar 30 hari (Chart.js via CDN lokal).
- [ ] Halaman "Laporan → Pembayaran": ringkasan per metode (Cash/Transfer/QRIS), tabel per hari, daftar pembayaran dengan status.
- [ ] Semua query per-tenant (scope trait) + index tanggal sudah dipakai.
- [ ] Tombol Print (CSS print dari ISS-031).
- [ ] Test: data seeded dengan tanggal berbeda → angka ringkasan & per-hari benar; filter tanggal bekerja; tenant A tidak melihat data tenant B.

## Acceptance Criteria

- [ ] `php artisan test --filter=Report` lulus.
- [ ] Manual: angka laporan cocok dengan data demo.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan export Excel (buffer ISS-039).
- Jangan query N+1 (eager load; cek dengan `DB::enableQueryLog` di test bila ragu).
