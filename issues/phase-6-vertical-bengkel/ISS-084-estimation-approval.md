# ISS-084 — Estimasi Biaya & Persetujuan Pelanggan (PDF/WA)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-083
- **Perkiraan**: 0.5–1 hari
- **Area**: `resources/views/pdf/estimasi.blade.php`, `app/Http/Controllers/Admin/Workshop/`, `tests/`

## Konteks

Estimasi biaya (plan #12 Output) dikirim ke pemilik kendaraan untuk persetujuan (sesuai alur wajib). MVP: PDF + WA manual; approval tetap dicatat oleh kasir (klik "Disetujui") — approval online publik = backlog.

## Tasks

- [ ] PDF estimasi (DomPDF, kop tenant): kendaraan (plat, merk, km), keluhan, tabel item estimasi (sparepart/jasa + harga), total, catatan "harga dapat berubah setelah pemeriksaan", validitas.
- [ ] Tombol "Kirim Estimasi via WA" (WaLink ke nomor pemilik kendaraan; template pesan estimasi) + tombol "Persetujuan diterima (telepon/WA)" → approved_at + status lanjut.
- [ ] Jika pelanggan menolak sebagian: edit items sebelum approve (masih boleh di status ESTIMASI), estimasi PDF diperbarui.
- [ ] Audit log ESTIMATION_SENT/APPROVED/REVISED.
- [ ] Test: PDF berisi items & total; WA link nomor pemilik benar; edit sebelum approve ok, setelah approve ditolak; audit terekam; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Estimation` lulus.
- [ ] Manual dari HP: kirim estimasi via WA → klik approve → status lanjut SERVIS.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan halaman approval online pelanggan (backlog).
- Jangan izinkan SERVIS tanpa approved_at (sudah di-guard ISS-083 — test regressi).
