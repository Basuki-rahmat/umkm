# ISS-054 — Katalog Produk/Jasa Publik + WhatsApp Berkonteks

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-053
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Http/Controllers/Public/`, `resources/views/public/catalog/`, `tests/`

## Konteks

Katalog publik membaca data produk/jasa panel (Phase 2) + tombol WA per item (plan #17): pesan otomatis "Saya tertarik dengan {nama}". Opsi sembunyikan harga per tenant (beberapa UMKM tak ingin harga publik).

## Tasks

- [ ] Halaman `/produk`: grid kartu (foto thumb, nama, kategori, harga bila ditampilkan, tombol WA per item) + filter kategori + pagination; card kosong bila tidak ada.
- [ ] Detail produk/jasa `/produk/{slug-or-id}`: foto besar, deskripsi, harga (bila tampil), tombol WA, produk terkait (kategori sama, max 4).
- [ ] Setting tenant `show_prices` (bool, default true) → saat false semua harga disembunyikan, tombol WA jadi "Tanyakan Harga".
- [ ] Pesan WA per item: `Halo, saya tertarik dengan {nama produk}. Apakah tersedia?` — via `WaLink` (ISS-032), nomor dari settings.
- [ ] Tampilan per template_type: kuliner = gaya daftar menu per kategori (foto + harga), percetakan = portfolio grid, bengkel = daftar layanan. Cukup beda class/partial, satu controller.
- [ ] Hanya `is_active` produk/jasa yang tampil; urutan: featured/manual sort nanti.
- [ ] Test: grid menampilkan produk aktif tenant saja; filter kategori; show_prices=false menyembunyikan semua harga; link WA berisi nama produk benar; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=PublicCatalog` lulus.
- [ ] Manual dari HP: klik produk → WA terbuka dengan pesan benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat keranjang/checkout online (order tetap via WA — sesuai MVP).
- Jangan tampilkan stok persis di publik (cukup "tersedia"/"habis").
