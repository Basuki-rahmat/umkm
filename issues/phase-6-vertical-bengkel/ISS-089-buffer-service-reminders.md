# ISS-089 — Buffer: Pengingat Servis Berkala

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-088
- **Perkiraan**: 1 hari (opsional; tidak menghalangi ISS-090)
- **Area**: `app/Console/`, `app/Support/`, `tests/`

## Konteks

Buffer plan Phase 6: pengingat servis berkala (km/waktu) dari riwayat — modal untuk retensi pelanggan. MVP tetap klik-kirim WA manual, tapi kandidatnya dihitung otomatis.

## Tasks

- [ ] Config: interval pengingat per tenant (mis. tiap 3 bulan / 5.000 km — simpan setting tenant, default 90 hari).
- [ ] Command `workshop:compute-reminders` (scheduler harian): untuk tiap kendaraan aktif, hitung selisih hari & km sejak servis terakhir → kandidat pengingat (tabel/flag `reminders` in-memory per run atau tabel `service_reminders` sederhana: `id`, `tenant_id`, vehicle_id, due_date, due_km, notified_at nullable, timestamps).
- [ ] Panel: halaman "Pengingat Servis" — daftar kandidat (nama, plat, hari sejak servis, km sejak), tombol WA per baris (template pengingat ramah) + tombol "Tandai terkirim" (notified_at).
- [ ] Test: kandidat benar (border: tepat 90 hari); sudah notified tidak muncul lagi sampai servis baru; WA template benar; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=ServiceReminder` lulus.
- [ ] Manual: jalankan command → daftar muncul → kirim WA dari HP.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim WA otomatis/terjadwal (butuh gateway API = fase lanjutan).
- Jangan pengingat untuk kendaraan tanpa riwayat.
