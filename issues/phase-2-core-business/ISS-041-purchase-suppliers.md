# ISS-041 — Pembelian & Supplier (Dasar)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-040
- **Perkiraan**: 1.5 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Admin/`, `resources/views/admin/purchases/`, `tests/`

## Konteks

Melengkapi siklus stok: stok masuk lewat pembelian ke supplier (bukan hanya penyesuaian manual). Schema `suppliers` sudah dirancang di plan (#23). Retur pembelian sederhana: buat ulang pembelian minus.

## Tasks

- [ ] Migration `create_suppliers_table` (tenant_id, name, phone nullable, address nullable, timestamps, softDeletes; unique `(tenant_id, name)`) + CRUD sederhana.
- [ ] Migration `create_purchases_table` (tenant_id, `number` via DocumentNumber prefix `PO` — format `PO/YYYYMM/0001`, period `YYYYMM` + padding 4), `supplier_id` nullable, status `COMPLETED`/`CANCELLED`, total decimal(16,2), note, created_by, timestamps) + `create_purchase_items_table` (tenant_id, purchase_id, product_id, qty integer, unit_cost decimal(16,2), subtotal decimal(16,2), timestamps).
- [ ] Flow create pembelian: pilih supplier, tambah produk + qty + harga beli → simpan atomik: RecordMovement `IN` per item + update `cost_price` produk terakhir (opsional, wajib konfirmasi).
- [ ] Batalkan pembelian (OWNER, hanya jika stok cukup dikembalikan) → RecordMovement OUT dengan reference pembelian.
- [ ] Halaman list pembelian + detail; laporan pembelian per periode/supplier (masuk menu Laporan).
- [ ] Test: pembelian menambah stok + mutasi tercatat + total benar; cancel mengembalikan stok; isolasi tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Purchase|Supplier` lulus.
- [ ] Manual: beli 100 pcs → stok naik, mutasi IN tampil, laporan pembelian benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat purchase order approval flow (fase lanjutan).
- Jangan hubungkan pembelian ke pengeluaran (expenses) — terpisah.
