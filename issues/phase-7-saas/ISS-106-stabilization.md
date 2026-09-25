# ISS-106 — Stabilisasi Akhir Phase 7 & Penutup Roadmap

- **Tag**: `[AI+riviu]` — go-live SaaS = keputusan manusia (server, domain, gateway produksi)
- **Depends on**: ISS-103 (+ ISS-104/105 opsional)
- **Perkiraan**: 1–1.5 hari
- **Area**: seluruh repo (perbaikan), `docs/`, `issues/README.md`

## Konteks

Gerbang akhir roadmap Phase 1–7 (plan #35 PHASE 7 — SaaS P4). Kriteria: pendaftaran pelanggan baru 100% mandiri (daftar → bayar → website online), billing berjalan, paywall berlaku, super admin memantau bisnis. Setelah ini: Phase 8 (vertical tambahan) = perluasan, bukan roadmap inti.

## Tasks

- [ ] Suite penuh hijau (test, phpstan, pint) + semua scheduler terdaftar & terdokumentasi (`docs/scheduler.md`: daftar command jadwal + fungsinya — subscription status, invoice generation, reminders, domains:check, webhook alert, dsb).
- [ ] Deployment checklist produksi `docs/go-live.md`: server (Nginx/Caddy, SSL wildcard + on-demand), DNS (central + wildcard), cron scheduler, queue worker, gateway production keys, SMTP, rekening platform, backup cron (ISS-013), monitoring log webhook.
- [ ] Uji pendaftaran mandiri end-to-end di staging (checklist di bawah) — tanpa menyentuh panel super admin.
- [ ] Simulasi bisnis 1 bulan: seed 5 tenant (2 trial, 2 active, 1 expired) → jalankan semua command scheduler dalam urutan waktu → panel MRR/tagihan/paywall/reminders konsisten.
- [ ] Update `issues/README.md` → `Phase 7: 15/15 selesai` + ringkasan total issue sepanjang roadmap.
- [ ] Tulis `docs/phase-7-summary.md` + `docs/roadmap-complete.md`: capaian per phase (1–7), keputusan teknis besar, backlog gabungan (Phase 8 & fitur lanjutan: WA API, PWA, inventory lanjutan, accounting, AI, marketplace), rekomendasi operasional.

## Acceptance Criteria (checklist pendaftaran mandiri, staging)

- [ ] Daftar dari HP → verifikasi email → pilih kuliner + BUSINESS → pilih subdomain → mulai TRIAL → tenant & website live dengan konten awal.
- [ ] Bayar invoice setup via gateway sandbox → webhook → subscription ACTIVE (atau: manual transfer → verifikasi super admin).
- [ ] Ubah period_end ke masa lalu → warning PAST_DUE → lewat grace → paywall → perpanjang (sandbox) → akses kembali.
- [ ] Super admin melihat: MRR berubah sesuai, tagihan PAID, webhook log PROCESSED, reminder terkirim.
- [ ] Tenant EXPIRED terblokir panel tapi data aman & halaman billing accessible.
- [ ] Semua scheduler terdokumentasi & teruji via simulasi 1 bulan.
- [ ] Semua test + static analysis hijau; `migrate:fresh --seed` bersih.

## Jangan

- Jangan deploy produksi dalam issue ini (go-live = keputusan manusia, checklist ada di go-live.md).
- Jangan mulai Phase 8 dalam issue ini — roadmap inti selesai; buat issue baru dari backlog.
