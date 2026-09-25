# ISS-067 — Surat Jalan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-066
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `resources/views/pdf/surat-jalan.blade.php`, `app/Http/Controllers/Admin/`, `tests/`

## Konteks

Dokumen pengiriman/serah terima (plan #11 Output): nomor unik `SJ/202609/0001`, dicetak saat pesanan SIAP diambil/dikirim.

## Tasks

- [ ] Migration `create_delivery_orders_table`: `id`, `tenant_id`, `transaction_id` FK, `production_order_id` nullable FK, `number` unique `(tenant_id, number)` (DocumentNumber prefix `SJ`), `delivery_method` (AMBIL_SENDIRI/KIRIM), `courier_name` nullable, `receiver_name` nullable, `receiver_signature_note` nullable, `delivered_at` datetime nullable, timestamps.
- [ ] Form buat surat jalan dari detail produksi/transaksi (pilih metode, kurir/kendaraan bila KIRIM) → simpan → cetak.
- [ ] PDF surat jalan (DomPDF, kop tenant): nomor, tanggal, pelanggan, tabel item + qty, kolom tanda terima (nama + ttd), footer alamat/kontak. Layout print-friendly 2 copy per halaman (eksemplar platform + pelanggan).
- [ ] Mark `delivered_at` saat status produksi → SELESAI bila ada surat jalan (atau tombol manual "Sudah diterima").
- [ ] List surat jalan (filter tanggal/kurir) di menu Percetakan.
- [ ] Test: nomor per tenant berurutan; PDF berisi nomor & item; default 1 SJ per produksi — buat ulang SJ untuk produksi sama ditolak kecuali override dengan konfirmasi (kasus revisi); isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=DeliveryOrder` lulus.
- [ ] Manual: buat SJ → PDF tercetak rapi 2 copy → tandai diterima.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan tracking online/kurir API (backlog).
- Jangan ubah alur pembayaran.
