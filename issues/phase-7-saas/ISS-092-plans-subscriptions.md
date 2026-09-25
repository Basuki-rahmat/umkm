# ISS-092 — SaaS: Plans & Subscriptions (Schema + Seeder Paket)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-091 (MVP Phase 1–6 selesai)
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Models/`, `database/seeders/`, `tests/`

## Konteks

Fase SaaS (plan #21, #22): platform mulai berlangganan. Paket & harga WAJIB dari tabel Model Harga plan.md: STARTER (setup 750rb, 100–150rb/bln, 750rb–1jt/th), BUSINESS (1,5jt; 199–250rb/bln; 1,5–2,5jt/th), PRO (3–5jt; 499rb+/bln; 3–5jt/th), CUSTOM (quotation).

## Tasks

- [ ] Migration `create_subscription_plans_table`: `id`, `code` (STARTER/BUSINESS/PRO/CUSTOM), `name`, `monthly_price` decimal(16,2), `yearly_price` decimal(16,2) nullable, **`setup_price` decimal(16,2) nullable** (biaya aktivasi awal; CUSTOM melakukan quotation manual — setup_price nullable), `max_users` int nullable, `max_storage_mb` int nullable, `features` json nullable, `is_active`, timestamps.
- [ ] Migration `create_subscriptions_table`: `id`, `tenant_id` FK, `plan_id` FK, `billing_cycle` (enum `MONTHLY`, `YEARLY`), `status` (enum `TRIAL`, `ACTIVE`, `PAST_DUE`, `EXPIRED`, `CANCELLED`; default `TRIAL`), `trial_ends_at`, `current_period_start`, `current_period_end`, `cancelled_at` nullable, **`granted_free_months` int default 0** (bonus from owner; ISS-097), **`plan_change_pending` boolean default false** (diminta owner, applied saat perpanjangan ISS-094), timestamps; index (tenant_id, status).
- [ ] Migration `add_plan_code_to_tenants` (string nullable) — denormalisasi untuk guard cepat.
- [ ] Buat `config/saas.php` (pemilik: issue ini): `grace_days = 7` & `trial_days = 14` (dipakai ISS-093), `base_domain` (umkmku.id) & keyword subdomain reserved (dipakai ISS-099). Nilai default langsung diisi di sini.
- [ ] Migration `create_platform_settings_table` (key-value global, BUKAN tenant): `id`, `key` string unique, `value` text, timestamps — rumah untuk rekening platform & instruksi bayar (kebutuhan ISS-095), diisi via panel platform (ISS-102). Model `PlatformSetting` **tanpa** trait `BelongsToTenant` (resource platform/central, diakses panel platform & halaman billing tenant).
- [ ] Seeder `PlanSeeder`: 3 paket dengan nilai placeholder di dalam rentang plan.md (nilai final keputusan owner): STARTER — monthly 125rb, yearly 900rb, setup 750rb; BUSINESS — monthly 250rb, yearly 2,5jt, setup 1,5jt; PRO — monthly 500rb, yearly 5jt, setup 3jt. **CUSTOM tidak di-seed aktif** (pembuatan manual/quotation di ISS-095).
- [ ] Helper `tenantSubscription()` di model Tenant; saat tenant baru dibuat (platform panel) → subscription TRIAL 14 hari otomatis (hook di action create tenant).
- [ ] Catatan model: `Subscription` & `SubscriptionInvoice` (tabel invoice dibuat di ISS-094 — model bisa didefinisikan belakangan) diakses juga dari panel platform (bukan scope tenant) — tetapkan pola tegas: panggil `withoutGlobalScope()`/variant global-scope secara eksplisit (individu) dan JANGAN menambal query dengan relasi tenant-scalar di tengah-tengah kode tenant (jaga isolasi lintas 403 — gate ISS-044).
- [ ] Test: seeder idempotent; tenant baru dapat TRIAL 14 hari; helper status benar (trial aktif/habis).

## Acceptance Criteria

- [ ] `php artisan migrate --seed` sukses; test lulus.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat UI billing (ISS-094/095).
- Jangan hubungkan ke payment gateway (ISS-096).
