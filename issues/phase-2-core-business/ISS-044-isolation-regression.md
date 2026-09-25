# ISS-044 — Regressi Isolasi Tenant (Semua Resource Baru)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-043
- **Perkiraan**: 0.5–1 hari
- **Area**: `tests/Feature/Isolation/`

## Konteks

Mirroring ISS-014 untuk semua resource Phase 2: customer, category, unit, product, service, stock movement, transaction, payment, receivable, expense, invoice, purchase, supplier, refund. Gerbang keamanan sebelum stabilisasi.

## Tasks

- [ ] Tambah test isolasi per resource baru: list hanya menampilkan data tenant sendiri; akses/edit/hapus via ID tenant lain → 403/404; create lewat form selalu mengikat tenant session.
- [ ] Test khusus rawan: `DocumentNumber` per tenant tidak saling ganggu (nomor sama antar tenant BOLEH tapi tidak bentrok karena unique (tenant_id, number)); pembayaran transaksi tenant lain ditolak; export laporan tidak memuat data tenant lain.
- [ ] Update `docs/isolation-checklist.md` (tabel resource × status test, dari ISS-014).
- [ ] Perbaiki setiap kebocoran yang ditemukan.

## Acceptance Criteria

- [ ] `php artisan test --filter=Isolation` lulus semua resource.
- [ ] Checklist dokumen lengkap (14 resource).
- [ ] Suite penuh hijau.

## Jangan

- Jangan menandai test skip.
