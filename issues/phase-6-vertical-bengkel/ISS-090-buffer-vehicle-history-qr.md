# ISS-090 — Buffer: QR Riwayat Servis Pelanggan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-089 (atau ISS-088 bila ISS-089 dilewati)
- **Perkiraan**: 0.5–1 hari (opsional; tidak menghalangi ISS-091)
- **Area**: `app/Http/Controllers/Public/`, `resources/views/public/vehicle-history/`, `tests/`

## Konteks

Buffer plan Phase 6: pelanggan melihat riwayat servis kendaraannya via QR/link (transparansi → kepercayaan). Tanpa login: signed URL per kendaraan.

## Tasks

- [ ] Signed URL per kendaraan (Laravel signed routes, kedaluwarsa panjang 6 bulan, regenerate manual oleh bengkel) — halaman publik read-only: data kendaraan + timeline servis ringkas + bengkel kontak.
- [ ] QR digenerate untuk URL tsb (reuse library QR dari ISS-079; kalau ISS-079 dilewati, install library sama di sini).
- [ ] Panel: tombol "QR Riwayat" di detail kendaraan → tampil QR (bisa dicetak untuk stiker mobil/garasi) + tombol reset token.
- [ ] Halaman publik menolak URL invalid/expired dengan halaman sopan.
- [ ] Test: URL valid menampilkan riwayat benar; expired/invalid → 403 sopan; reset token membatalkan URL lama; tidak menampilkan data tenant lain; hanya kendaraan SELESAI yang tampil.

## Acceptance Criteria

- [ ] `php artisan test --filter=VehicleHistoryQr` lulus.
- [ ] Manual: scan QR dari HP → riwayat tampil rapi (mobile-first).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan login/akun pelanggan (signed URL cukup).
- Jangan tampilkan harga beli/internal di halaman publik — hanya ringkasan servis & biaya yang dibayar pelanggan.
