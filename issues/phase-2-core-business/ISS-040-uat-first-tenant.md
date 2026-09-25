# ISS-040 — UAT dengan 1 Calon Tenant (Percetakan)

- **Tag**: `[AI+riviu]` — butuh manusia: temui calon tenant nyata
- **Depends on**: ISS-039
- **Perkiraan**: 1–2 hari (termasuk koordinasi eksternal)
- **Area**: data demo, `docs/uat-feedback.md`

## Konteks

Gerbang validasi produk: calon tenant nyata (vertical percetakan) mencoba alur harian. Feedback menjadi input desain Phase 4. Issue ini 70% kegiatan manusia, 30% perbaikan kecil.

## Tasks

- [ ] Siapkan environment demo online/staging + seed tenant percetakan dengan produk & harga nyata sesuai input calon tenant.
- [ ] Sesi terpandu 1–2 jam: calon tenant memegang HP, kerjakan: input produk, buat transaksi, bayar parsial, lunas, cetak nota, lihat laporan.
- [ ] Catat semua feedback di `docs/uat-feedback.md` (keluhan, usulan, hal yang membingungkan) + kategorisasi: BLOCKER (perbaiki sekarang), INPUT PHASE 4 (catat), NICE-TO-HAVE (backlog).
- [ ] Perbaiki semua BLOCKER dalam issue ini (tanpa fitur baru).
- [ ] Sesi tindak lanjut singkat: verifikasi BLOCKER beres.

## Acceptance Criteria

- [ ] Dokumen feedback terisi lengkap dengan kategorisasi.
- [ ] Semua BLOCKER selesai & teruji ulang bersama calon tenant.
- [ ] Test + static analysis tetap hijau setelah perbaikan.

## Jangan

- Jangan terima scope baru (fitur custom request calon tenant → catat di backlog, jangan kerjakan).
- Jangan deploy ke produksi nyata dengan data pelanggan sungguhan.
