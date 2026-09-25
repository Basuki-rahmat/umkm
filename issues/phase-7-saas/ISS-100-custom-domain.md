# ISS-100 — Domain Kustom Pelanggan (Verifikasi + SSL)

- **Tag**: `[AI+riviu]` — kebijakan paket (domain hanya PRO?) & operasi SSL perlu manusia
- **Depends on**: ISS-099
- **Perkiraan**: 1–1.5 hari
- **Area**: `database/migrations/`, `app/Actions/Saas/`, `resources/views/admin/`, `app/Console/`, `tests/`, `docs/`

## Konteks

Domain kustom (plan #8, #24): pelanggan PRO memakai domain sendiri (aset pelanggan). Alur: input domain → instruksi DNS → verifikasi kepemilikan → aktif → SSL. Untuk MVP, SSL via Caddy on-demand TLS (otomatis saat request pertama) — konfigurasi server oleh manusia.

## Tasks

- [ ] Migration `create_tenant_domains_table`: `id`, `tenant_id` FK, `domain` unique (lowercase), `status` (enum `PENDING_VERIFICATION`, `VERIFIED`, `ACTIVE`, `FAILED`), `verification_token`, `verified_at`, `last_checked_at`, timestamps.
- [ ] Panel tenant (PRO): tambah domain (1 aktif; tambahan = backlog), tampilkan instruksi DNS: A record → IP server (config) ATAU CNAME → `{slug}.umkmku.id` + TXT `_umkmku-verify.{domain}` = token (verifikasi kepemilikan).
- [ ] Command `domains:check` (scheduler per jam): cek DNS record domain PENDING (DNS query via `dns_get_record`); TXT cocok → VERIFIED; setelah verified & A/CNAME mengarah → ACTIVE (kolom `tenants.domain` di-update — dipakai middleware ISS-004).
- [ ] Domain gagal cek > 7 hari → status FAILED + catat; owner bisa retry.
- [ ] Middleware sudah resolve via `tenants.domain` (ISS-004) — test end-to-end.
- [ ] Guard paket: hanya subscription plan PRO (atau flag fitur `custom_domain` di plan.features) yang boleh tambah domain — lainnya paywall kecil.
- [ ] Docs `docs/custom-domain-setup.md`: instruksi untuk pelanggan (langkah DNS di registrar umum) + checklist SSL Caddy on-demand untuk manusia.
- [ ] Test: verifikasi TXT benar (mock DNS function — injectable); guard paket; domain duplikat lintas tenant ditolak; middleware resolve domain ACTIVE; failed flow.

## Acceptance Criteria

- [ ] `php artisan test --filter=CustomDomain` lulus.
- [ ] Manual (staging): domain test → tambah → set DNS → command → ACTIVE + SSL jalan.
- [ ] Docs lengkap; `pint` + `phpstan` lolos.

## Jangan

- Jangan API registrar/DNS provider otomatis (backlog).
- Jangan izinkan domain platform sendiri sebagai tenant domain.
