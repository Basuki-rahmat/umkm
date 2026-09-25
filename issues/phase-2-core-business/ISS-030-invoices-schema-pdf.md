# ISS-030 — Invoice: Migrasi, Generate Otomatis & PDF (DomPDF)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-029
- **Perkiraan**: 1–1.5 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Actions/Invoices/`, `resources/views/pdf/`, `tests/`

## Konteks

Invoice dibuat otomatis saat transaksi ditutup (`CloseTransaction` → `COMPLETED` = order ditutup, **bukan** "lunas"), nomor unik `INV/2026/000001` per tenant (pakai `DocumentNumber` prefix `INV` — period `YYYY` + padding 6, konfigurasi global di ISS-025). Transaksi COMPLETED boleh masih punya piutang (sisa) — invoice tetap terbit. PDF memakai kop dari settings tenant. Lihat `docs/state-machine-core-vertical.md` §3 & §5.

## Tasks

- [ ] Migration `create_invoices_table`: `id`, `tenant_id`, `transaction_id` FK unique, `number` unique `(tenant_id, number)`, `status` (enum `ISSUED`, `CANCELLED`; default `ISSUED`), `issued_at` date, `due_date` nullable, timestamps.
- [ ] `invoice_items` TIDAK dibuat terpisah — render langsung dari `transaction_items` (menghindari duplikasi data; catat keputusan ini di laporan).
- [ ] Action `IssueInvoice` (dipanggil `CloseTransaction` saat order ditutup; juga tombol manual di detail transaksi bila belum ada). Idempotent; PDF menampilkan status bayar faktual (sudah dibayar & sisa).
- [ ] Template PDF `resources/views/pdf/invoice.blade.php`: kop (logo, nama, alamat, telepon dari settings), nomor + tanggal, data pelanggan, tabel item, subtotal/diskon/pajak/total, status pembayaran & sisa, footer terima kasih.
- [ ] Route `/admin/transactions/{id}/invoice/pdf` → stream PDF (DomPDF); tombol di detail transaksi & list.
- [ ] Font & ukuran A4 rapi; teruji teks Indonesia & angka ribuan `1.250.000`.
- [ ] Test: invoice otomatis terbit saat COMPLETED (sekali saja — idempotent); PDF route 200 & berisi nomor invoice; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Invoice` lulus.
- [ ] Manual: buat transaksi → invoice PDF terunduh dengan kop tenant.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim via WA/email (ISS-032).
- Jangan kwitansi (ISS-031).
