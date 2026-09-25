# ISS-061 — Kalkulasi Harga Kombinasi (Bahan × Ukuran × Finishing × Jumlah)

- **Tag**: `[AI+riviu]` — rumus harga dasar perlu keputusan owner (bisnis)
- **Depends on**: ISS-060
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Actions/Print/`, `app/Models/`, `database/migrations/`, `tests/`

## Konteks

Harga cetak = f(bahan, ukuran, finishing, qty). Rumus harus transparan & bisa override manual oleh kasir. Rumus default MVP: `harga/satuan = base_price` (atau harga dari `price_tiers` bila qty melewati break); `total = harga/satuan × qty + harga_finishing`. Rumus luas (bahan per m²) = backlog, sengaja tidak diimplementasikan di MVP (lihat Jangan). Owner bisa edit harga via `print_products`.

## Tasks

- [ ] Migration `create_print_products_table`: `id`, `tenant_id`, `product_id` FK (produk core — item jualan), `print_material_id` nullable, `print_size_id` nullable, `print_finishing_id` nullable, `base_price` decimal(16,2) (harga dasar per satuan — dihitung manual owner), `min_qty` int default 1, `price_tiers` json nullable (break qty → harga/satuan, mis. {50: 15000, 100: 12000}), timestamps.
- [ ] Action `App\Actions\Print\CalculatePrice::run(PrintProduct, qty, ?finishing): PriceResult` — memakai price_tiers bila ada, fallback base_price; result berisi rincian (bahan, ukuran, finishing, tier yang dipakai) agar bisa ditampilkan di quotation.
- [ ] UI kalkulator di panel (halaman "Hitung Harga"): pilih produk cetak → qty → finishing → tampil harga + rincian; tombol "Jadikan Item Transaksi" (prefill form transaksi dari Phase 2).
- [ ] Stok produk cetak tetap memakai stok core (unit rim/pcs/lembar) — kalkulator tidak menyentuh stok.
- [ ] Test: tier harga benar di break qty; tanpa tier → base_price; finishing menambah; kombinasi tanpa bahan/ukuran tetap valid (produk jasa desain misalnya); isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=PrintPrice` lulus.
- [ ] Manual: hitung banner 3×1m vinyl 50pcs → angka & rincian masuk akal, bisa lanjut jadi item transaksi.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan hardcode rumus luas otomatis (bahan per m²) di MVP — cukup base_price per satuan + tier; rumus luas = backlog.
- Jangan izinkan harga < 0.
