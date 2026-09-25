# ISS-027 — Pembayaran: Migrasi & Pembayaran Cash/Transfer (Upload Bukti)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-026
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, `app/Actions/Payments/`, `tests/`

## Konteks

Metode MVP: Cash, Transfer (dengan upload bukti), QRIS statis (nominal dibayar bebas). Satu transaksi bisa dibayar bertahap (parsial) sampai lunas. Pembayaran sah pada transaksi `PENDING` (mis. DP percetakan); status transaksi TIDAK diubah oleh pembayaran (status = siklus order — `docs/state-machine-core-vertical.md` §3).

## Tasks

- [ ] Migration `create_payments_table`: `id`, `tenant_id`, `transaction_id` FK, `method` (enum `CASH`, `TRANSFER`, `QRIS`), `amount` decimal(16,2) > 0, `paid_at` date, `proof_path` nullable (bukti transfer), `status` (enum `PENDING`, `CONFIRMED`, `REJECTED`, `REFUNDED`; CASH & QRIS langsung CONFIRMED, TRANSFER default PENDING sampai diverifikasi; `REFUNDED` diset aksi refund ISS-043), `note` nullable, `confirmed_by` nullable, `created_by`, timestamps.
- [ ] Kolom `transactions.paid_amount` (sudah ada di migrasi ISS-025) diperbarui otomatis saat payment CONFIRMED / refund (ISS-043).
- [ ] Action `RecordPayment`: validasi amount ≤ sisa tagihan; hitung ulang `paid_amount`; refresh receivable (sisa/OPEN/SETTLED). Status transaksi TIDAK diubah di sini — `COMPLETED` hanya dari `CloseTransaction`. Kasus khusus `type=SALE` (tanpa fulfillmen): bayar lunas → kasir diarahkan tutup order (`CloseTransaction`) dalam satu alur "Bayar & Selesai".
- [ ] UI verifikasi: OWNER melihat pembayaran TRANSFER berstatus PENDING → tombol Konfirmasi/Tolak (tolak wajib alasan di note).
- [ ] Halaman "Pembayaran" di sidebar: list semua payment tenant (filter metode/status/tanggal).
- [ ] Test: cash langsung confirmed & paid_amount naik; transfer menunggu verifikasi; parsial: 2× bayar → lunas tepat; amount > sisa ditolak; DP saat `PENDING` → receivable `OPEN` (sisa benar) & status tetap `PENDING`; isolasi (konfirmasi payment tenant lain → 403).

## Acceptance Criteria

- [ ] `php artisan test --filter=Payment` lulus.
- [ ] Manual: bayar parsial 2 tahap → status lunas benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat receivables table (ISS-028).
- Jangan QRIS dinamis/gateway (Phase 7).
