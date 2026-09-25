# ISS-064 — Konversi Quotation → Pesanan + Order Kilat

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-063
- **Perkiraan**: 1 hari
- **Area**: `app/Actions/Print/`, `app/Http/Controllers/Admin/`, `tests/`

## Konteks

Dua jalur masuk pesanan percetakan: (1) quotation ACCEPTED → konversi 1 klik jadi transaksi, (2) order kilat langsung tanpa quotation (form transaksi biasa sudah cukup — issue ini memastikan rincian percetakan ikut terbawa).

## Tasks

- [ ] Action `ConvertQuotationToTransaction`: buat transaksi core dari quotation (items dibuat dari quotation_items, `name_snapshot` + rincian kombinasi bahan/ukuran/finishing disimpan di `transaction_items.detail` — kolom yang diadakan ISS-025 untuk konversi vertikal), status transaksi `PENDING`, quotation berstatus `ACCEPTED` + kolom `transaction_id` (migration add, nullable unique) → idempotent (1 quotation 1 transaksi). Potong stok produk mengikuti aturan core (`CreateTransaction`).
- [ ] Tombol "Jadikan Pesanan" di detail quotation ACCEPTED → redirect ke detail transaksi.
- [ ] Jalur order kilat: di form transaksi (Phase 2), tenant percetakan punya opsi per item "Lihat Kalkulator" → item dibuat dari PriceResult (detail tersimpan) — cukup tambahan field note/detail, tanpa ubah logika core.
- [ ] Upload file desain di detail transaksi (kolom tabel `transaction_attachments`: tenant_id, transaction_id, path, original_name, uploaded_by, timestamps; maks 10MB via AllowedUpload; folder `tenant-{id}/designs/`).
- [ ] Test: konversi membuat transaksi + items benar + quotation terkunci; konversi kedua ditolak; upload desain tersimpan & terunduh; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=QuotationConvert|DesignUpload` lulus.
- [ ] Manual: quotation → jadikan pesanan → detail transaksi tampil items + upload desain bekerja.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan potong stok bahan roll (`print_materials.stock`) di konversi — stok bahan dikelola manual owner (lihat ISS-060). Stok produk cetak (core) tetap dipotong sesuai aturan create transaksi.
- Jangan izinkan upload > 10MB / file non-desain.
