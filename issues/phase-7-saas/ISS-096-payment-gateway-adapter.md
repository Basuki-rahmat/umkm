# ISS-096 — Payment Gateway: Integrasi & Checkout Tagihan

- **Tag**: `[AI+riviu]` — pilihan provider & akun perlu manusia
- **Depends on**: ISS-095
- **Perkiraan**: 1.5–2 hari
- **Area**: `composer.json`, `app/Services/Gateway/`, `config/services.php`, `resources/views/admin/billing/`, `tests/`

## Konteks

Gateway untuk tagihan langganan (plan #22 kandidat: Midtrans/Xendit/Duitku — bandingkan MDR & payout sebelum memilih). Arsitektur: **adapter interface** supaya provider bisa berganti tanpa mengubah bisnis.

## Tasks

- [ ] Interface `PaymentGateway` (createCharge(invoice): redirect_url, handleWebhook(payload): PaymentResult) + adapter provider terpilih (Midtrans Snap default — SNAP token via server, bukan lib berat).
- [ ] Config `.env`: `PAYMENT_GATEWAY=midtrans`, server/client key, environment sandbox/production, `gateway.is_active` (false = tombol gateway disembunyikan, manual transfer tetap jalan).
- [ ] Checkout: tombol "Bayar via Gateway" di invoice UNPAID → buat charge (simpan `gateway_ref`, `payment_url` di subscription_invoices — migration) → redirect owner ke halaman bayar provider.
- [ ] Status invoice tetap UNPAID sampai webhook (ISS-097) mengkonfirmasi — tidak percaya redirect.
- [ ] Halaman invoice menampilkan status "menunggu pembayaran gateway" + tombol "cek status" (query API status — idempotent).
- [ ] Test (sandbox/mock): createCharge membuat ref & url; fallback saat gateway inactive; cek status memperbarui; interface — provider mock untuk test tanpa jaringan.

## Acceptance Criteria

- [ ] `php artisan test --filter=Gateway` lulus (mock).
- [ ] Manual sandbox: bayar tagihan uji via Snap → status berganti (setelah webhook ISS-097).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan simpan credential di kode/git — semua env.
- Jangan mark PAID dari redirect (hanya webhook/status API).
- Jangan dukung multi-provider sekaligus (satu aktif).
