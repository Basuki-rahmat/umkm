# ISS-057 — WhatsApp Mengapung Global & Google Maps

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-056
- **Perkiraan**: 0.5 hari
- **Area**: `resources/views/public/`, `app/Support/`, `tests/`

## Konteks

Melengkapi komponen komunikasi yang tersebar: tombol WA mengapung di SEMUA halaman publik + Maps konsisten (plan #9, #17).

## Tasks

- [ ] Komponen tombol WA mengapung (kanan bawah, ikon + label singkat): nomor & template pesan dari settings; di mobile cukup ikon; hilang otomatis bila nomor setting kosong; tidak menutupi konten (safe-area).
- [ ] Halaman yang sudah punya CTA WA spesifik (produk, kontak) tidak dobel — komponen float menghindari halaman kontak.
- [ ] Komponen partial Maps: dari setting `map_embed_url` bila diisi (link share Google Maps → format embed), fallback iframe dari `map_lat`/`map_lng`, fallback "Lihat di Google Maps" (link) bila keduanya kosong; dipakai di Kontak + section lokasi Home.
- [ ] Helper parsing link share Google Maps → koordinat/embed URL (test beberapa format link umum).
- [ ] Test: float tampil di semua halaman kecuali kontak; nomor kosong → komponen tidak dirender; parsing link Maps benar; halaman tetap valid tanpa maps.

## Acceptance Criteria

- [ ] `php artisan test --filter=WaFloat|Maps` lulus.
- [ ] Manual dari HP: tombol WA selalu terlihat, tidak mengganggu; maps tampil di kontak.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan load Google Maps JS SDK — iframe saja (ringan).
- Jangan pakai API key Google.
