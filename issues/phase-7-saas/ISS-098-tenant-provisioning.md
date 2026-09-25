# ISS-098 — Provisioning Tenant Otomatis (Daftar → Bayar → Aktif)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-097
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Actions/Saas/ProvisionTenant.php`, `app/Console/`, `tests/`

## Konteks

Provisioning otomatis (plan #22, #28): setelah pendaftaran dibayar, tenant aktif tanpa campur tangan manusia. Dua jalur: (a) daftar → bayar gateway → aktif otomatis; (b) daftar → trial → tetap perlu konfirmasi manual pembayaran pertama.

## Tasks

- [ ] Action `ProvisionTenant::run(data)`: buat tenant + owner + settings dasar + website (template sesuai vertical) + subscription (TRIAL atau ACTIVE bila langsung bayar) + seeder konten awal vertical (menu/produk/layanan contoh sesuai template_type) — semuanya dalam DB transaction; idempotent (email/plate unik guard).
- [ ] Trigger: (1) webhook PAID untuk tagihan "pendaftaran" (invoice khusus type setup — kolom `type` enum `SUBSCRIPTION`, `SETUP` di subscription_invoices); (2) perpanjangan biasa tidak mem-provision ulang (guard status tenant).
- [ ] Command `saas:provision-check`: audit kesehatan — daftar tenant pending provisioning > 1 jam → alert log/Email platform (deteksi webhook hilang).
- [ ] Audit log seluruh langkah `ProvisionTenant` (created/paid/webhook diasosiasikan ke tenant) via `App\Support\Audit` konteks platform — acuan troubleshoot provisioning gagal.
- [ ] Seeder konten awal per vertical: menu contoh (kuliner), produk cetak + bahan (percetakan), layanan + sparepart (bengkel) — cukup 3–5 item agar website tidak kosong.
- [ ] Simulasi beban: command test provision 10 tenant sekaligus (loop) — tidak tabrakan nomor/slug (suffix otomatis).
- [ ] Test: provisioning lengkap via webhook; idempotent; tabrakan slug → suffix; command audit mendeteksi pending; konten awal sesuai vertical.

## Acceptance Criteria

- [ ] `php artisan test --filter=Provisioning` lulus.
- [ ] Manual sandbox: daftar (via ISS-101) → bayar → tenant aktif dengan konten awal.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan provisioning manual via panel hilang — tetap ada (untuk bantu pelanggan offline).
- Jangan provisioning tanpa pembayaran kecuali jalur trial eksplisit.
