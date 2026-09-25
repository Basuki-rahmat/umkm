# ISS-099 — Subdomain Otomatis (*.umkmku.id)

- **Tag**: `[AI+riviu]` — setup DNS/wildcard server perlu manusia (akses provider)
- **Depends on**: ISS-098
- **Perkiraan**: 1 hari
- **Area**: `config/`, `app/Actions/Saas/`, `resources/views/onboarding/`, `tests/`, `docs/`

## Konteks

Subdomain otomatis (plan #22): wildcard DNS `*.umkmku.id` → satu A record ke server + wildcard SSL, sehingga tenant baru TIDAK butuh perubahan DNS per tenant. Yang perlu otomasi: validasi slug, reservasi, dan middleware resolusi (sudah ada dari ISS-004).

## Tasks

- [ ] Config `saas.php`: `base_domain` (umkmku.id), wildcard subdomain (list reserved: www, api, admin, mail, ftp, app, dashboard).
- [ ] Action `ReserveSubdomain`: validasi slug (regex, min 3, tidak reserved, unik di tenants.slug), lowercase; dipanggil saat provisioning (slug = dari nama usaha, konflik → suffix angka) & saat onboarding user pilih sendiri.
- [ ] Halaman publik cek ketersediaan (AJAX sederhana untuk onboarding ISS-101).
- [ ] Verifikasi infra: dokumen `docs/subdomain-setup.md` — record DNS wildcard, SSL wildcard (Let's Encrypt DNS-01 via Caddy/Certbot — butuh akses manusia ke provider domain), webserver config; checklist pemeriksaan.
- [ ] Middleware: pastikan subdomain baru langsung resolved tanpa restart (sudah by-design — tulis test end-to-end tenant baru → website live di subdomainnya dalam 1 request).
- [ ] Test: reserve valid/invalid/reserved/konflik → suffix; tenant baru → halaman publik live di subdomain tanpa langkah manual; central domain tak terganggu.

## Acceptance Criteria

- [ ] `php artisan test --filter=Subdomain` lulus.
- [ ] Manual: provisioning tenant baru → website live di `{slug}.umkmku.id` dengan SSL (staging).
- [ ] `docs/subdomain-setup.md` lengkap (untuk eksekusi oleh manusia).
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan API DNS provider di MVP (wildcard cukup — API automation = backlog).
- Jangan SSL per-subdomain (wildcard saja).
