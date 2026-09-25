# ISS-036 — Dashboard Owner Final

- **Tag**: `[AI+riviu]` — tata letak/kepadatan info perlu review manusia
- **Depends on**: ISS-035
- **Perkiraan**: 1 hari
- **Area**: `resources/views/admin/dashboard/`, `app/Http/Controllers/Admin/`, `tests/`

## Konteks

Menyelesaikan dashboard sesuai sketsa plan (#16): penjualan hari ini, pesanan aktif, piutang, grafik, transaksi terbaru. Widget piutang sudah nyata dari ISS-028; issue ini menyempurnakan sisanya.

## Tasks

- [ ] Kartu "Penjualan Hari Ini" (transaksi COMPLETED hari ini, vs kemarin ±%) & "Pesanan Aktif" (PENDING).
- [ ] Grafik 30 hari penjualan harian (Chart.js, line).
- [ ] Tabel "Transaksi Terbaru" (10 baris: nomor, pelanggan, total, status, waktu) + link detail.
- [ ] Widget "Piutang" final: total OPEN + tombol ke halaman piutang.
- [ ] Query ringan (< 100ms pada data demo): cache ringkas 5 menit bila perlu (`Cache::remember`).
- [ ] Layout mobile: kartu grid 1 kolom di HP, 2–4 kolom desktop.
- [ ] Test: angka kartu & transaksi terbaru benar; cache tidak menampilkan data tenant lain (key per tenant).

## Acceptance Criteria

- [ ] `php artisan test --filter=Dashboard` lulus.
- [ ] Manual dari HP: dashboard informatif & cepat.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan menambah metrik di luar sketsa plan (#16).
