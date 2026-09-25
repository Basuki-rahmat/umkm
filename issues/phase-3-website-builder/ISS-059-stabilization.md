# ISS-059 — Stabilisasi Akhir Phase 3

- **Tag**: `[AI+riviu]` — penilaian visual & performa perlu manusia
- **Depends on**: ISS-058
- **Perkiraan**: 1 hari
- **Area**: seluruh repo (perbaikan), `docs/`, `issues/README.md`

## Konteks

Gerbang penutup Phase 3. Kriteria: setiap tenant punya website publik layak jual — konten dari panel, cepat di HP, share WA rapi. Ini yang dijual ke pelanggan paket STARTER.

## Tasks

- [ ] Suite penuh hijau (test + phpstan + pint).
- [ ] Seed 3 tenant demo lengkap (kuliner/percetakan/bengkel) dengan konten realistis: logo, 4 keunggulan, 8+ produk dengan foto, galeri 6 foto, 3 testimoni, 2 artikel, settings/maps — `migrate:fresh --seed` → ketiganya website jadi.
- [ ] Audit Lighthouse mobile (target tercantum di Acceptance) untuk 3 tenant; perbaiki temuan performa/aksesibilitas ringan (ukuran gambar, kontras, ukuran tap target).
- [ ] Cek cross-template: tidak ada section milik vertical lain yang bocor tampil.
- [ ] Uji alur owner baru: isi semua konten dari PANEL saja (tanpa kode) → website layak; catat bagian yang masih perlu dev di `docs/phase-3-summary.md`.
- [ ] Update `issues/README.md` → `Phase 3: 14/14 selesai`.
- [ ] Tulis `docs/phase-3-summary.md`: fitur, keputusan teknis (tema via CSS var, tanpa WYSIWYG, dll), skor Lighthouse, backlog lanjutan.

## Acceptance Criteria (checklist manual)

- [ ] `migrate:fresh --seed` → 3 website demo online di subdomain masing-masing.
- [ ] Lighthouse mobile: Performance ≥ 80, Accessibility ≥ 90, SEO ≥ 90 untuk ketiganya.
- [ ] Semua konten (hero, produk, galeri, testimoni, artikel) diatur dari panel.
- [ ] Share link home & produk ke WA → preview gambar + judul rapi.
- [ ] Form kontak masuk ke panel & follow-up WA berfungsi.
- [ ] Tombol WA float di semua halaman; Maps tampil di kontak.
- [ ] Isolasi antar tenant di semua halaman publik (host A tidak melihat data B).

## Jangan

- Jangan deploy produksi (deployment = keputusan manusia terpisah).
- Jangan tambah fitur baru di gerbang ini.
