# ISS-086 — Booking Online dari Website Publik

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-085
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Public/BookingController.php`, `resources/views/public/booking/`, `resources/views/admin/workshop/`, `tests/`

## Konteks

Booking servis (plan #12): pelanggan pesan slot dari website bengkel → masuk sebagai work order status BOOKING → kasir konfirmasi saat datang (CHECK-IN).

## Tasks

- [ ] Halaman publik `/booking` (template bengkel): form — nama, telepon/WA, plat, merk/model, keluhan, pilih tanggal & slot jam (slot config: jam buka + durasi slot 30 menit, kapasitas per slot = jumlah mekanik aktif), kirim.
- [ ] Simpan: customer+vehicle dibuat/dipakai (match by plat; belum ada → buat baru), work order status `BOOKING` + booking_at; halaman sukses + tombol WA ke bengkel (pesan berisi ringkasan booking).
- [ ] Panel: daftar booking (filter tanggal), tombol "Konfirmasi" (BOOKING→CHECK-IN manual saat datang), "Tolak" (alasan) — tidak ada auto-confirm (bengkel harus sanggup).
- [ ] Anti-spam: rate limit per IP, honeypot, slot penuh ditolak dengan pesan sopan.
- [ ] Halaman booking hanya ada di template BENGKEL + section aktif.
- [ ] Test: booking sukses membuat WO BOOKING + kendaraan/pelanggan match/buat; slot penuh ditolak; hari libur/tutup ditolak (config jam buka); konfirmasi di panel; isolasi tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=Booking` lulus.
- [ ] Manual dari HP: booking → muncul di panel → konfirmasi → lanjut alur WO.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan reminder otomatis (buffer ISS-089).
- Jangan pembayaran/deposit online (Phase 7).
