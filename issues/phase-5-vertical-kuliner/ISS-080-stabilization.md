# ISS-080 — Stabilisasi Akhir Phase 5 (Vertical Kuliner)

- **Tag**: `[AI+riviu]` — validasi operasional nyata perlu manusia
- **Depends on**: ISS-078 (ISS-079 opsional)
- **Perkiraan**: 1 hari
- **Area**: seluruh repo (perbaikan), `docs/`, `issues/README.md`

## Konteks

Gerbang penutup vertical kuliner. Kriteria: satu hari operasional penuh berjalan dari HP — order → dapur → struk → bayar → laporan — tanpa hambatan; tenant lain tidak melihat fitur kuliner.

## Tasks

- [ ] Suite penuh hijau (test, phpstan, pint).
- [ ] Seeder demo kuliner hidup: menu + varian/add-on + order berbagai tipe/status + kitchen logs → dapur & laporan berisi.
- [ ] Simulasi 1 hari operasional (checklist di bawah) dari HP — catat temuan.
- [ ] Audit akses: tenant percetakan/bengkel tidak melihat menu Kuliner/Kitchen (route guard + test).
- [ ] Audit setting key vertical: tetapkan pemilik & kontrol UI untuk `hide_unavailable` (ISS-073), `receipt_width` (ISS-076), `kitchen_warn_minutes` (ISS-075) — simpan via helper `setting()` (ISS-010), kontrol di panel Pengaturan jurusan masing-masing.
- [ ] Update `issues/README.md` → `Phase 5: 10/10 selesai` (atau 9/10 bila ISS-079 sengaja ditunda — catat).
- [ ] Tulis `docs/phase-5-summary.md`: fitur, keputusan (kitchen polling, thermal via print browser, parse json laporan di PHP), backlog (websocket, ESC-POS, QR payment), rekomendasi Phase 6.

## Acceptance Criteria (checklist manual end-to-end)

- [ ] Order cepat dari HP: 3 menu, 1 varian wajib + add-on, tipe ANTAR + alamat.
- [ ] Dapur: order muncul ≤ 15 detik, pindah status sampai SIAP → DIANTAR.
- [ ] Bayar CASH dengan kembalian → struk thermal 58mm tercetak rapi.
- [ ] Struk digital via WA (BUNGKUS) → pelanggan bisa buka PDF.
- [ ] Toggle menu habis → website berubah.
- [ ] 3 laporan kuliner + Excel akurat (cocok dengan transaksi demo).
- [ ] Tenant non-kulier tidak melihat fitur (403).
- [ ] Semua test + static analysis hijau.

## Jangan

- Jangan mulai Phase 6 sebelum gerbang lolos.
- Jangan abaikan temuan UX dari simulasi (catat minimal).
