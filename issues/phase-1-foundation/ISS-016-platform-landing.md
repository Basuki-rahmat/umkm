# ISS-016 — Landing Page Platform (Sentral)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-015
- **Perkiraan**: 0.5–1 hari
- **Area**: `resources/views/landing/`, `app/Http/Controllers/`, `routes/`

## Konteks

Halaman depan platform (di central domain): menjelaskan produk + 3 paket harga (dari plan #21) + CTA kontak WA. MVP: statis, tanpa CMS. Website per-tenant dibuat di Phase 3 — ini hanya halaman platform.

## Tasks

- [ ] Route `/` (central): hero (nama produk, tagline "Sistem Digitalisasi UMKM Siap Pakai"), 3 kartu paket (STARTER Rp750rb + 100–150rb/bln; BUSINESS Rp1,5jt + 199–250rb/bln; PRO Rp3–5jt + 499rb+/bln) + tombol "Konsultasi via WhatsApp".
- [ ] Section keunggulan (4 item: Multi-tenant, Siap pakai, WhatsApp terintegrasi, Laporan otomatis) + footer sederhana (© 2026).
- [ ] Link "Masuk" ke `/admin` atau `/login`.
- [ ] Responsif mobile-first, gambar dari placeholder lokal (bukan hotlink).
- [ ] Meta title + description Indonesia.

## Acceptance Criteria

- [ ] Halaman `/` tampil, 3 kartu paket akurat sesuai angka di atas.
- [ ] Lighthouse mobile: skor aksesibilitas ≥ 90.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat CMS/dynamic content.
- Jangan tampilkan harga berbeda dari tabel Model Harga di plan.md (angka di task ini adalah acuan).
