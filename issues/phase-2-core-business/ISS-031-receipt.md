# ISS-031 — Kwitansi & Nota Print-Friendly

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-030
- **Perkiraan**: 0.5–1 hari
- **Area**: `resources/views/pdf/`, `app/Http/Controllers/Admin/`, `routes/`, `tests/`

## Konteks

Kwitansi = bukti pembayaran (per payment), struk/nota = ringkasan transaksi untuk pelanggan. Keduanya print-friendly (bukan PDF lib — cukup CSS print) agar cepat dari HP + printer kasir.

## Tasks

- [ ] Route kwitansi per payment: `/admin/payments/{id}/receipt` — layout print-friendly (kop tenant, "Telah diterima dari", jumlah terbilang (helper `terbilang()`), metode, untuk pembayaran nomor transaksi X, ttd/stempel area).
- [ ] Route nota per transaksi: `/admin/transactions/{id}/nota` — ringkasan item + status pembayaran, layout ringkas setengah-halaman.
- [ ] CSS `@media print` (sembunyikan navigasi, ukuran kertas A5/nota, margin kecil).
- [ ] Tombol "Cetak Kwitansi"/"Cetak Nota" di halaman detail terkait.
- [ ] Helper `App\Support\Terbilang::make(1234.5): string` → "seribu dua ratus tiga puluh empat koma lima rupiah" + test beberapa kasus (0, 15, 1.250.000, desimal).
- [ ] Test: route 200 berisi nomor & jumlah; isolasi tenant; terbilang benar.

## Acceptance Criteria

- [ ] `php artisan test --filter=Receipt|Terbilang` lulus.
- [ ] Manual: buka dari HP → print preview rapi tanpa elemen panel.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan duplikasi data pembayaran ke tabel lain.
