# ISS-079 — Buffer: Meja & QR Order (Modal Fitur Lanjutan)

- **Tag**: `[AI+riviu]` — keputusan rilis fitur publik perlu manusia
- **Depends on**: ISS-078
- **Perkiraan**: 1–1.5 hari (opsional; boleh dilewati tanpa menghalangi ISS-080)
- **Area**: `database/migrations/`, `app/Http/Controllers/Public/`, `resources/views/public/qr-order/`, `tests/`

## Konteks

Buffer plan checklist Phase 5: QR order per meja. Modal kecil untuk fitur lanjutan — kalau tidak sempat, skip issue ini (tidak menghalangi stabilisasi).

## Tasks

- [ ] Migration `create_dining_tables_table`: `id`, `tenant_id`, `name`/`number`, `qr_token` unique, `is_active`, timestamps.
- [ ] Panel: CRUD meja + cetak kartu QR (URL publik `/{tenant}/order/{qr_token}` — QR digenerate pakai library ringan `simplesoftwareio/simple-qrcode` atau setara).
- [ ] Halaman publik QR order: katalog menu (reuse katalog publik), pilih item (varian/add-on), catatan, nama → simpan sebagai culinary order `DI_TEMPAT` dengan `table_number` dari meja + flag `source = QR` (kolom enum `source` sudah ada di culinary_orders, default `KASIR` — lihat ISS-074) — masuk ke panel & dapur seperti order kasir.
- [ ] Pembayaran tetap manual di kasir (order QR = pesanan, bukan checkout online).
- [ ] Rate limit + validasi meja aktif; anti-spam sederhana.
- [ ] Test: order QR masuk ke dapur dengan nomor meja benar; meja nonaktif ditolak; rate limit; isolasi token per tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=QrOrder` lulus.
- [ ] Manual: scan QR dari HP (kamera) → order → muncul di dapur.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pembayaran online di halaman QR (Phase 7).
- Jangan session login untuk pelanggan.
