# ISS-070 — Stabilisasi Akhir Phase 4 (Vertical Percetakan)

- **Tag**: `[AI+riviu]` — validasi alur bisnis nyata perlu manusia
- **Depends on**: ISS-069
- **Perkiraan**: 1 hari
- **Area**: seluruh repo (perbaikan), `docs/`, `issues/README.md`

## Konteks

Gerbang penutup vertical pertama. Kriteria: satu pesanan percetakan mengalir lengkap tanpa hambatan — quotation → order → DP → produksi → surat jalan → pelunasan — semuanya dari HP, dan tenant non-percetakan tidak melihat fitur percetakan.

## Tasks

- [ ] Suite penuh hijau (test, phpstan, pint).
- [ ] Seeder demo percetakan diperkaya: quotation berbagai status + pesanan beragam status produksi + log + surat jalan + DP → `migrate:fresh --seed` menceritakan bisnis yang hidup.
- [ ] Walkthrough manual end-to-end (checklist di bawah) dari HP, peran OWNER dan STAFF.
- [ ] Audit menu/akses: tenant kuliner & bengkel TIDAK melihat menu Percetakan (panel + route guard) — test menegaskan.
- [ ] Update `issues/README.md` → `Phase 4: 11/11 selesai`.
- [ ] Tulis `docs/phase-4-summary.md`: fitur, keputusan (rumus harga, production_logs, refund guard), feedback backlog (kanban, approve publik, rumus luas otomatis), rekomendasi Phase 5.

## Acceptance Criteria (checklist manual end-to-end)

- [ ] Buat quotation dari kalkulator → kirim PDF via wa.me → ACCEPTED → jadikan pesanan.
- [ ] Order kilat dengan DP 50% → payment & piutang benar.
- [ ] Upload file desain → produksi jalan (status + log) → deadline terpantau di widget.
- [ ] Surat jalan tercetak 2 copy → tandai diterima → produksi SELESAI.
- [ ] Bayar sisa → kwitansi pelunasan → laporan penjualan/produksi + Excel akurat.
- [ ] Tenant kuliner tidak melihat menu/route percetakan (403).
- [ ] Semua test + static analysis hijau.

## Jangan

- Jangan mulai fitur vertical berikutnya sebelum gerbang ini lolos.
- Jangan skip kegagalan apapun di checklist.
