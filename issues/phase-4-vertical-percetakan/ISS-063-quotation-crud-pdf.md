# ISS-063 — Quotation: CRUD, Alur Status & PDF

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-062
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Http/Controllers/Admin/QuotationController.php`, `resources/views/admin/quotations/`, `resources/views/pdf/quotation.blade.php`, `tests/`

## Konteks

Alur quotation (plan #11): buat penawaran → kirim ke pelanggan → status berjalan DRAFT → SENT → ACCEPTED/REJECTED, dan otomatis EXPIRED lewat valid_until.

## Tasks

- [ ] CRUD: index (filter status/pelanggan, search nomor), create (pelanggan + items: nama manual atau dari kalkulator ISS-061, qty, harga, valid_until, note), edit hanya saat DRAFT.
- [ ] Status transition (tombol di detail, role KASIR/OWNER): Kirim (DRAFT→SENT, catat sent_at), Terima (SENT→ACCEPTED), Tolak (SENT→REJECTED wajib alasan di note).
- [ ] Auto-EXPIRED: scheduler harian → quotation SENT melewati valid_until → EXPIRED (routes/console.php).
- [ ] PDF quotation (DomPDF, kop tenant, rincian item + detail kombinasi bahan/ukuran/finishing, total, valid_until, footer kontak) + tombol kirim via wa.me (template pesan penawaran — reuse WaLink).
- [ ] Audit log CREATE/SENT/ACCEPT/REJECT.
- [ ] Test: alur status valid (transition liar ditolak); expired otomatis (travel time / jalankan command); PDF 200 berisi nomor; wa.me link berisi nomor & total; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Quotation` lulus.
- [ ] Manual: buat → kirim → PDF → WA link benar → terima.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan approval online oleh pelanggan (link approve publik = backlog).
- Jangan ubah transaksi core (konversi di ISS-064).
