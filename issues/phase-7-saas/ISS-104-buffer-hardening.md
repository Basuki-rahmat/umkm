# ISS-104 — Buffer: Hardening Billing & Webhook

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-103
- **Perkiraan**: 1 hari (opsional; boleh dipotong sebagian tanpa menghalangi ISS-106)
- **Area**: `app/Services/Gateway/`, `app/Console/`, `tests/`, `docs/`

## Konteks

Buffer plan Minggu 6 Phase 7: uji regresi siklus penuh, hardening webhook (retry/alert), dokumentasi operasional. Murni kualitas — tidak ada fitur.

## Tasks

- [ ] Test siklus penuh regensi (feature test panjang): TRIAL → bayar (gateway mock) → ACTIVE → period end → PAST_DUE (reminder H-7/H-1) → bayar telat → ACTIVE lagi; dan jalur EXPIRED → paywall → perpanjang → akses kembali. Dua skenario × monthly/yearly.
- [ ] Webhook hardening: retry handler untuk FAILED (command `webhooks:replay --failed` dengan backoff log), alert log/email platform saat FAILED > 5 dalam 1 jam (scheduler cek), cek idempotensi di bawah payload duplikat massal (loop 50×).
- [ ] Divergensi kecil yang muncul saat uji: dokumentasikan di `docs/ops-billing.md` — SOP: verifikasi manual (ISS-095), handle refund (review flow), handle komplain tagihan, cara replay webhook, cara memberi gratis bulan.
- [ ] Simulasi kegagalan gateway: matikan config gateway (inactive) di tengah siklus → manual transfer jalur tetap berfungsi.
- [ ] Test semua di atas; pastikan suite penuh tetap hijau.

## Acceptance Criteria

- [ ] Test siklus penuh lulus (2 skenario × 2 cycle).
- [ ] Replay command berjalan; alert menyala pada simulasi gagal beruntun.
- [ ] `docs/ops-billing.md` mencakup 5 SOP di atas.
- [ ] `pint` + `phpstan` + suite penuh hijau.

## Jangan

- Jangan tambah fitur baru (murni hardening).
- Jangan tulis SOP teori — harus sesuai implementasi aktual.
