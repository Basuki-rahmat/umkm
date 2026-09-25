# ISS-078 — Kuliner: Struk Digital via WA & Notifikasi Siap

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-077
- **Perkiraan**: 0.5 hari
- **Area**: `app/Support/`, `resources/views/admin/culinary/`, `tests/`

## Konteks

Komunikasi WA fase MVP (plan #17): klik-kirim manual. Untuk kuliner: struk digital ke pelanggan (BUNGKUS/ANTAR dengan nomor HP) + "pesanan siap" (DI_TEMPAT, panggil via WA bila pelanggan menunggu di luar).

## Tasks

- [ ] Template pesan: (1) struk digital — nomor, items ringkas, total, status, ucapan terima kasih; (2) pesanan siap — "Pesanan #{nomor} sudah siap diambil/diantar".
- [ ] Tombol di detail transaksi & kitchen card: "Kirim via WA" (WaLink ke nomor pelanggan transaksi; walk-in tanpa nomor → tombol tersembunyi).
- [ ] Struk digital PDF dikirim manual pelanggan (user download + attach di WA manual) — link PDF share-friendly (publik dengan token sekali pakai opsional sederhana: signed URL 24 jam).
- [ ] Audit log untuk semua kirim WA.
- [ ] Test: template terisi benar; signed URL expired setelah 24 jam; tombol hilang bila tanpa nomor; audit terekam; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=CulinaryWa` lulus.
- [ ] Manual dari HP: klik → WA terbuka dengan pesan benar; link PDF bisa dibuka pelanggan.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim otomatis (semua tombol manual).
- Jangan URL PDF permanen publik (signed & expiring).
