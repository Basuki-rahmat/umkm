# ISS-088 — Notifikasi WA "Mobil Siap" & QC Checklist

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-087
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Support/`, `resources/views/admin/workshop/`, `tests/`

## Konteks

Notifikasi WA (plan #12) fase MVP = klik manual: "mobil siap diambil" saat QC lulus. QC checklist sederhana sebelum PEMBAYARAN.

## Tasks

- [ ] Migration menambah `qc_checklist` json nullable di `service_orders` (add column, ISS-083 sudah dibuat) — daftar cek bawaan (config `workshop-qc.php`: mis. "Lampu", "Rem", "Kliping/kampas", "Bocor", "Test drive") + item tambahan manual; semua wajib dicentang sebelum lanjut PEMBAYARAN.
- [ ] Template WA: "Yth {nama}, {merk} {plat} Anda sudah selesai diservis dan siap diambil di {nama bengkel}. Total: Rp {total}." — tombol di detail WO (status QC/PEMBAYARAN), audit log.
- [ ] (Opsional) Sertakan link PDF ringkasan servis (signed URL 24 jam — pola ISS-078).
- [ ] Test: QC belum lengkap → lanjut ditolak; WA template terisi; tombol sesuai status; audit; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=QcChecklist|WorkshopWa` lulus.
- [ ] Manual: QC dari HP → lanjut → kirim WA → pemilik menerima pesan benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim otomatis (manual click).
- Jangan QC untuk work order tanpa items.
