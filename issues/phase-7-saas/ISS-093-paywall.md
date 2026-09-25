# ISS-093 — Paywall: Middleware Langganan (Trial/Expired/Grace)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-092
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Middleware/`, `bootstrap/app.php`, `resources/views/saas/`, `tests/`

## Konteks

Tenant expired diblokir panel sesuai kebijakan (plan #22): grace period 7 hari (warning) → paywall (read-only atau blokir penuh). SUPER ADMIN & halaman billing selalu bisa diakses.

## Tasks

- [ ] Middleware `CheckSubscription` (group web, setelah IdentifyTenant): hitung status — TRIAL aktif, ACTIVE, PAST_DUE (expired ≤ 7 hari: warning banner kuning + tetap akses), EXPIRED (> 7 hari: redirect halaman paywall "Langganan berakhir — perpanjang untuk lanjut", link ke billing; data TIDAK dihapus).
- [ ] Halaman paywall sederhana (nama, tanggal berakhir, tombol ke halaman billing tenant, kontak WA platform).
- [ ] Whitelist route: billing tenant, logout, profile — tetap accessible saat EXPIRED.
- [ ] Command `saas:update-subscription-status` (scheduler harian): TRIAL habis → PAST_DUE → EXPIRED; ACTIVE lewat period_end tanpa pembayaran → PAST_DUE dst (sumber kebenaran status, middleware hanya membaca).
- [ ] Tanpa subscription (tenant lama/demo) → perlakukan sebagai ACTIVE + warning log (migrasi lembut).
- [ ] Test: tiap status → perilaku benar (akses/warning/blokir); whitelist; command mengubah status tepat; SUPER ADMIN tidak terdampak; tenant tanpa subscription aman.

## Acceptance Criteria

- [ ] `php artisan test --filter=Paywall|SubscriptionStatus` lulus.
- [ ] Manual: ubah period_end ke masa lalu → warning; lewat grace → paywall.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan hapus/arsip data tenant saat expired (hanya blokir akses).
- Jangan hardcode grace period — config `saas.php` (grace_days=7, trial_days=14; file dibuat oleh ISS-092).
