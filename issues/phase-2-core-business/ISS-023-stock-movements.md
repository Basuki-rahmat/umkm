# ISS-023 — Stok Dasar: Pencatatan Mutasi (stock_movements)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-022
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Actions/Stock/`, `tests/`

## Konteks

Semua perubahan stok wajib tercatat (jejak audit + dasar modul inventory fase lanjutan). Stok produk TIDAK diubah langsung — selalu lewat action yang mencatat mutasi.

## Tasks

- [ ] Migration `create_stock_movements_table`: `id`, `tenant_id`, `product_id`, `type` (enum: `IN`, `OUT`, `ADJUSTMENT`), `qty` integer, `stock_before`, `stock_after`, `reference_type` nullable (mis. Transaction nanti), `reference_id` nullable, `note` nullable, `user_id`, timestamps.
- [ ] Class `App\Actions\Stock\RecordMovement` (satu pintu: validasi stok tidak minus untuk OUT/ADJUSTMENT minus → throw exception custom `InsufficientStockException`).
- [ ] Halaman "Penyesuaian Stok" (OWNER): pilih produk, input qty baru (atau +/-), note → memakai type ADJUSTMENT.
- [ ] Halaman "Stok Menipis": produk dengan `stock <= min_stock AND is_active`, link ke penyesuaian.
- [ ] Menu sidebar "Stok" (OWNER).
- [ ] Test: IN menambah; OUT mengurangi + gagal bila stok tidak cukup; ADJUSTMENT set ke nilai absolut + selisih tercatat; riwayat mutasi per produk tampil.

## Acceptance Criteria

- [ ] `php artisan test --filter=Stock` lulus.
- [ ] Manual: penyesuaian stok → riwayat mutasi tampil dengan before/after benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kaitkan ke transaksi (ISS-026).
- Jangan izinkan ubah `stock` langsung dari form produk (hanya via mutasi) — hapus kolom stock dari ProductRequest jika masih ada.
