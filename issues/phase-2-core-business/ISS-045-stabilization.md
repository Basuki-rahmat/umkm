# ISS-045 — Stabilisasi Akhir Phase 2

- **Tag**: `[AI+riviu]` — keputusan siap ke Phase 3 perlu manusia
- **Depends on**: ISS-044
- **Perkiraan**: 1 hari
- **Area**: seluruh repo (perbaikan saja), `docs/`, `issues/README.md`

## Konteks

Gerbang penutup Phase 2. Kriteria: seluruh alur uang & stok konsisten, laporan akurat, siap dibangun website publik (Phase 3) di atas data yang benar.

## Tasks

- [ ] Suite penuh hijau: `php artisan test`, `vendor/bin/phpstan`, `vendor/bin/pint --dirty`.
- [ ] `migrate:fresh --seed` → state demo lengkap: kedua tenant punya transaksi beragam (lunas, parsial/piutang, dibatalkan), pembayaran (cash/transfer/qris; sebagian parsial), invoice, pengeluaran, pembelian → semua laporan berisi angka masuk akal. Perluas seeder ISS-024 dengan transaksi dari akun KASIR / tanggal berbeda (bukan hanya hari ini) agar laporan per-periode teruji.
- [ ] Walkthrough manual lengkap (checklist di bawah) dari HP.
- [ ] Update `issues/README.md` → `Phase 2: 28/28 selesai`.
- [ ] Tulis `docs/phase-2-summary.md`: fitur selesai, keputusan teknis (mis. invoice tanpa tabel items, refund via tabel terpisah), hasil performa, feedback UAT yang masuk backlog Phase 4.

## Acceptance Criteria (checklist manual end-to-end)

- [ ] Kasir: buat transaksi multi-item dari HP → bayar parsial → lunas → nota tercetak.
- [ ] Invoice PDF terbit otomatis & terkirim via wa.me (klik).
- [ ] Piutang muncul otomatis, umur benar, lunas → settled.
- [ ] Refund penuh 1 transaksi → semua status konsisten.
- [ ] Pembelian menambah stok; laporan pembelian & pendapatan (2 komponen biaya) benar.
- [ ] 7 laporan inti + export Excel berfungsi dengan filter periode.
- [ ] Dashboard menampilkan angka hari ini yang benar.
- [ ] Isolasi: akses lintas tenant 403/404 di semua resource.

## Jangan

- Jangan menambah fitur baru — ini gerbang kualitas, bukan fitur.
- Jangan tinggalkan test skip/error yang diabaikan.
