# Log Review Phase 3–7 (Perbaikan Konsistensi Dokumen)

> Catatan: dokumen ini mencatat perbaikan hasil review konsistensi lintas issue untuk **Phase 3 (website builder) — ISS-046–059**, **Phase 4 (percetakan) — ISS-060–070**, **Phase 5 (kuliner) — ISS-071–080**, **Phase 6 (bengkel) — ISS-081–091**, dan **Phase 7 (SaaS) — ISS-092–106**. Semua perubahan hanya menyentuh dokumen (belum ada kode), dan tidak mengubah kontrak desain di `docs/state-machine-core-vertical.md` kecuali yang dicatat. Pola temuan: skema kolom tak terdefinisi, rujukan section keliru, dua sumber kebenaran, dan setting key tanpa pemilik.

---

## 1. Phase 3 — Website Builder (ISS-046–059)

Review Cakupan/Chain/Dependency. Temuan & perbaikan:

| Issue | Masalah | Perbaikan |
| --- | --- | --- |
| ISS-048 | Model Website belum eksplisit `BelongsToTenant`; aturan "segera hadir" belum seragam | Model + trait; aturan render section: **section aktif tanpa data/implementasi tidak merender wrapper kosong** + test |
| ISS-050 | Tema vendor: "3 template_type" vs keputusan tema; section config mengiklankan section yatim (harga, cara-order, booking, riwayat-servis, promo) | 4 tema (1 netral `default` + 3 vertical); aturan render section global + test |
| ISS-051 | Upload galeri 2MB vs standard 5MB; trait belum eksplisit | trait `BelongsToTenant` + 5MB (sinkron ISS-012) |
| ISS-052 / ISS-055 | Trait belum eksplisit | `BelongsToTenant` |
| ISS-046 | Kondisi fallback "segera hadir" ambigu (websites vs is_published) | Disatukan: `websites` tidak ada **ATAU** `is_published = false` |
| ISS-049 | Kunci maps tak konsisten; `show_prices` tanpa pemilik panel | Pakai `map_embed_url`/`map_lat`/`map_lng`; toggle `show_prices` ditugaskan ke tab Profil Usaha |
| ISS-057 | Kunci maps tak konsisten | `map_lat`/`map_lng` |
| ISS-010 (Fase 1) | Kunci maps tak konsisten | `map_lat`/`map_lng` + `map_embed_url` |
| plan.md:1629 | "3 varian tema" usang | "4 varian tema (1 netral + 3 vertical)" |

---

## 2. Phase 4 — Vertical Percetakan (ISS-060–070)

Temuan & perbaikan:

| Issue | Masalah | Perbaikan |
| --- | --- | --- |
| ISS-062 | Kolom `sent_at` dipakai ISS-063 tapi tak ada di migrasi `quotations` | Tambah `sent_at` datetime nullable |
| ISS-065 | Flag `near_deadline` dipakai ISS-066 tapi tak ada di migrasi `production_orders` | Tambah `near_deadline` boolean default false |
| ISS-064 | Kontradiksi potong stok: "Jangan potong stok di konversi" vs aturan core `CreateTransaction`; rincian konversi tak jelas kolom tujuan | Rincian → `transaction_items.detail` (dibuat ISS-025); stok produk ikut aturan core; stok bahan roll (`print_materials.stock`) informasional manual |
| ISS-061 | Formula Konteks (luas × qty + diskon) tidak cocok implementasi (`base_price`+`price_tiers`) | Konteks disamakan; rumus luas m² eksplisit backlog |
| ISS-060 | Dua sumber kebenaran vertical (`template_type` vs `vertical`); stok bahan ambigu | `tenants.vertical` = satu-satunya sumber; `print_materials.stock` = INFORMASIONAL |
| ISS-067 | Aturan duplikat surat jalan ambigu | Default 1 SJ/produksi; ulang = override dengan konfirmasi |

---

## 3. Phase 5 — Vertical Kuliner (ISS-071–080)

Temuan & perbaikan:

| Issue | Masalah | Perbaikan |
| --- | --- | --- |
| ISS-074 | Kolom `source` (KASIR/QR) dipakai ISS-079 tapi tak ada di migrasi `culinary_orders` | Tambah `source` enum (`KASIR`, `QR`; default `KASIR`) |
| ISS-071 | "Menu hanya tenant `template_type = KULINER`" vs keputusan single-source | `vertical = KULINER` (rujuk ISS-060) |
| ISS-072 | Rujukan kolom detail usang ("json/note item … bila perlu") | Pilihan varian/add-on → `transaction_items.detail` (kolom ISS-025), jangan kolom baru |
| ISS-075 | Ambang warning dapur tak bernama | Key setting `kitchen_warn_minutes` (default 15) |
| ISS-076 / ISS-073 | Setting key (`receipt_width`, `hide_unavailable`) tanpa pemilik | Ditugaskan audit owner di ISS-080 |
| ISS-079 | Rujukan kolom `source` (nullable di culinary_orders) usang | Sinkron: kolom enum sudah ada di ISS-074 |
| ISS-080 | Setting key vertical tak diaudit | Task audit baru (pemilik + kontrol UI, helper `setting()`) |

---

## 4. Phase 6 — Vertical Bengkel (ISS-081–091)

Temuan & perbaikan:

| Issue | Masalah | Perbaikan |
| --- | --- | --- |
| ISS-083 | `service_order_logs` dipakai ISS-087 untuk durasi tapi **tak pernah didefinisikan** di schema | Migration `create_service_order_logs_table` baru (pola `production_logs`) + `number` di-tambahkan `unique (tenant_id, number)` |
| ISS-083 | Rujukan doc keliru: "§5.5" = Retail, bukan bengkel | Guard & kepemilikan → §6 (sinkronisasi) + tabel action §5.5 |
| ISS-088 | `qc_checklist` kolom baru di `service_orders` tanpa migration eksplisit | Migration add-column eksplisit di ISS-088 |
| ISS-087 | "tambahkan log bila belum ada" (tak jelas pemilik) | Baca `service_order_logs` (dibuat ISS-083) |
| ISS-084 | Typo "estipasi" | → "estimasi" |
| ISS-091 | Config booking online & QC tanpa pemilik | Task audit setting/config key vertical |

---

## 5. Phase 7 — SaaS (ISS-092–106)

Temuan & perbaikan:

| Issue | Masalah | Perbaikan |
| --- | --- | --- |
| ISS-092 | `config/saas.php` dirujuk ISS-093 & ISS-099 tapi tak ada pemilik | ISS-092 jadi pemilik (grace_days=7, trial_days=14, base_domain, reserved subdomain) |
| ISS-092 | Rekening/instruksi bayar (dipakai ISS-095) tak punya rumah schema | Migration `platform_settings` (key-value global, BUKAN tenant); model tanpa `BelongsToTenant`; UI kelola di ISS-102 |
| ISS-092 | Seeder "nilai titik tengah" (klaim keliru — bukan midpoint) | "nilai placeholder dalam rentang plan.md (final keputusan owner)" |
| ISS-094 | Wording "periode berjalan" ambigu vs "periode berikutnya" ISS-095 | "periode baru hasil perpanjangan" |
| ISS-095 | Rujukan rekening platform tak eksplisit | Arahkan ke `platform_settings` (dibuat ISS-092); migration `manual_proof_path`/`submitted_at` lokal |
| ISS-102 | Tidak ada UI kelola `platform_settings` | Tab "Pengaturan Platform" (rekening, instruksi bayar, kontak) |
| ISS-093 | `config/saas.php` dianggap ada tanpa pemilik | Catatan "file dibuat oleh ISS-092" |

---

## 6. Prinsip/Keputusan yang Menjadi Acuan Lintas Fase

1. **Single-source vertical**: `tenants.vertical` (KULINER/PERCETAKAN/BENGKEL) adalah satu-satunya kebenaran. `websites.template_type` hanya sinyal website — dipakai untuk section & tema, bukan guard menu panel.
2. **`transaction_items.detail`** (dibuat ISS-025) adalah satu-satunya kolom tempat snapshot opsi/varian/bahan dari vertical — quotation percetakan, order menu kuliner, dst. Jangan tambah kolom baru.
3. **Setting key** dibuat dengan namanya **di issue pemakainya**, kelola UI-nya ditugaskan eksplisit (pola: `kitchen_warn_minutes`, `hide_unavailable`, `receipt_width`; audit di issue stabilisasi).
4. **Stok**: vertical yang "menjual produk" tetap potong stok lewat action core (`CreateTransaction`); kolom stok vertikal yang informasional harus ditandai (bahan roll percetakan).
5. **DocumentNumber**: penomoran dokumen vertical per tenant (`unique (tenant_id, number)`); penomoran platform/global (SUB) tanpa tenant.
6. **Isolasi platform vs tenant**: model vertical/platform diakses tanpa global-scope **secara eksplisit** (`withoutGlobalScope()`), jangan relasi tenant-scalar di tengah kode tenant.
7. **Status mesin**: dokumentasi `docs/state-machine-core-vertical.md` §5.x adalah kontrak; setiap issue yang menyentuh status wajib merujuk section yang benar dan memakai Action core eksplisit (bukan event/observer).

---

## 7. Ringkasan Angka

| Phase | Rentang | Jumlah issue | Perbaikan sesi ini |
| --- | --- | --- | --- |
| 3 (Website) | ISS-046–059 | 14 | 8 |
| 4 (Percetakan) | ISS-060–070 | 11 | 8 |
| 5 (Kuliner) | ISS-071–080 | 10 | 6 |
| 6 (Bengkel) | ISS-081–091 | 11 | 6 |
| 7 (SaaS) | ISS-092–106 | 15 | 7 |
| **Total** | | **61** | **35** |

> Status tersisa yang butuh keputusan owner (bukan konsistensi): harga paket seeder ISS-092 & setup/yearly (placeholder dalam rentang plan.md), provider payment gateway (ISS-096), domain kustom hanya PRO? (ISS-100), varian tema final (Phase 3).