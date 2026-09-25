# ISS-071 — Kuliner: Master Menu (Schema & CRUD)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-070 (Phase 4 selesai)
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Http/Controllers/Admin/Menu/`, `resources/views/admin/menu/`, `tests/`

## Konteks

Vertical kedua: kuliner (plan #13). Menu = produk kuliner, tersimpan sebagai produk core agar transaksi/piutang/laporan core terpakai. Menu hanya untuk tenant `vertical = KULINER` (sumber kebenaran vertical — `tenants.vertical`, lihat keputusan ISS-060).

## Tasks

- [ ] Migration `create_menu_details_table`: `id`, `tenant_id`, `product_id` FK unique (produk core), `is_spicy_level` bool default false (punya level pedas), `prep_minutes` int nullable (estimasi siap), `labels` json nullable (mis. ["favorit","pedas","vegetarian"]), timestamps. Menu = produk core + detail ini.
- [ ] Panel: halaman "Menu" (admin kuliner) = list produk core dengan kolom detail menu; form create/edit menu → membuat/mengedit produk core + detail sekaligus (kategori & satuan dari master core, foto via ProcessImage).
- [ ] Menu sidebar "Kuliner" (submenu: Menu, Ketersediaan nanti) — visible hanya tenant kuliner; guard route sama.
- [ ] Seeder tenant kuliner diperkaya: 12 menu dengan kategori (Makanan/Minuman/Snack), harga & foto placeholder.
- [ ] Test: create menu → product + menu_details konsisten; hapus detail tidak menghapus produk core; isolasi; menu tersembunyi untuk tenant lain.

## Acceptance Criteria

- [ ] `php artisan test --filter=Menu` lulus.
- [ ] Manual: tambah menu dengan foto → tampil di list produk core juga.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan duplikasi produk ke tabel kedua (menu_details adalah extension, bukan salinan).
- Jangan varian/add-on (ISS-072).
