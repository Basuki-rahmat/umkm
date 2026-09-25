# ISS-103 — Pengingat Perpanjangan (Email + WA Manual)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-102
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Console/`, `app/Mail/`, `app/Support/`, `tests/`

## Konteks

Grace period (ISS-093) bekerja lebih baik jika owner diingatkan sebelum jatuh tempo (plan #22): email otomatis (murah) + tombol WA manual oleh super admin (untuk pelanggan VIP).

## Tasks

- [ ] Command `saas:send-reminders` (scheduler harian): email ke owner tenant — H-7 dan H-1 sebelum period_end (invoice UNPAID ada), dan hari-1 grace period (PAST_DUE). Template email Indonesia + link ke halaman billing tenant. Dedup: 1 pengingat per (tenant, jenis, fase) — tabel `saas_reminders_sent` sederhana.
- [ ] Mail via queue (bila queue aktif) — dari config.
- [ ] Panel super admin: daftar "Perlu Follow-up WA" (tenant PAST_DUE + belum bayar) dengan tombol WaLink manual ke owner (template pesan perpanjangan) + tandai sudah dihubungi.
- [ ] Email bounces/kegagalan dicatat (log) — tanpa integrasi SMTP kompleks.
- [ ] Test: email terkirim di fase yang tepat & tidak dobel (dedup); link billing benar; daftar follow-up akurat; jalankan command 2× aman.

## Acceptance Criteria

- [ ] `php artisan test --filter=RenewalReminder` lulus.
- [ ] Manual: ubah tanggal → jalankan → email tampil (Mail::trap/log) → daftar WA follow-up benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan WA otomatis (masih manual — gateway WA fase lanjutan).
- Jangan spam: dedup ketat, maks 3 email per siklus.
