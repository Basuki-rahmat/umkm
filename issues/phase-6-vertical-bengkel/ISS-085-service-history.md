# ISS-085 — Riwayat Servis per Kendaraan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-084
- **Perkiraan**: 0.5 hari
- **Area**: `app/Http/Controllers/Admin/Workshop/`, `resources/views/admin/workshop/vehicles/`, `tests/`

## Konteks

Riwayat servis (plan #12 Output): kendaraan punya jejak lengkap — dasar pengingat berkala (buffer) & kepercayaan pelanggan.

## Tasks

- [ ] Detail kendaraan: timeline riwayat servis (work order SELESAI: tanggal, km, pekerjaan/items ringkas, total, mekanik) terbaru di atas.
- [ ] Ringkasan: total kunjungan, total pengeluaran, servis terakhir, km terakhir — kartu di atas timeline.
- [ ] Tombol "Buat Work Order Baru" dari detail kendaraan (prefill kendaraan + keluhan kosong).
- [ ] Riwayat juga tampil ringkas di detail pelanggan (semua kendaraannya, tab).
- [ ] Export riwayat per kendaraan ke PDF (cetak untuk pelanggan, opsional tapi mudah — reuse pattern).
- [ ] Test: timeline urut & hanya SELESAI (atau SELESAI + dibatalkan dengan penanda); ringkasan angka benar; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=ServiceHistory` lulus.
- [ ] Manual: 2 servis demo → timeline & ringkasan benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan edit/hapus riwayat (immutable log — audit trail).
- Jangan pengingat otomatis (buffer ISS-089).
