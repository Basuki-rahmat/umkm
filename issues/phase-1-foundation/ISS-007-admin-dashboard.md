# ISS-007 — Admin Dashboard Layout (Filament/Blade)

- **Tag**: `[AI+riviu]` — pilihan Filament vs Blade murni perlu keputusan manusia di awal task
- **Depends on**: ISS-006
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Providers/Filament/` (jika Filament) atau `resources/views/admin/`, `routes/`, `tests/`

## Konteks

Membuat kerangka panel admin yang akan dipakai semua CRUD berikutnya. **Keputusan arsitektur**: pakai Filament (cepat, lengkap) ATAU Blade+Tailwind manual (kontrol penuh). CATATAN: jika memilih Filament, pilihannya berlaku untuk seluruh proyek dan integrasi multi-tenant Filament harus diuji di task berikutnya.

## Tasks

- [ ] Pasang kerangka `/admin`: sidebar navigasi (Dashboard, Pelanggan, Produk, Transaksi, Pengaturan — placeholder link), topbar dengan nama user + dropdown logout, warna dasar + font.
- [ ] Dashboard awal: 3 widget placeholder (Penjualan Hari Ini, Pesanan Aktif, Piutang) menampilkan `Rp 0`/`0` — data nyata di Phase 2.
- [ ] Menu navigasi difilter per role: OWNER melihat semua; KASIR hanya Pelanggan+Transaksi; STAFF hanya Produk (hardcode dulu, policies datang nanti).
- [ ] Responsif: sidebar collapse di layar kecil (mobile-first).
- [ ] Halaman 403 kustom sederhana.

## Acceptance Criteria

- [ ] Login sebagai OWNER → dashboard tampil dengan 3 widget dan sidebar lengkap.
- [ ] Login sebagai KASIR (bikin dulu manual via panel `/admin/users` yang menyusul di ISS-009; seeder user demo baru ada di ISS-009/ISS-017) → menu terbatas sesuai daftar.
- [ ] Buka dari layar ≤ 380px tetap usable (manual check).
- [ ] `pint` + `phpstan` + `php artisan test` lolos.

## Jangan

- Jangan buat CRUD di issue ini (hanya kerangka + placeholder).
- Jangan integrate data transaksi nyata.
