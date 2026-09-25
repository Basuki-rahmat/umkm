# ISS-029 — Pengeluaran (Expenses)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-028
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Admin/`, `resources/views/admin/expenses/`, `tests/`

## Konteks

Pencatatan pengeluaran operasional (bahan, listrik, transport) untuk laporan pendapatan (pendapatan = pemasukan − pengeluaran).

## Tasks

- [ ] Migration `create_expense_categories_table` (tenant_id, name, unique per tenant) + `create_expenses_table`: `id`, `tenant_id`, `expense_category_id` nullable FK, `amount` decimal(16,2) > 0, `spent_at` date, `description` nullable, `proof_path` nullable (foto nota, opsional, via `AllowedUpload`), `created_by`, timestamps.
- [ ] CRUD pengeluaran (list filter tanggal/kategori + total periode di atas; create/edit/delete OWNER & KASIR).
- [ ] CRUD kategori pengeluaran (sederhana, inline).
- [ ] Audit log CREATE/DELETE.
- [ ] Test: CRUD; total periode benar; isolasi tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Expense` lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan hubungkan pengeluaran ke pembelian supplier (fase lanjutan).
