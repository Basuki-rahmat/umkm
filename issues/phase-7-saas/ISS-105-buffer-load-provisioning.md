# ISS-105 — Buffer: Simulasi Beban Provisioning & Multi-Tenant

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-104
- **Perkiraan**: 0.5–1 hari (opsional)
- **Area**: `database/seeders/`, `app/Console/`, `tests/`, `docs/`

## Konteks

Buffer plan Minggu 6 Phase 7: pastikan 10+ tenant provisioning bersamaan tidak tabrakan, dan platform tetap sehat dengan banyak tenant aktif (regressi performa Phase 2 di volume lebih besar).

## Tasks

- [ ] Command `saas:simulate-load --tenants=10`: provision 10 tenant (mix 3 vertical, subdomain suffix acak, konten awal) via action resmi (bukan insert manual) — hasil: 10 tenant usable.
- [ ] Test paralel: jalankan provisioning 2 proses bersamaan (Process/queue dua worker test) → tidak ada slug/nomor/WO/QTN duplikat (unique constraint terbukti cukup, tanpa race error 500).
- [ ] Regressi performa dengan 10 tenant × data: dashboard & transaksi create per tenant < 300ms query (QueryLog) — bandingkan baseline ISS-037, catat di `docs/performance-baseline.md`.
- [ ] Isolasi spot-check: acak 20 pasangan tenant → akses lintas 403/404 (test loop).
- [ ] Kesehatan storage: tiap tenant punya folder terpisah; simulasi upload tidak saling menimpa.
- [ ] Dokumentasikan hasil + bottleneck (jika ada) di `docs/phase-7-summary.md` bagian load.

## Acceptance Criteria

- [ ] Simulasi 10 tenant sukses & usable (login + konten awal + website live).
- [ ] Tidak ada duplikasi nomor dokumen/slug di bawah konkurensi.
- [ ] Baseline performa tercatat; isolasi 20/20 lolos.
- [ ] `pint` + `phpstan` + suite hijau.

## Jangan

- Jangan optimasi prematur di luar temuan ukur.
- Jangan simulate-load di database produksi.
