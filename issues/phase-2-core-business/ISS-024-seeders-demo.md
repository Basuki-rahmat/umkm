# ISS-024 — Seeder Data Demo Lengkap

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-023
- **Perkiraan**: 0.5 hari
- **Area**: `database/seeders/`

## Konteks

Data demo untuk pengembangan & demo ke calon tenant. `php artisan migrate:fresh --seed` harus selalu menghasilkan state yang bisa langsung dipakai mencoba semua fitur yang sudah ada.

## Tasks

- [ ] Seeder tenant demo tetap: "Percetakan Lampung" (vertical PERCETAKAN) & "Warung Maju Jaya" (vertical KULINER), masing-masing OWNER + KASIR + STAFF, settings terisi (nama, telepon, alamat, logo placeholder dari file lokal).
- [ ] Untuk tenant percetakan: 3 kategori, 12 produk (banner, stiker, undangan, dll dengan sku & harga wajar), 5 jasa (desain, laminating, dll), 8 pelanggan, beberapa mutasi stok awal (IN).
- [ ] Untuk tenant kuliner: kategori menu, produk minuman/makanan sederhana, 5 pelanggan.
- [ ] Seeder transaksi demo (setelah transaksi & payment tersedia ISS-026/027): kedua tenant diberi transaksi beragam dengan tanggal berbeda (beberapa bulan lalu s.d. hari ini) — lunas, parsial (piutang tersisa), dibatalkan, + pembayaran & invoice terkait — agar laporan per-periode & receivable langsung berisi angka masuk akal. Idempotent (skip bila sudah ada).
- [ ] Semua seeder idempotent (`firstOrCreate`) dan cepat (< 10 detik).
- [ ] Password demo seragam (`password`) — dilarang dipakai di produksi, tulis di README seeder.

## Acceptance Criteria

- [ ] `php artisan migrate:fresh --seed` bersih & < 10 detik.
- [ ] Login owner demo → semua master data tampil sesuai skenario.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pakai service eksternal (gambar placeholder lokal saja).
