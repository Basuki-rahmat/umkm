# ISS-076 — Struk Thermal (58/80mm) Kuliner

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-075
- **Perkiraan**: 1 hari
- **Area**: `resources/views/receipts/`, `app/Http/Controllers/Admin/`, `tests/`

## Konteks

Struk dapur/ kasir (plan #13): format thermal 58mm & 80mm via CSS print (bukan DomPDF — thermal lebih akurat dengan print HTML murni), plus struk PDF DomPDF sebagai cadangan digital (kirim via WA).

## Tasks

- [ ] Template struk thermal `receipts/thermal.blade.php`: lebar 58mm/80mm (pilih di settings `receipt_width`), font monospace, kop (nama, alamat, telepon), nomor, tipe order (DI TEMPAT/BUNGKUS/ANTAR + meja/alamat), items dengan varian/add-on/catatan per baris, total, bayar/kembalian (bila CASH), footer terima kasih.
- [ ] Route `/admin/culinary/orders/{id}/receipt?w=58|80` → print-friendly (window.print() otomatis via tombol).
- [ ] Halaman bayar CASH: input uang diterima → kembalian otomatis (murni JS, tidak disimpan kecuali di note transaksi).
- [ ] Struk digital PDF (DomPDF) + tombol kirim via WA (reuse WaLink) — teks struk disederhanakan.
- [ ] Tombol cetak ulang struk dari detail transaksi (audit log PRINT).
- [ ] Test: route 200 berisi nomor & items + varian; kembalian JS tidak error; PDF version valid; isolasi; audit PRINT.

## Acceptance Criteria

- [ ] `php artisan test --filter=Receipt` lulus.
- [ ] Manual: cetak struk 58mm dari HP → printer thermal menghasilkan struk rapi tidak terpotong.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan integrasi driver printer khusus (bluetooth/ESC-POS = backlog; print browser cukup).
- Jangan duplikasi data pembayaran.
