# ISS-065 — Status Produksi Pesanan Percetakan

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-064
- **Perkiraan**: 1–1.5 hari
- **Area**: `database/migrations/`, `app/Actions/Print/`, `app/Http/Controllers/Admin/`, `resources/views/admin/print/production/`, `tests/`

## Konteks

Workflow produksi (plan #11): MENUNGGU → DICETAK → FINISHING → QC → SIAP DIAMBIL/DIKIRIM → SELESAI. Status produksi terpisah dari status pembayaran transaksi; keduanya nyambung saat akhir. Aturan sync ke core: `docs/state-machine-core-vertical.md` §5.2 (`settle_ready` di terminal, `CloseTransaction` saat kasir selesaikan).

## Tasks

- [ ] Migration `create_production_orders_table`: `id`, `tenant_id`, `transaction_id` FK unique, `status` (enum `MENUNGGU`, `DICETAK`, `FINISHING`, `QC`, `SIAP`, `SELESAI`, `CANCELLED`; default `MENUNGGU`), `deadline` datetime nullable, `priority` (enum `NORMAL`, `BURUAN`; default `NORMAL`), `delivery_method` (enum `AMBIL_SENDIRI`, `KIRIM` nullable), `near_deadline` boolean default false (set oleh command ISS-066; sumber widget), timestamps.
- [ ] Migration `create_production_logs_table`: `tenant_id`, production_order_id, from_status, to_status, note nullable, user_id, created_at (jejak perpindahan + waktu lama per tahap).
- [ ] Auto-create production order (status MENUNGGU) saat transaksi percetakan dibuat (via action konversi/order kilat — deteksi tenant vertical PERCETAKAN).
- [ ] Halaman produksi: list per status dengan tombol pindah status (wajib note saat QC gagal kembali — validasi transition), filter deadline, penanda telat (merah jika deadline lewat & belum SELESAI).
- [ ] Saat status → SELESAI: panggil `SetSettleReady` → `transactions.settle_ready = true` (tidak mengubah paid_amount/status pembayaran — flag + audit log).
- [ ] Role: STAFF/OPERATOR bisa pindah status produksi; OWNER semua.
- [ ] Test: alur status benar + transition liar ditolak; telat terdeteksi; log tercatat; auto-create saat konversi; `SELESAI` → `settle_ready = true` & `paid_amount` tidak berubah; cancel → `CANCELLED`; isolasi.

## Acceptance Criteria

- [ ] `php artisan test --filter=Production` lulus.
- [ ] Manual dari HP (peran STAFF): pindah status pesanan → log & warna deadline benar.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim notifikasi (ISS-066).
- Jangan kanban UI drag-drop (list + tombol cukup; kanban = backlog).
