# ISS-019 — CRUD Pelanggan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-018
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/CustomerController.php`, `app/Http/Requests/`, `resources/views/admin/customers/`, `routes/`, `tests/`

## Konteks

CRUD pertama panel admin — jadi pola referensi untuk semua CRUD berikutnya (list dengan search/filter/pagination, form, soft delete).

## Tasks

- [ ] Halaman index: tabel (nama, telepon, email, jumlah transaksi nanti kosong dulu, dibuat), search by nama/telepon/email, pagination 25, sort terbaru.
- [ ] Form create/edit via Form Request `CustomerRequest`: `name` required; `phone` nullable format Indonesia (`08…`/`+62…` regex); `email` nullable email; address/note nullable.
- [ ] Delete = soft delete; list default menyembunyikan terhapus + tab "Terhapus" (OWNER saja) dengan restore.
- [ ] Navigasi sidebar: masuk menu "Pelanggan" (role KASIR & OWNER — sesuai izin dari ISS-007).
- [ ] Tambah audit log CREATE/UPDATE/DELETE (pola dari ISS-011).
- [ ] Test fitur: CRUD sukses; validasi telepon salah ditolak; isolasi: customer tenant B tidak terlihat/tidak bisa diedit tenant A (403/404).

## Acceptance Criteria

- [ ] `php artisan test --filter=Customer` lulus.
- [ ] Manual: alur buat → edit → hapus → restore (OWNER) berjalan.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat import/export (nanti di buffer Phase 2).
- Jangan relasi ke transaksi dulu (tabelnya belum ada).
