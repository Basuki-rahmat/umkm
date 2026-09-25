# ISS-073 — Ketersediaan Menu (Habis/Tersedia) Real-Time ke Website

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-072
- **Perkiraan**: 0.5 hari
- **Area**: `app/Http/Controllers/Admin/Menu/`, `app/Http/Controllers/Public/`, `tests/`

## Konteks

Plan checklist Phase 5: toggle tersedia/habis cepat dari panel → langsung terlihat di website publik (katalog Phase 3).

## Tasks

- [ ] Panel: halaman "Ketersediaan" (grid semua menu) — toggle besar per item (is_active produk core) + tombol "Set semua tersedia".
- [ ] Website publik (katalog menu kuliner): menu habis tetap tampil tapi badge "HABIS" + tombol WA dinonaktifkan; opsi setting `hide_unavailable` (default false) untuk sembunyikan total.
- [ ] Jika stok core menu terpakai (menu dengan stok): menu stok 0 otomatis tampil habis (mapping stok → availability).
- [ ] Test: toggle habis → website menampilkan badge & WA mati; hide_unavailable menyembunyikan; stok 0 → habis; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Availability` lulus.
- [ ] Manual: toggle dari HP → refresh website → berubah.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan polling/websocket real-time (refresh halaman cukup untuk MVP).
- Jangan ubah katalog tenant non-kuliner.
