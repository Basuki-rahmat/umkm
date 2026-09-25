# ISS-032 — Kirim Invoice via WhatsApp (wa.me) & Email Opsional

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-031
- **Perkiraan**: 0.5 hari
- **Area**: `app/Support/`, `resources/views/admin/transactions/`, `tests/`

## Konteks

Sesuai strategi WhatsApp plan (#17): fase MVP memakai **link wa.me + template pesan** (klik → WA terbuka dengan pesan terisi; invoice PDF dikirim manual oleh user). API di fase lanjutan.

## Tasks

- [ ] Helper `App\Support\WaLink::build(string $phone, string $message): string` — normalisasi nomor Indonesia ke `62…` (dari 08… / +62… / 62…), tanpa karakter non-digit, validasi panjang minimal.
- [ ] Template pesan invoice (dari plan): `Halo Bapak/Ibu {nama}. Pesanan #{nomor}. Total: Rp {total}. Status: {status}. Terima kasih.` — generate dari transaksi; tombol "Kirim via WhatsApp" di detail transaksi (target = telepon pelanggan).
- [ ] Tombol serupa untuk pengingat piutang (template berbeda: pengingat sisa tagihan + jatuh tempo).
- [ ] Log audit EXPORT setiap kali tombol WA diklik (buktikan follow-up pernah dilakukan).
- [ ] (Opsional) Kirim email invoice (mail bawaan, `Mail::fake` di test) — catat di audit log.
- [ ] Test: normalisasi nomor (08→62, +62→62, spasi/dash dihapus); template berisi semua placeholder terisi; audit log terekam.

## Acceptance Criteria

- [ ] `php artisan test --filter=WaLink|InvoiceShare` lulus.
- [ ] Manual dari HP: klik tombol → WA terbuka dengan pesan & nomor benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan pakai API/gateway berbayar (fase lanjutan).
- Jangan kirim otomatis tanpa klik user.
