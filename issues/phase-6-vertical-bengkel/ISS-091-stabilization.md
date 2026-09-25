# ISS-091 — Stabilisasi Akhir Phase 6 (Vertical Bengkel) & Penutup MVP

- **Tag**: `[AI+riviu]` — gerbang MVP utama, wajib manusia
- **Depends on**: ISS-088 (ISS-089/090 opsional)
- **Perkiraan**: 1–1.5 hari
- **Area**: seluruh repo (perbaikan), `docs/`, `issues/README.md`

## Konteks

Gerbang ganda: menutup vertical bengkel DAN **menutup seluruh MVP** (Phase 1–6 sesuai kriteria sukses plan #34/Kriteria Sukses MVP). Setelah gerbang ini lolos, platform layak dijual ke tenant pertama.

## Tasks

- [ ] Suite penuh hijau (test, phpstan, pint).
- [ ] Seeder demo bengkel hidup: kendaraan + work order berbagai status + riwayat 3 bulan + sparepart stok → laporan & pengingat berisi.
- [ ] Walkthrough end-to-end bengkel dari HP (checklist di bawah).
- [ ] **Audit MVP menyeluruh vs Kriteria Sukses MVP (plan #34)** — verifikasi 12 poin satu per satu, catat hasil.
- [ ] Audit menu 3 vertical saling tersembunyi (kuliner ≠ percetakan ≠ bengkel) — test regressi.
- [ ] Audit setting/config key vertical: tetapkan pemilik & kontrol UI untuk config booking online (jam buka, durasi slot 30, kapasitas per slot) milik ISS-086 dan config QC (`workshop-qc.php`) milik ISS-088 — booking config disimpan sebagai setting tenant (helper `setting()`, pola ISS-080).
- [ ] Update `issues/README.md` → `Phase 6: 11/11 selesai`.
- [ ] Tulis `docs/phase-6-summary.md` + `docs/mvp-readiness.md`: fitur per vertical, keputusan teknis, backlog gabungan (dari semua buffer yang dilewati), rekomendasi Phase 7 (SaaS).

## Acceptance Criteria (checklist manual end-to-end bengkel)

- [ ] Booking online dari web → konfirmasi → check-in.
- [ ] Pemeriksaan → estimasi PDF + WA → persetujuan wajib → servis → QC checklist lengkap → pembayaran (transaksi core) → SELESAI.
- [ ] Riwayat kendaraan ter-update; laporan mekanik/servis/sparepart + Excel akurat.
- [ ] WA "mobil siap" terkirim (klik) dari HP.
- [ ] Tenant lain tidak melihat fitur bengkel (dan sebaliknya).
- [ ] Semua test + static analysis hijau.

## Acceptance Criteria (checklist MVP — dari plan #34)

- [ ] Admin dapat membuat tenant; tenant dapat login & atur profil/produk/pelanggan/transaksi.
- [ ] Sistem membuat invoice, mencatat pembayaran, menghasilkan laporan.
- [ ] Website publik tampil per tenant; data antar tenant terisolasi; usable via HP.

## Jangan

- Jangan mulai Phase 7 sebelum kedua checklist lolos.
- Jangan loloskan checklist dengan catatan diam-diam — semua temuan ditulis di mvp-readiness.md.
