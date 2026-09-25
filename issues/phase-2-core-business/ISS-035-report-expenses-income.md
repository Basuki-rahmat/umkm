# ISS-035 — Laporan Pengeluaran & Pendapatan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-034
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Http/Controllers/Admin/Reports/`, `resources/views/admin/reports/`, `tests/`

## Konteks

Laporan terakhir inti: pendapatan = total pembayaran CONFIRMED − total pengeluaran per periode. Ringkasan keuangan sederhana plan (#20).

## Tasks

- [ ] "Laporan → Pengeluaran": per kategori & per hari, total periode, daftar detail dengan bukti (link file).
- [ ] "Laporan → Pendapatan": ringkasan periode — pemasukan (per metode), pengeluaran (per kategori), **laba bersih**; tabel bulanan 12 bulan terakhir (pemasukan, pengeluaran, net); grafik line net bulanan.
- [ ] Widget dashboard "Penjualan Hari Ini" & "Piutang" kini pakai angka nyata (finalize dari ISS-028).
- [ ] Test: laba bersih dihitung benar (seeded: pembayaran 1jt, pengeluaran 300rb → 700rb); pembayaran PENDING tidak masuk pemasukan; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Report|Income` lulus.
- [ ] Manual: dashboard menampilkan angka nyata yang cocok dengan laporan.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat accounting penuh (jurnal/neraca — fase lanjutan).
