# ISS-049 — Halaman Home, Tentang & Kontak

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-048
- **Perkiraan**: 1–1.5 hari
- **Area**: `resources/views/public/pages/`, `app/Http/Controllers/Public/`, `tests/`

## Konteks

Halaman inti pertama sesuai struktur plan (#9): Home (hero, keunggulan, section dinamis), Tentang, Kontak. Konten dari `websites` + `settings`.

## Tasks

- [ ] **Home**: hero besar (judul, subjudul, CTA wa.me dari `websites`), section "Keunggulan" (4 item, konten dari settings baru `advantage_1..4` di panel), section placeholder untuk katalog/testimoni/galeri/artikel (render bila section aktif & datanya ada).
- [ ] **Tentang**: deskripsi usaha (settings `about`), jam operasional (settings), foto dari galeri (max 3, jika ada).
- [ ] **Kontak**: alamat, telepon/WA (tombol wa.me), email, sosmed ikon link, jam operasional, embed Google Maps (partial Maps dari `map_embed_url` atau fallback `map_lat`/`map_lng` di settings — ISS-057; skip rapi bila kosong).
- [ ] Panel: tab "Profil Usaha" diperluas dengan field about, jam operasional, 4 keunggulan, dan toggle `show_prices` (kontrol harga publik katalog ISS-054).
- [ ] Semua halaman memakai layout ISS-047 + meta title/description sendiri.
- [ ] Test: 3 halaman 200 dengan konten settings benar; hero tampil; maps tidak merender bila koordinat kosong; isolasi (host tenant A menampilkan data A).

## Acceptance Criteria

- [ ] `php artisan test --filter=PublicPages` lulus.
- [ ] Manual dari HP: 3 halaman rapi, CTA WA berfungsi.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat katalog/artikel/galeri lengkap (issue berikutnya).
- Jangan load Maps SDK berat — cukup iframe embed.
