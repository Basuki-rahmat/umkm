# ISS-072 — Varian & Add-on Menu

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-071
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Admin/Menu/`, `tests/`

## Konteks

Varian (level pedas, ukuran) & add-on (topping) wajib ikut ke perhitungan transaksi (plan #5, #13). Implementasi: opsi tersimpan di tabel menu; saat dipilih di transaksi, menjadi item/harga tambahan di transaction_items core.

## Tasks

- [ ] Migration `create_menu_variants_table`: `id`, `tenant_id`, `menu_detail_id` FK, `name` (mis. "Level Pedas"), `options` json (array: {label, extra_price}), `is_required` bool default false, timestamps.
- [ ] Migration `create_menu_addons_table`: `id`, `tenant_id`, `menu_detail_id` FK, `name` (mis. "Telur"), `price` decimal(16,2), `is_active`, timestamps.
- [ ] Panel: di form menu, section "Varian" & "Add-on" (dinamis, tambah/hapus baris; options varian = pasangan label + harga tambahan).
- [ ] Helper `App\Actions\Menu\ResolveMenuItemPrice::run(product, variantSelections[], addonIds[]): ItemPriceBreakdown` — harga akhir + rincian (untuk dipakai transaksi & ditampilkan).
- [ ] Panel transaksi kuliner: saat memilih menu, modal pilih varian (wajib bila is_required) + add-on → harga item live update.
- [ ] Test: harga = base + varian + add-on benar; varian wajib tidak boleh kosong; add-on nonaktif tak bisa dipilih; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Variant|Addon` lulus.
- [ ] Manual: menu dengan level pedas + topping → harga di form transaksi sesuai.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan simpan pilihan varian sebagai teks bebas — harus dari options (agar laporan per varian akurat).
- Pilihan varian/add-on disimpan di `transaction_items.detail` json (kolom yang diadakan ISS-025 untuk order menu vertikal) — jangan tambah kolom baru.
