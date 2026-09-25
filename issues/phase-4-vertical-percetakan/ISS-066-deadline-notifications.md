# ISS-066 — Deadline Terpantau & Notifikasi Internal

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-065
- **Perkiraan**: 0.5–1 hari
- **Area**: `app/Console/`, `app/Support/`, `resources/views/admin/`, `tests/`

## Konteks

Plan #11: deadline adalah nyawa percetakan. Notifikasi internal dulu (dashboard), WA ke pelanggan opsional (meniru pola bengkel di fase 6).

## Tasks

- [ ] Widget dashboard percetakan: "Perlu Perhatian" — production orders: deadline ≤ hari ini & belum SIAP (dikelompokkan: terlambat / hari ini / besok), priority BURUAN di atas.
- [ ] Command `print:check-deadlines` (scheduler per jam di jam kerja 08–17): menandai produksi mendekati deadline (flag `near_deadline` bool) — sumber data widget, agar query widget murah.
- [ ] Badge angka di menu "Produksi" (jumlah terlambat + hari ini).
- [ ] (Opsional) template pesan WA "pesanan Anda sedang diproses / siap diambil" — tombol manual per pesanan dari detail produksi (WaLink), tanpa otomatis.
- [ ] Test: command menandai dengan benar (edge: deadline jam 23:59); widget tampil & terurut; WA link manual benar; performa widget (indexed).

## Acceptance Criteria

- [ ] `php artisan test --filter=Deadline` lulus.
- [ ] Manual: set deadline hari ini pada 1 pesanan → muncul di widget & badge.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim WA/email otomatis (fase lanjutan).
- Jangan scheduler di luar jam kerja (spam log).
