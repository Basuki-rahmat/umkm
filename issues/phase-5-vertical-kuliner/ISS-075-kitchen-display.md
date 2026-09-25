# ISS-075 — Tampilan Dapur (Kitchen Display)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-074
- **Perkiraan**: 1–1.5 hari
- **Area**: `app/Http/Controllers/Admin/Kitchen/`, `resources/views/admin/kitchen/`, `tests/`

## Konteks

Kitchen status (plan #13): MENUNGGU → DIMASAK → SIAP → DIANTAR/DIAMBIL → SELESAI. Layar dapur = halaman khusus, font besar, auto-refresh, sederhana untuk dihubungi jempol oleh staf dapur. Sync terminal ke core: `docs/state-machine-core-vertical.md` §5.3.

## Tasks

- [ ] Migration `create_kitchen_orders_table`: `id`, `tenant_id`, `culinary_order_id` FK unique, `status` (enum `MENUNGGU`, `DIMASAK`, `SIAP`, `DIANTAR`, `DIAMBIL`, `SELESAI`, `CANCELLED`; default `MENUNGGU`), `started_at`, `ready_at`, `finished_at` nullable, timestamps. Auto-create saat culinary order dibuat.
- [ ] Halaman `/admin/kitchen` (role STAFF/OPERATOR/OWNER): kolom per status, card per order (nomor, tipe order, items + varian/add-on + catatan, waktu menunggu menit), tombol besar pindah status; DIANTAR hanya untuk ANTAR, DIAMBIL untuk lainnya.
- [ ] Auto-refresh 15 detik (meta refresh / fetch ringan; tanpa websocket), tanpa navigasi panel (layout khusus, minimal).
- [ ] Waktu: card berubah warna bila menunggu > `kitchen_warn_minutes` (setting, default 15).
- [ ] Sinkron: status SELESAI dapur → panggil `SetSettleReady` → `transactions.settle_ready = true` (flag + audit, bukan ubah paid_amount/status pembayaran).
- [ ] Test: alur status + transition liar ditolak; DIANTAR hanya ANTAR; auto-create; waktu menunggu benar; `SELESAI` → `settle_ready = true` & payment tak berubah; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Kitchen` lulus.
- [ ] Manual di tablet/HP layar besar: pindahkan 3 order antar kolom, angka waktu jalan.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan cetak apa pun dari dapur (struk = ISS-076).
- Jangan bump teknologi realtime (polling cukup).
