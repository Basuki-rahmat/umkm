# ISS-038 — UX Mobile: Alur Kasir dari HP

- **Tag**: `[AI+riviu]` — hasil uji perlu review manusia
- **Depends on**: ISS-037
- **Perkiraan**: 1 hari
- **Area**: `resources/views/admin/**` (perbaikan), dokumentasi hasil uji

## Konteks

Plan #36: dashboard wajib nyaman di layar kecil — kasir UMKM bekerja dari HP. Target: 1 transaksi lengkap dikerjakan mulus dari HP tanpa pinch-zoom.

## Tasks

- [ ] Audit seluruh form & list di layar 360px: input font-size ≥ 16px (hindari auto-zoom iOS), tombol ≥ 44px, tabel overflow-x yang wajar.
- [ ] Form transaksi: baris item mudah ditambah/dihapus dengan jempol; dropdown pencarian produk menyatu (bukan modal kecil); angka keypad (`inputmode="numeric"`) untuk qty/harga.
- [ ] Verifikasi pembayaran & halaman piutang usable dari HP.
- [ ] Perbaiki semua temuan; dokumentasikan hasil uji (screenshot deskripsi) di `docs/mobile-ux-checklist.md`.
- [ ] (Opsional) Tambah PWA manifest sederhana (nama, ikon, warna) — tanpa service worker kompleks.

## Acceptance Criteria

- [ ] Checklist dokumen lengkap semua halaman inti (login, dashboard, transaksi create/detail/list, pembayaran, piutang, laporan utama).
- [ ] Manual: buat transaksi lengkap dari HP Chrome Android → tanpa hambatan.
- [ ] `pint` + `phpstan` + test lolos.

## Jangan

- Jangan ubah logika backend — hanya tampilan/UX.
- Jangan install framework CSS baru.
