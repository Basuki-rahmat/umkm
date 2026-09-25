# ISS-102 — Panel Super Admin: Monitoring Langganan & MRR

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-101
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Platform/`, `resources/views/platform/saas/`, `tests/`

## Konteks

Super Admin memantau bisnis (plan #25): status semua langganan, MRR sederhana, tagihan — satu panel.

## Tasks

- [ ] Halaman "Langganan" (platform): tabel semua subscription (tenant, paket, cycle, status, period berakhir, MRR kontribusi), filter status/paket, search tenant; aksi: ubah paket manual, beri gratis x bulan (kolom `granted_free_months` di subscriptions → dipakai GenerateInvoice untuk skip), cancel.
- [ ] Kartu ringkasan: MRR aktif (Σ harga bulanan subscription ACTIVE; YEARLY = harga/12), jumlah tenant per status (TRIAL/ACTIVE/PAST_DUE/EXPIRED), tagihan UNPAID count + nominal.
- [ ] Grafik MRR 6 bulan (dari subscription_invoices PAID per bulan — Chart.js).
- [ ] Halaman "Webhook & Integrasi": daftar webhook_logs terakhir (status, invoice terkait), tombol "Proses ulang" untuk FAILED (replay handler secara aman).
- [ ] Halaman "Pengaturan Platform" (tab): kelola `platform_settings` (rekening & instruksi bayar untuk ISS-095, kontak platform) — key-value dibuat di ISS-092.
- [ ] Export Excel daftar langganan & tagihan (pola ISS-039).
- [ ] Test: MRR dihitung benar (campuran monthly/yearly); granted_free_months skip billing; replay webhook idempotent; angka kartu cocok seeded; isolasi platform.

## Acceptance Criteria

- [ ] `php artisan test --filter=SaasAdmin` lulus.
- [ ] Manual: panel menampilkan angka bisnis yang masuk akal dari data demo.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan prediksi churn/LTV (laporan sederhana saja).
- Jangan edit data keuangan langsung dari panel (hanya operasi baku: ubah paket, gratis bulan, cancel).
