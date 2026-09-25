# ISS-053 — Kompresi & Optimasi Gambar Otomatis

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-052
- **Perkiraan**: 0.5–1 hari
- **Area**: `composer.json`, `app/Actions/Media/`, semua titik upload, `tests/`

## Konteks

Plan #36 (mobile-first, halaman ringan): semua gambar upload (logo, produk, galeri, bukti bayar) dikompres otomatis + dibuat thumbnail. Pakai `intervention/image` (GD).

## Tasks

- [ ] Install `intervention/image`; pastikan extension GD tersedia di environment (catat di docs bila perlu konfigurasi server).
- [ ] Action `App\Actions\Media\ProcessImage`: resize maks 1600px sisi panjang, kualitas JPEG 80, buat thumbnail 400px (`*-thumb.jpg`) untuk grid; return path utama + thumb.
- [ ] Terapkan di semua titik upload gambar yang ada: logo (resize maks 512px + PNG dipertahankan transparansi), produk, galeri, testimoni.
- [ ] Tambah helper thumb di tampilan: grid memakai `*-thumb`, detail memakai asli.
- [ ] Non-gambar (PDF/desain) tidak diproses — lewat apa adanya.
- [ ] Test: upload JPG 4000px 4MB → tersimpan ≤ 1600px & ukuran < 800KB; thumb dibuat; PNG logo tetap punya alpha; PDF tidak berubah.

## Acceptance Criteria

- [ ] `php artisan test --filter=ImageProcessing` lulus.
- [ ] Manual: halaman galeri memuat thumbnail ringan (cek ukuran via DevTools network).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pakai service eksternal/cloud (lokal saja).
- Jangan hapus file asli (simpan sebagai source; opsional).
