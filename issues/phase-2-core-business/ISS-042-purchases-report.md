# ISS-042 — Integrasi Laporan Pembelian ke Laporan Pendapatan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-041
- **Perkiraan**: 0.5 hari
- **Area**: `app/Http/Controllers/Admin/Reports/`, `resources/views/admin/reports/`, `tests/`

## Konteks

Setelah pembelian ada, laporan pendapatan (ISS-035) kini harus mencerminkan realita: pengeluaran operasional + pembelian stok sebagai komponen biaya terpisah (tidak dicampur).

## Tasks

- [ ] Laporan "Pembelian": per periode/supplier/produk — qty dibeli, nilai pembelian.
- [ ] Update "Laporan → Pendapatan": tambah baris "Pembelian stok" terpisah dari "Pengeluaran operasional"; laba kotor = pemasukan − pembelian; laba bersih = laba kotor − pengeluaran operasional. Pertahankan angka lama tetap kompatibel (pembelian lama = 0).
- [ ] Export Excel ikut diperbarui (reuse).
- [ ] Test: angka laba kotor/bersih benar dengan data pembelian + pengeluaran campuran; regressi laporan lama tetap hijau.

## Acceptance Criteria

- [ ] `php artisan test --filter=Report|Income` lulus (termasuk regressi).
- [ ] Manual: laporan pendapatan menampilkan 2 komponen biaya terpisah.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan ubah definisi laporan lain.
