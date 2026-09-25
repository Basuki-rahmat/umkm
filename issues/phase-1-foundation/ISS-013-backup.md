# ISS-013 — Backup Database & Storage

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-012
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Console/Commands/`, `config/`, `storage/backups/` (gitignored), dokumentasi

## Konteks

Backup minimal sesuai plan (#30): database + folder storage tenant. MVP memakai command artisan manual yang bisa dijadwalkan cron nanti (scheduler disiapkan, cron di server saat deploy).

## Tasks

- [ ] Command `backup:run`:
  - Dump database via `mysqldump` (konfigurasi dari `.env`) ke `storage/backups/db-YYYY-MM-DD-HHi.sql.gz` (gzip).
  - Zip folder `storage/app/public/tenant-*` ke `storage/backups/files-YYYY-MM-DD-HHi.zip`.
  - Tulis log ringkas (nama file, ukuran, durasi).
- [ ] Command `backup:clean --keep=7`: hapus backup lebih lama dari N (default 7 file terakhir per tipe).
- [ ] Daftarkan scheduler (daily 02:00) di `routes/console.php` — di-comment jika environment dev tidak siap cron.
- [ ] Tambah `storage/backups/` ke `.gitignore`.
- [ ] Test: command berjalan di environment dengan `mysqldump` tersedia; jika binary tidak ada → error message jelas (bukan crash aneh). Backup file terbentuk & berisi (cek ukuran > 0).
- [ ] Tulis `docs/backup-restore.md` singkat: cara backup, cara restore (`mysql < dump.sql`), lokasi file.
- [ ] Uji pemulihan sungguhan: restore dump & zip ke environment terpisah (DB test), lalu verifikasi hitung baris tabel kunci (users, transactions) & file tenant ada — tulis prosedur checkbox di `docs/backup-restore.md`; otomatisasi test restore dianggap bonus opsional.

## Acceptance Criteria

- [ ] `php artisan backup:run` menghasilkan 2 file (db + files) di `storage/backups/`.
- [ ] `php artisan backup:clean --keep=1` menyisakan 1 file per tipe.
- [ ] Dokumentasi restore ada dan akurat.

## Jangan

- Jangan upload ke cloud (S3 nanti, fase lanjutan).
- Jangan simpan credential di dalam command — pakai config dari `.env`.
