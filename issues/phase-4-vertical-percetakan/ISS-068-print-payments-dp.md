# ISS-068 — Pembayaran Percetakan: DP & Pelunasan Saat Ambil

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-067
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Http/Controllers/Admin/`, `resources/views/admin/print/`, `tests/`

## Konteks

Kebiasaan percetakan (plan #11 Piutang): DP saat order, pelunasan saat ambil. Semua mekanisme sudah ada di core (ISS-027/028) — issue ini menambah UX percetakan di atasnya: shortcut & angka DP otomatis. Sesuai `docs/state-machine-core-vertical.md` §8: DP direkam saat transaksi `PENDING` dan receivable langsung `OPEN` (sisa), tanpa menunggu transaksi ditutup.

## Tasks

- [ ] Saat create transaksi percetakan: field opsional "DP %" (default 50) → otomatis membuat payment pertama (CASH/TRANSFER) sesuai persentase + receivable untuk sisanya (memakai action core, bukan logika baru).
- [ ] Halaman produksi & detail transaksi menampilkan: total, sudah dibayar, sisa — dengan tombol "Bayar Sisa" langsung (prefill amount = sisa).
- [ ] Guard: pesanan SIAP diambil dengan sisa belum lunas → warning kuning di halaman produksi + halaman ambil (jangan blok — keputusan kasir), audit log jika diambil dengan sisa.
- [ ] Kwitansi DP & kwitansi pelunasan otomatis tersedia (reuse ISS-031) — pastikan tampil untuk metode percetakan.
- [ ] Test: DP 50% saat transaksi `PENDING` → payment CONFIRMED + receivable `OPEN` (sisa 50%) & status transaksi tidak berubah; tombol bayar sisa prefill benar; warning muncul tepat; regressi core pembayaran tetap hijau.

## Acceptance Criteria

- [ ] `php artisan test --filter=PrintPayment|Payment` lulus.
- [ ] Manual: order dengan DP → produksi jalan → ambil + bayar sisa → kwitansi 2x tercetak.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat sistem pembayaran baru (murni UX di atas core).
- Jangan izinkan DP > total.
