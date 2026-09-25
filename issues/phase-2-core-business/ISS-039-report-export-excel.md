# ISS-039 — Export Excel Semua Laporan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-038
- **Perkiraan**: 1 hari
- **Area**: `composer.json`, `app/Http/Controllers/Admin/Reports/`, `tests/`

## Konteks

Plan #20: laporan dapat View/Print/PDF/**Excel**. Pakai `maatwebsite/excel` (atau openpyxl-style sederhana via `openspout` bila mau ringan — pilih SATU, konsisten).

## Tasks

- [ ] Install paket export pilihan; konfigurasi default (format xlsx).
- [ ] Export untuk 7 laporan inti (penjualan, pembayaran, piutang, pelanggan, produk, pengeluaran, pendapatan): header kolom Indonesia, baris ringkasan di atas, nama file `laporan-<jenis>-YYYY-MM-DD.xlsx`.
- [ ] Filter periode diteruskan ke export (query sama dengan halaman web — reuse action/query yang sama, JANGAN duplikasi).
- [ ] Tombol "Export Excel" di tiap halaman laporan.
- [ ] Test: export menghasilkan file valid (parse kembali / assert content-type & ukuran > 0); angka baris pertama cocok dengan data seeded; hanya role OWNER & ADMIN TENANT yang boleh export.

## Acceptance Criteria

- [ ] `php artisan test --filter=Export` lulus.
- [ ] Manual: 7 laporan ter-export dan dibuka di Excel/LibreOffice tanpa warning.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan export via client-side hack (CSV diganti nama) — harus xlsx asli.
- Jangan query ulang berbeda dari halaman web.
