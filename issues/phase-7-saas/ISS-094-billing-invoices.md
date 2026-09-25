# ISS-094 — SaaS: Auto-Billing (Subscription Invoices)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-093
- **Perkiraan**: 1–1.5 hari
- **Area**: `database/migrations/`, `app/Actions/Saas/`, `app/Console/`, `tests/`

## Konteks

Tagihan langganan terbit otomatis (plan #22): scheduler harian membuat invoice untuk subscription yang period_end-nya mendekat, nominal sesuai plan & billing_cycle.

## Tasks

- [ ] Migration `create_subscription_invoices_table`: `id`, `tenant_id`, `subscription_id` FK, `type` (enum `SUBSCRIPTION`, `SETUP`; default `SUBSCRIPTION` — SETUP = tagihan pendaftaran/aktivasi awal di ISS-098), `number` (prefix `SUB`, DocumentNumber — counter platform/global, BUKAN per tenant), `amount` decimal(16,2), `period_start`, `period_end`, `due_date`, `status` (enum `UNPAID`, `PAID`, `CANCELLED`; default `UNPAID`), `paid_at` nullable, `payment_ref` nullable (metode/ref dari gateway/manual), timestamps.
- [ ] Action `GenerateSubscriptionInvoice` (idempotent: 1 period = 1 invoice — unique (subscription_id, period_start)); command `saas:generate-invoices` harian: buat invoice H-7 sebelum period_end (atau hari-H bila setelah due).
- [ ] Saat invoice PAID (dari ISS-095/097): subscription diperpanjang (current_period_* maju 1 bulan/tahun dari period_end terakhir), status ACTIVE; bila `plan_change_pending` → terapkan plan baru (digunakan untuk periode baru hasil perpanjangan — selaras "berlaku di periode berikutnya" ISS-095).
- [ ] PDF invoice langganan (template sederhana, kop platform — bukan tenant) + nomor.
- [ ] Audit log semua aktivitas billing (generate / PAID / CANCELLED / plan change) via `App\Support\Audit` konteks platform — untuk rekonsiliasi & komplain pelanggan.
- [ ] Test: invoice dibuat tepat H-7; idempotent; bayar → periode maju benar (MONTHLY/YEARLY); tidak dobel; isolasi platform vs tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=SaasBilling` lulus.
- [ ] Manual: jalankan command → invoice muncul dengan nominal & jatuh tempo benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pakai counter nomor tenant (invoice platform).
- Jangan prorata/kupon (backlog).
