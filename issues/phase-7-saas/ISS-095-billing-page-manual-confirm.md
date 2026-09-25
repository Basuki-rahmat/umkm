# ISS-095 — Halaman Billing Tenant + Verifikasi Manual Transfer

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-094
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Admin/Billing/`, `resources/views/admin/billing/`, `tests/`

## Konteks

Sebelum gateway aktif, pembayaran manual dulu (plan #22): owner upload bukti transfer → SUPER ADMIN verifikasi. Halaman billing juga menampilkan paket, status, riwayat.

## Tasks

- [ ] Halaman `/admin/billing` (owner): kartu langganan (paket, status, trial/period berakhir, banner PAST_DUE), daftar tagihan (nomor, periode, nominal, jatuh tempo, status), tombol "Bayar" per tagihan UNPAID.
- [ ] Form bayar manual: pilih metode (Transfer — rekening platform dari `platform_settings`, tabel dibuat ISS-092), upload bukti (AllowedUpload), catatan → invoice berstatus UNPAID + kolom `manual_proof_path`, `submitted_at` (migration tambahan di issue ini), menunggu verifikasi.
- [ ] Panel SUPER ADMIN: daftar tagihan menunggu verifikasi → lihat bukti → tombol "Konfirmasi" (invoice PAID + subscription diperpanjang via action ISS-094) / "Tolak" (alasan, bukti dibersihkan, owner dinotif di halaman billing).
- [ ] Rekening & instruksi bayar ditampilkan dari `platform_settings` (key-value global dibuat ISS-092; UI kelola di ISS-102).
- [ ] Upgrade paket: tombol "Ganti Paket" (BUSINESS↔PRO; STARTER↔BUSINESS) — berlaku di periode berikutnya (catat `plan_change_pending`), bukan prorata.
- [ ] Test: submit bukti → menunggu; konfirmasi → PAID + periode maju; tolak → kembali UNPAID dengan catatan; ganti paket pending benar; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=ManualBilling|PlanChange` lulus.
- [ ] Manual: submit bukti (owner) → verifikasi (super admin) → langganan diperpanjang.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pembayaran gateway di issue ini (ISS-096/097).
- Jangan prorata refund (backlog).
