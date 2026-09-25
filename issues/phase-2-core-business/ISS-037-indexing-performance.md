# ISS-037 — Indexing & Optimasi Query

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-036
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Models/`, laporan hasil

## Konteks

Uji dengan volume realistis (1.000 transaksi × 3 item = 3.000 item, 1 tenant + 50 pelanggan) sebelum lanjut. Menemukan N+1 & query lambat sekarang, bukan saat produksi.

## Tasks

- [ ] Command seeder `db:seed --class=VolumeDemoSeeder` (param `--transactions=1000`) — data tanggal tersebar 90 hari.
- [ ] Review semua halaman list & laporan: tambah index yang dipakai (`(tenant_id, created_at)`, `(tenant_id, status)`, `(tenant_id, paid_at)` pada payments, dst — buat migration index).
- [ ] Eager load semua relasi di list (customer, items count, dsb) — buktikan tanpa N+1 (`DB::listen` / query log assertion di test list transaksi & laporan).
- [ ] Pagination di SEMUA list (tanpa `all()` di halaman index).
- [ ] Ukur & catat waktu render (QueryLog total) untuk: dashboard, index transaksi, laporan penjualan → target < 200ms query time masing-masing.
- [ ] Dokumentasikan hasil di `docs/performance-baseline.md` (angka sebelum/sesudah index).

## Acceptance Criteria

- [ ] Volume seeder jalan; halaman utama tetap responsif (< 200ms query) pada volume tsb.
- [ ] Test N+1 (atau dokumentasi query log) menunjukkan jumlah query konstan.
- [ ] `migrate` sukses; `pint` + `phpstan` lolos.

## Jangan

- Jangan optimasi prematur di luar daftar (cache agresif, queue, dsb).
- Jangan hapus index lama tanpa alasan.
