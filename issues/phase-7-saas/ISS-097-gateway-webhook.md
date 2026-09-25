# ISS-097 — Webhook Gateway (Verifikasi, Idempotensi, Logging)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-096
- **Perkiraan**: 1 hari
- **Area**: `routes/`, `app/Http/Controllers/Webhook/`, `database/migrations/`, `tests/`

## Konteks

Webhook = sumber kebenaran pembayaran gateway (plan #22). Wajib: validasi signature, idempotent (notif berulang aman), log semua payload, retry aman.

## Tasks

- [ ] Route POST `/webhooks/payment` (tanpa CSRF, tanpa auth session) → controller webhook.
- [ ] Validasi signature (sesuai provider — Midtrans: sha512 order_id+status_code+gross_amount+serverKey) → invalid = 403 + log.
- [ ] Tabel `webhook_logs`: payload json, signature_valid bool, result (PROCESSED/IGNORED/FAILED), related invoice, created_at.
- [ ] Idempotensi: transaksi PAID (order_id sama) datang 2× → proses sekali (cek status invoice sebelum update); order yang tidak dikenal → IGNORED + log (bukan error 500).
- [ ] Handler: PAID/SETTLEMENT → invoice PAID (action ISS-094: periode maju, subscription ACTIVE); EXPIRE/CANCEL → tetap UNPAID + log; refund → flag manual review (tulis log + tandai invoice, jangan auto-refund).
- [ ] Anti-duplikasi pembayaran ganda: order_id beda untuk invoice sama → proses pertama PAID, kedua IGNORED + log review.
- [ ] Test: signature valid/invalid; idempotent 2×; order tidak dikenal; periode maju tepat sekali; refund → review flag.

## Acceptance Criteria

- [ ] `php artisan test --filter=Webhook` lulus.
- [ ] Manual sandbox: bayar uji → webhook masuk → invoice PAID & langganan perpanjang.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan percaya payload tanpa validasi signature.
- Jangan auto-refund (review manusia).
- Jangan log data sensitif penuh (mask credential/token).
