# SISTEM DIGITALISASI UMKM SIAP PAKAI

> **Satu platform, disesuaikan dengan jenis usaha Anda.**

---

## 1. VISI PRODUK

Membangun platform **Sistem Digitalisasi UMKM Siap Pakai** yang memungkinkan berbagai jenis usaha menggunakan satu core application dengan modul dan workflow yang disesuaikan dengan karakteristik bisnis masing-masing.

Platform bukan sekadar website company profile, tetapi menjadi:

> **Sistem Operasional Digital untuk UMKM**

Konsep utama:

```text
INPUT
   ↓
PROSES BISNIS
   ↓
TRANSAKSI
   ↓
PEMBAYARAN
   ↓
OUTPUT
   ↓
LAPORAN
```

---

# 2. TUJUAN SISTEM

Sistem harus mampu:

* Membantu UMKM memiliki website profesional.
* Membantu UMKM mengelola operasional.
* Mengelola pelanggan.
* Mengelola produk dan jasa.
* Mengelola transaksi.
* Mengelola pembayaran.
* Menghasilkan invoice/nota.
* Menghasilkan laporan.
* Terintegrasi dengan WhatsApp.
* Mendukung domain sendiri.
* Mendukung subdomain.
* Mendukung multi-tenant.
* Mendukung berbagai jenis usaha.
* Memiliki sistem paket/langganan.
* Dapat dikembangkan menjadi SaaS.

---

# 3. PRINSIP ARSITEKTUR

## Jangan membuat 8 aplikasi terpisah

Platform menggunakan:

```text
                    UMKM DIGITAL
                         │
                  CORE APPLICATION
                         │
        ┌────────────────┼────────────────┐
        │                │                │
     Kuliner         Percetakan        Bengkel
        │                │                │
     Laundry            Travel           Toko
        │                │                │
      Fashion            Jasa            dll
        │                │                │
        └────────────────┼────────────────┘
                         │
                   DATABASE ENGINE
```

Semua jenis usaha menggunakan core engine yang sama.

Yang berbeda adalah:

* template
* menu
* field
* workflow
* laporan
* tampilan
* modul khusus

---

# 4. VERTICAL USAHA

Target vertical awal:

1. 🍜 Kuliner
2. 🖨️ Percetakan
3. 🚗 Bengkel

Vertical tahap berikutnya:

4. 🧺 Laundry
5. 🏪 Toko
6. 👕 Fashion
7. 🏠 Jasa
8. 🚌 Travel

---

# 5. MATRIKS INPUT → PROSES → OUTPUT

| Vertical       | Input                    | Proses        | Output            |
| -------------- | ------------------------ | ------------- | ----------------- |
| 🍜 Kuliner     | Menu, harga, bahan       | Pesanan       | Struk/order       |
| 👕 Fashion     | Produk, ukuran, warna    | Stok          | Katalog & laporan |
| 🖨️ Percetakan | Bahan, ukuran, pelanggan | Produksi      | Invoice & nota    |
| 🚗 Bengkel     | Kendaraan, sparepart     | Servis        | Work order        |
| 🧺 Laundry     | Pelanggan, kiloan        | Status cucian | Nota laundry      |
| 🚌 Travel      | Rute, jadwal, penumpang  | Booking       | E-ticket          |
| 🏠 Jasa        | Paket, pelanggan         | Pekerjaan     | Invoice           |
| 🏪 Toko        | Produk, stok             | Penjualan     | Struk & laporan   |

---

# 6. CORE SYSTEM

Semua tenant mendapatkan core system berikut:

```text
AUTHENTICATION
├── Login
├── Logout
├── Register
├── Reset Password
└── Role & Permission

DASHBOARD
├── Ringkasan transaksi
├── Pendapatan
├── Piutang
├── Pesanan
└── Statistik

MASTER DATA
├── Pelanggan
├── Produk
├── Jasa
├── Kategori
├── Satuan
└── Harga

TRANSAKSI
├── Pesanan
├── Penjualan
├── Pembelian
├── Pembayaran
└── Pembatalan

KEUANGAN
├── Pembayaran
├── Piutang
├── Pengeluaran
└── Ringkasan keuangan

DOKUMEN
├── Invoice
├── Nota
├── Kwitansi
└── Surat Jalan

LAPORAN
├── Penjualan
├── Pembayaran
├── Pelanggan
├── Produk
├── Piutang
└── Keuangan

KOMUNIKASI
├── WhatsApp (fase awal: link wa.me + template pesan; API di fase lanjutan)
├── Email
└── Notifikasi

WEBSITE PUBLIK
├── Home
├── Tentang
├── Produk/Jasa
├── Galeri
├── Artikel
├── Kontak
└── Google Maps
```

---

# 7. SISTEM MULTI-TENANT

Platform harus dirancang sebagai aplikasi multi-tenant.

Satu instalasi/core application dapat melayani banyak UMKM.

Contoh:

```text
Tenant #001
Toko Maju Jaya

Tenant #002
Percetakan Lampung

Tenant #003
Bengkel Makmur

Tenant #004
Laundry Bersih
```

Setiap tenant mempunyai data yang terisolasi.

Minimal setiap tabel transaksi harus memiliki:

```text
tenant_id
```

Contoh:

```text
users
tenants
customers
products
transactions
transaction_items
payments
invoices
expenses
```

## Mekanisme Isolasi Tenant

Isolasi ditegakkan di level aplikasi (Laravel):

* **Global scope** pada setiap model milik tenant sehingga semua query otomatis terfilter `tenant_id`.
* **Trait `BelongsToTenant`** yang menerapkan global scope dan mengisi `tenant_id` otomatis saat create.
* **Policy/Authorization** per model untuk mencegah akses langsung lewat ID (IDOR).
* Pengujian khusus: percobaan akses data tenant lain **harus** menghasilkan 403/404.

Tidak boleh ada tenant melihat data tenant lain. Ini merupakan persyaratan keamanan utama. (Detail keamanan lanjutan: bagian Keamanan.)

---

# 8. DOMAIN DAN SUBDOMAIN

## Model A — Domain milik pelanggan

Pelanggan memiliki domain sendiri.

Contoh:

```text
tokomaju.com
percetakanabc.id
bengkeljaya.com
```

Platform menyediakan konfigurasi domain.

Keuntungan:

* domain merupakan aset pelanggan
* lebih profesional
* pelanggan dapat mempertahankan domain jika berhenti berlangganan

---

## Model B — Subdomain platform

Platform menyediakan:

```text
tokomaju.umkmku.id
percetakanabc.umkmku.id
bengkeljaya.umkmku.id
```

Model ini cocok untuk paket entry.

## Upgrade dan Otomatisasi

Pelanggan dapat upgrade ke domain sendiri:

```text
namatoko.com
namatoko.id
namatoko.co.id
```

Platform membantu konfigurasi DNS.

Otomatisasi domain/subdomain (provisioning, DNS, SSL) adalah fitur fase lanjutan — lihat MVP vs Fitur Lanjutan.

---

# 9. STRUKTUR WEBSITE PUBLIK

Setiap tenant mempunyai website publik.

Contoh:

```text
HOME
│
├── Hero
├── Tentang Usaha
├── Produk/Jasa
├── Keunggulan
├── Galeri
├── Testimoni
├── Artikel
├── Lokasi
└── WhatsApp
```

Website harus responsive:

```text
Mobile
Tablet
Desktop
```

---

# 10. TEMPLATE WEBSITE

Setiap vertical memiliki template.

## Template Kuliner

```text
Home
Menu
Kategori
Promo
Galeri
Lokasi
WhatsApp
```

## Template Percetakan

```text
Home
Layanan
Produk
Portfolio
Harga
Cara Order
Request Penawaran
Kontak
```

## Template Bengkel

```text
Home
Layanan
Sparepart
Booking Servis
Riwayat Servis
Promo
Lokasi
Kontak
```

---

# 11. MODUL KHUSUS PERCETAKAN

Percetakan menjadi salah satu vertical prioritas.

## Input

```text
Pelanggan
Produk
Bahan
Ukuran
Finishing
Jumlah
Harga
Deadline
Catatan
File desain
```

## Proses

```text
Quotation
    ↓
Pesanan
    ↓
Produksi
    ↓
Quality Control
    ↓
Selesai
    ↓
Pembayaran
    ↓
Pengiriman / Diambil
```

## Output

```text
Quotation
Invoice
Nota
Surat Jalan
Status Pesanan
Laporan Penjualan
Laporan Produksi
Laporan Piutang
```

---

# 12. MODUL KHUSUS BENGKEL

## Input

```text
Pelanggan
Kendaraan
Nomor Polisi
Merk
Tipe
Kilometer
Keluhan
Sparepart
Mekanik
```

## Proses

```text
Booking
    ↓
Check-in
    ↓
Pemeriksaan
    ↓
Estimasi
    ↓
Persetujuan
    ↓
Servis
    ↓
Quality Check
    ↓
Pembayaran
    ↓
Selesai
```

## Output

```text
Work Order
Estimasi Biaya
Invoice
Nota
Riwayat Servis
Laporan Mekanik
Laporan Pendapatan
```

---

# 13. MODUL KHUSUS KULINER

## Input

```text
Menu
Kategori
Harga
Bahan
Varian
Tambahan
```

## Proses

```text
Order
    ↓
Konfirmasi
    ↓
Produksi
    ↓
Siap
    ↓
Pembayaran
    ↓
Selesai
```

## Output

```text
Struk
Order
Laporan Penjualan
Laporan Menu
Laporan Pendapatan
```

---

# 14. VERTICAL TAHAP BERIKUTNYA

## Laundry

```text
Pelanggan
Jenis layanan
Berat
Harga
Tanggal masuk
Estimasi selesai
Status cucian
Pembayaran
Nota
```

Status:

```text
DITERIMA
↓
DICUCI
↓
DIKERINGKAN
↓
DISETRIKA
↓
SELESAI
↓
DIAMBIL
```

---

## Toko

```text
Produk
Kategori
Stok
Supplier
Pembelian
Penjualan
Pembayaran
Retur
Laporan
```

---

## Fashion

```text
Produk
Ukuran
Warna
Varian
Stok
Harga
Penjualan
Katalog
```

---

## Jasa

```text
Paket jasa
Pelanggan
Project
Deadline
Progress
Pembayaran
Invoice
```

---

## Travel

```text
Rute
Jadwal
Armada
Penumpang
Booking
Pembayaran
E-ticket
Laporan
```

---

# 15. ROLE DAN PERMISSION

Minimal:

```text
SUPER ADMIN
ADMIN TENANT
OWNER
KASIR
STAFF
OPERATOR
```

Contoh:

```text
OWNER
├── Dashboard
├── Laporan
├── Keuangan
└── Pengaturan

KASIR
├── Pelanggan
├── Transaksi
├── Pembayaran
└── Cetak Nota

STAFF
├── Produk
├── Pesanan
└── Produksi
```

Permission harus dapat dikembangkan secara modular.

---

# 16. DASHBOARD OWNER

Dashboard harus menampilkan:

```text
┌───────────────────────────────────┐
│ PENJUALAN HARI INI                │
├───────────────────────────────────┤
│ Rp ........                       │
└───────────────────────────────────┘

┌───────────────┬───────────────────┐
│ PESANAN       │ PIUTANG           │
│ 25            │ Rp ........       │
└───────────────┴───────────────────┘

┌───────────────────────────────────┐
│ GRAFIK PENJUALAN                  │
└───────────────────────────────────┘

TRANSAKSI TERBARU
```

---

# 17. WHATSAPP

Integrasi WhatsApp menjadi salah satu fitur utama.

## Strategi Implementasi Bertahap

| Fase | Metode | Catatan |
| --- | --- | --- |
| MVP | Link `wa.me` + template pesan | Manual, tanpa biaya API, klik untuk kirim |
| Fase lanjutan | Gateway pihak ketiga (mis. Fonnte/Wablas) atau WhatsApp Business API resmi | Perlu biaya & approval; putuskan saat dibutuhkan |

Contoh alur:

```text
Pesanan baru
↓
Generate pesan
↓
WhatsApp
```

Template:

```text
Halo Bapak/Ibu {nama}.

Pesanan #{invoice}
Total: Rp {total}

Status: {status}

Terima kasih.
```

Fungsi:

* konfirmasi order
* invoice
* pengingat pembayaran
* status pesanan
* promosi
* follow-up pelanggan

---

# 18. INVOICE

Invoice harus dapat:

* dibuat otomatis
* memiliki nomor unik
* PDF
* print
* dikirim melalui WhatsApp
* dikirim melalui email

Contoh:

```text
INV/2026/000001
```

---

# 19. SISTEM PEMBAYARAN

## Metode (MVP)

```text
Cash
Transfer
QRIS statis
```

## Metode (Fase Lanjutan)

```text
QRIS dinamis
E-wallet
Payment Gateway (kandidat: Midtrans / Xendit / Duitku)
```

Status:

```text
PENDING
PAID
PARTIAL
CANCELLED
REFUNDED
```

---

# 20. LAPORAN

Core reporting:

```text
Laporan Penjualan
Laporan Pembayaran
Laporan Piutang
Laporan Pelanggan
Laporan Produk
Laporan Pengeluaran
Laporan Pendapatan
```

Laporan dapat:

```text
View
Print
PDF
Excel
```

---

# 21. MODEL HARGA

Model bisnis utama: **setup + langganan bulanan**, dengan opsi bayar tahunan (diskon). Semua harga dalam satu tabel:

| Paket | Fokus | Setup | Langganan / bulan | Opsi Tahunan |
| --------------- | ------------------------------ | --------------------- | ----------------------- | ----------------------------- |
| 🟢 STARTER | Website UMKM | Rp750.000 | Rp100.000–150.000 | Rp750.000–1.000.000 / tahun |
| 🔵 BUSINESS | Website + sistem operasional | Rp1.500.000 | Rp199.000–250.000 | Rp1.500.000–2.500.000 / tahun |
| 🟣 PRO | Aplikasi bisnis vertical | Rp3.000.000–5.000.000 | Rp499.000+ | Rp3.000.000–5.000.000 / tahun |
| ⚫ CUSTOM | Pengembangan khusus | Custom quotation | Custom quotation | Custom quotation |

## 🟢 STARTER — Website UMKM

Untuk UMKM yang baru ingin online.

Fitur:

* Website
* Domain/subdomain
* Hosting
* Profil usaha
* Katalog
* WhatsApp
* Google Maps
* Galeri
* Basic SEO

Belum termasuk aplikasi manajemen usaha penuh.

## 🔵 BUSINESS — Website + Sistem Operasional

Fitur:

* Semua Starter
* Dashboard
* Pelanggan
* Produk/Jasa
* Transaksi
* Invoice
* Laporan
* WhatsApp
* Backup
* Maintenance dasar

## 🟣 PRO — Aplikasi Bisnis Vertical

Untuk usaha yang membutuhkan sistem lebih lengkap.

Fitur:

* Semua Business
* Modul vertical
* Workflow khusus
* Multi-user
* Role & permission
* Laporan lanjutan
* Integrasi tambahan
* Backup berkala
* Maintenance
* Dukungan teknis

## Catatan Harga

Harga final dapat disesuaikan dengan:

* jumlah user
* kapasitas storage
* domain
* fitur
* integrasi
* kebutuhan custom
* volume transaksi

Biaya operasional yang memengaruhi harga:

* domain
* hosting
* storage
* email
* WhatsApp
* payment gateway
* server
* maintenance
* support

---

# 22. RECURRING REVENUE

Pendapatan tidak hanya berasal dari pembuatan aplikasi.

Sumber pendapatan:

```text
Setup
│
├── Domain
├── Hosting
├── Subscription
├── Maintenance
├── Custom Module
├── Additional User
├── Additional Storage
├── Integrasi
└── Support
```

Target jangka panjang:

> **Recurring Revenue > One Time Project Revenue**

---

# 23. DATABASE CORE

Konsep awal:

```text
tenants
users
roles
permissions

customers
suppliers

categories
products
services
units

transactions
transaction_items
payments

invoices
invoice_items

expenses
receivables

settings
websites
domains

notifications
articles
galleries

subscriptions
subscription_plans
```

Vertical-specific tables ditambahkan sesuai kebutuhan.

Contoh:

```text
workshops
vehicles
service_orders
mechanics
```

Untuk bengkel.

Atau:

```text
laundry_orders
laundry_services
laundry_statuses
```

Untuk laundry.

---

# 24. ISOLASI DATA & KEAMANAN TENANT

## Prinsip Isolasi

```text
USER
 ↓
TENANT
 ↓
DATA TENANT
```

Query harus selalu memperhatikan:

```text
tenant_id
```

Tidak boleh ada tenant melihat data tenant lain.

Ini merupakan persyaratan keamanan utama.

## Keamanan Wajib

* HTTPS
* Password hashing
* CSRF protection
* SQL injection protection
* XSS protection
* Session security
* Role & permission
* Tenant isolation
* Audit log
* Backup
* Rate limiting
* Validasi upload
* Pembatasan file type
* Pembatasan ukuran file

---

# 25. ADMIN MASTER PLATFORM

Super Admin memiliki:

```text
Dashboard

Tenant
├── Daftar tenant
├── Detail tenant
├── Status
├── Paket
└── Subscription

Users

Plans

Payments

Domains

Servers

System Logs

Support Tickets

Reports
```

---

# 26. ONBOARDING TENANT

Alur:

```text
DAFTAR
  ↓
PILIH JENIS USAHA
  ↓
PILIH TEMPLATE
  ↓
ISI DATA USAHA
  ↓
UPLOAD LOGO
  ↓
INPUT PRODUK/JASA
  ↓
PILIH DOMAIN
  ↓
PEMBAYARAN
  ↓
AKTIVASI
  ↓
WEBSITE ONLINE
```

Tujuan:

> UMKM dapat memiliki sistem online tanpa harus memahami teknologi.

---

# 27. TEMPLATE ENGINE

Template harus dapat dikonfigurasi.

Contoh:

```text
template_type = kuliner
```

kemudian sistem menentukan:

```text
menu
produk
order
meja
promo
```

Sedangkan:

```text
template_type = bengkel
```

menentukan:

```text
kendaraan
servis
mekanik
sparepart
work_order
```

---

# 28. CUSTOM FIELD

Sistem sebaiknya mendukung custom field agar tidak semua kebutuhan harus membuat tabel baru.

Contoh:

```text
field_name
field_type
field_value
```

Jenis:

```text
text
number
date
select
checkbox
file
textarea
```

Ini akan membantu pengembangan vertical baru.

---

# 29. FILE & MEDIA MANAGEMENT

Setiap tenant dapat memiliki:

```text
Logo
Foto produk
Foto galeri
Dokumen
Invoice
File desain
Lampiran transaksi
```

Storage harus dipisahkan berdasarkan tenant:

```text
/storage/
    tenant-001/
    tenant-002/
    tenant-003/
```

---

# 30. BACKUP

Backup minimal:

```text
Database
File upload
Konfigurasi
```

Strategi:

```text
Daily Backup
Weekly Backup
Monthly Backup
```

Backup harus dapat dipulihkan.

---

# 31. AUDIT LOG

Sistem mencatat aktivitas penting:

```text
LOGIN
CREATE
UPDATE
DELETE
PAYMENT
EXPORT
PRINT
LOGIN FAILED
```

Contoh:

```text
24-09-2026 10:15
Admin
Update Produk
Produk: Banner 3x2
```

---

# 32. TEKNOLOGI

Stack utama (final):

```text
Backend
Laravel 13 (rilis 17 Maret 2026, wajib PHP 8.3+)

Database
MySQL / MariaDB

Frontend
Blade
Bootstrap / Tailwind

JavaScript
Vanilla JS / Alpine.js

PDF
DomPDF

Authentication
Laravel Authentication

Storage
Local / S3-compatible

Web Server
Nginx / Apache

Cache
Redis (optional)

Queue
Redis / Database Queue

Admin Panel (opsional, mempercepat Phase 1)
Filament
```

Node.js/Express **bukan** teknologi wajib — hanya optional service untuk fase lanjutan (mis. kebutuhan realtime khusus), tidak dipakai dari awal.

---

# 33. RESPONSIVE DESIGN

Prioritas:

```text
Mobile First
```

Target:

```text
Android
iPhone
Tablet
Desktop
```

Dashboard harus tetap nyaman digunakan pada layar kecil.

---

# 34. MVP vs FITUR LANJUTAN

Jangan langsung membangun seluruh sistem.

## Cakupan MVP

```text
MVP
│
├── Core Platform
├── Multi Tenant
├── Website UMKM
├── Transaksi Dasar
├── Invoice
├── Pembayaran
├── Laporan Dasar
└── 3 Vertical Awal
     ├── Percetakan
     ├── Kuliner
     └── Bengkel
```

Rincian MVP:

```text
CORE
├── Authentication
├── Tenant
├── User
├── Role
├── Dashboard
├── Customer
├── Product
├── Transaction
├── Payment
├── Invoice
├── Report
└── Website
```

## Fitur Lanjutan

```text
FITUR LANJUTAN
│
├── SaaS Subscription
├── Domain Automation
├── WhatsApp API
├── Payment Gateway
├── Mobile/PWA
├── Advanced Reporting
├── Inventory
├── Accounting
├── AI
├── Additional Vertical
└── Marketplace/Ecosystem
```

## Kriteria Sukses MVP

MVP dianggap berhasil jika:

* Admin dapat membuat tenant.
* Tenant dapat login.
* Tenant dapat mengatur profil.
* Tenant dapat mengatur produk/jasa.
* Tenant dapat memasukkan pelanggan.
* Tenant dapat membuat transaksi.
* Sistem dapat membuat invoice.
* Sistem dapat mencatat pembayaran.
* Sistem dapat menghasilkan laporan.
* Website publik dapat ditampilkan.
* Data antar tenant terisolasi.
* Sistem dapat digunakan melalui HP.

---

# 35. ROADMAP & PRIORITAS DEVELOPMENT

Urutan prioritas dikombinasikan dengan fase:

```text
P0 — WAJIB
├── Authentication
├── Tenant
├── Database
├── Security
└── Core transaction

P1 — PRODUK UTAMA
├── Customer
├── Product
├── Invoice
├── Payment
├── Dashboard
└── Report

P2 — NILAI JUAL
├── Website
├── WhatsApp
├── PDF
├── Template
└── Domain

P3 — VERTICAL
├── Percetakan
├── Kuliner
└── Bengkel

P4 — SaaS
├── Subscription
├── Billing
├── Provisioning
└── Domain automation
```

## Ringkasan Estimasi (Solo Developer, full-time)

| Phase | Fokus | Durasi | PJ |
| --- | --- | --- | --- |
| Phase 1 | Foundation (P0) | 3–4 minggu | Developer |
| Phase 2 | Core Business (P1) | 6–8 minggu | Developer |
| Phase 3 | Website Builder (P2) | 4–5 minggu | Developer |
| Phase 4 | Vertical Percetakan (P3) | 3–4 minggu | Developer |
| Phase 5 | Vertical Kuliner (P3) | 2–3 minggu | Developer |
| Phase 6 | Vertical Bengkel (P3) | 3–4 minggu | Developer |
| Phase 7 | SaaS (P4) | 4–6 minggu | Developer |
| Phase 8 | Additional Vertical | 2–4 minggu per vertical | Developer |

**Total MVP (Phase 1–6): ±21–28 minggu (5–7 bulan).**

Catatan estimasi:

* Asumsi: 1 developer full-time, durasi sudah termasuk testing dan deployment dasar.
* Sudah termasuk buffer ±20% untuk bug fix dan revisi kecil.
* Phase 8 dikerjakan bertahap setelah MVP stabil, durasi dihitung per vertical.
* Jika menambah orang, kandidat yang paling menguntungkan untuk didelegasikan: Phase 3 (website builder) dan styling template vertical.

## PHASE 1 — FOUNDATION (P0)

Estimasi: 3–4 minggu · PJ: Developer

> **Implementasi**: fase ini (dan fase berikutnya) dipecah menjadi issue kecil yang siap dieksekusi junior dev / model AI — lihat `issues/README.md` dan `issues/CONVENTIONS.md`.

* Project setup
* Database
* Authentication
* Tenant
* User
* Role
* Permission
* Admin dashboard
* Settings
* Security dasar (tenant isolation)

### Checklist Mingguan — Phase 1

> Centang langsung di file ini. Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Project Setup & Fondasi

- [ ] Install Laravel 13 (PHP 8.3+), konfigurasi `.env`, timezone `Asia/Jakarta`, locale `id`
- [ ] Inisialisasi Git repo + konvensi branch/commit sederhana
- [ ] Setup database MySQL/MariaDB + migrasi bawaan (users, cache, jobs)
- [ ] Install paket inti: Filament (opsional; alternatif Blade + Alpine), spatie/laravel-permission, laravel/pint, larastan
- [ ] Setup storage lokal + `storage:link` (struktur per tenant: `/storage/tenant-{id}/`)
- [ ] Buat sketsa ERD core (tenants, users, roles, settings) sesuai bagian Database Core
- [ ] Setup testing (PHPUnit/Pest) + 1 test smoke yang berjalan
- [ ] (Opsional) CI sederhana: pint + larastan + test

#### Minggu 2 — Multi-Tenant & Authentication

- [ ] Migrasi + model `tenants` (nama, slug, status, paket)
- [ ] Trait `BelongsToTenant`: global scope + auto-isi `tenant_id` saat create
- [ ] Middleware identifikasi tenant dari subdomain/domain
- [ ] Kolom `tenant_id` di tabel users; user terikat tenant
- [ ] Flow auth: login, logout, register admin tenant, reset password
- [ ] Halaman pendaftaran tenant sederhana (nama usaha + akun owner) — versi MVP manual
- [ ] Unit test: query lintas tenant terfilter otomatis (tidak bocor)
- [ ] Seeder: 1 tenant demo + user owner

#### Minggu 3 — Role, Permission & Admin Dashboard

- [ ] Setup spatie/laravel-permission + seeder role: SUPER ADMIN, ADMIN TENANT, OWNER, KASIR, STAFF, OPERATOR
- [ ] Gate/policy dasar + middleware proteksi route per role
- [ ] Panel admin: layout, navigasi, menu dinamis per role
- [ ] Dashboard ringkasan versi awal: widget placeholder (penjualan hari ini, pesanan, piutang)
- [ ] CRUD user dalam tenant + assignment role
- [ ] Panel Super Admin: CRUD tenant, ubah status/paket
- [ ] Uji manual semua role: menu & akses sesuai haknya

#### Minggu 4 — Settings, Keamanan & Stabilisasi

- [ ] Modul settings tenant: nama usaha, logo, kontak, alamat, sosmed, koordinat maps
- [ ] Validasi upload (tipe & ukuran file) untuk logo
- [ ] Rate limiting login + audit log awal (LOGIN, CREATE, UPDATE, DELETE, LOGIN FAILED)
- [ ] Script backup (dump DB + folder storage)
- [ ] Uji isolasi end-to-end: 2 tenant dummy, akses data lintas tenant harus 403/404
- [ ] Bersihkan larastan/pint, perbaiki bug, rapikan seeders
- [ ] Deploy awal ke VPS (Nginx + SSL) atau siapkan environment demo — opsional
- [ ] Review Phase 1, buat backlog task Phase 2

#### Kriteria Selesai Phase 1 (Definisi Selesai)

- [ ] Tenant baru dapat dibuat dari panel super admin dan langsung login
- [ ] Semua query model tenant otomatis terfilter `tenant_id` (terbukti lewat test)
- [ ] Role bawaan berfungsi: menu & route terproteksi
- [ ] Settings tenant tersimpan & tampil di panel
- [ ] Audit log mencatat login & perubahan data
- [ ] Larastan/pint lolos tanpa error

## PHASE 2 — CORE BUSINESS (P1)

Estimasi: 6–8 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 28 issue (ISS-018 … ISS-045) di `issues/phase-2-core-business/` — lihat `issues/README.md`.

* Customer
* Product
* Service
* Category
* Transaction
* Payment
* Invoice
* Expense
* Receivable
* Reports
* Dashboard

### Checklist Mingguan — Phase 2

> Centang langsung di file ini. Estimasi inti 6 minggu; minggu 7–8 sebagai buffer (Excel export, performance, UAT). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Master Data: Pelanggan & Kategori

- [ ] Migrasi `customers` (tenant_id, nama, telepon, email, alamat, catatan) + `categories` (tenant_id, nama, tipe)
- [ ] Model + trait `BelongsToTenant` untuk keduanya
- [ ] CRUD pelanggan: list (search, filter, pagination), create, edit, delete (soft delete)
- [ ] CRUD kategori + validasi unik per tenant
- [ ] Form request & validasi (telepon Indonesia, email)
- [ ] Feature test: CRUD + isolasi tenant untuk kedua modul

#### Minggu 2 — Master Data: Produk, Jasa & Satuan

- [ ] Migrasi `products` (tenant_id, nama, sku, kategori, satuan, harga beli, harga jual, stok, min-stok, foto), `services` (tenant_id, nama, harga, deskripsi), `units`
- [ ] CRUD produk + upload foto (validasi tipe & ukuran, simpan di `/storage/tenant-{id}/`)
- [ ] CRUD jasa + satuan
- [ ] Stok dasar: perubahan stok tercatat (tabel `stock_movements` sederhana) — dasar untuk modul inventory fase lanjutan
- [ ] Halaman stok menipis (di bawah min-stok)
- [ ] Seeder data demo 1 tenant (5 pelanggan, 10 produk, 3 jasa)
- [ ] Feature test CRUD + isolasi tenant

#### Minggu 3 — Transaksi & Pesanan

- [ ] Migrasi `transactions` (tenant_id, nomor, tipe, customer_id, status, subtotal, diskon, pajak, total, catatan, dibuat_oleh) + `transaction_items` (transaction_id, product/service, qty, harga, diskon, subtotal)
- [ ] Generator nomor transaksi per tenant per periode (contoh: TRX/202609/0001)
- [ ] Flow buat pesanan: pilih pelanggan → tambah item (produk/jasa) → hitung otomatis → simpan
- [ ] Status transaksi: PENDING → SELESAI / CANCELLED (+ alasan batal)
- [ ] DB transaction + locking stok saat simpan (hindari stok minus)
- [ ] List transaksi: filter status/tanggal/pelanggan, detail transaksi
- [ ] Feature test: hitung total, potong stok, batalkan pesanan (stok kembali)

#### Minggu 4 — Pembayaran & Piutang

- [ ] Migrasi `payments` (tenant_id, transaction_id/invoice_id, metode, jumlah, tanggal, bukti, status)
- [ ] Metode MVP: Cash, Transfer, QRIS statis (+ upload bukti transfer)
- [ ] Pembayaran parsial: sisa tagihan terhitung otomatis
- [ ] Migrasi `receivables` (tenant_id, transaction_id, sisa, jatuh tempo, status) — terbuat otomatis saat transaksi belum lunas
- [ ] Halaman piutang: daftar belum lunas, tandai lunas, umur piutang
- [ ] Status pembayaran: PENDING/PAID/PARTIAL/CANCELLED/REFUNDED diterapkan konsisten
- [ ] Feature test: lunas, parsial, refund, sinkronisasi piutang

#### Minggu 5 — Invoice, Nota & Kwitansi

- [ ] Migrasi `invoices` + `invoice_items` (tenant_id, nomor unik, status) — contoh format: INV/2026/000001 per tenant
- [ ] Generate invoice otomatis dari transaksi; kwitansi dari pembayaran
- [ ] Template PDF via DomPDF (kop tenant dari settings: logo, alamat, kontak)
- [ ] Halaman print-friendly nota/struk
- [ ] Kirim invoice: link wa.me dengan template pesan (MVP) + email (opsional, log kirim)
- [ ] Feature test: nomor invoice unik per tenant, PDF tergenerate

#### Minggu 6 — Pengeluaran, Laporan & Dashboard Final

- [ ] CRUD `expenses` (tenant_id, kategori, jumlah, tanggal, catatan, bukti)
- [ ] Laporan inti: penjualan, pembayaran, piutang, pelanggan, produk, pengeluaran, pendapatan (filter periode, view + print + PDF)
- [ ] Dashboard owner final: penjualan hari ini, pesanan aktif, piutang, grafik 30 hari, transaksi terbaru
- [ ] Ringkasan keuangan sederhana: pendapatan − pengeluaran per periode
- [ ] Rapikan larastan/pint, perbaiki bug
- [ ] Review Phase 2, buat backlog Phase 3

#### Minggu 7–8 (Buffer, opsional)

- [ ] Export Excel untuk semua laporan
- [ ] Optimasi query + indexing kolom tenant_id/tanggal (coba dengan 1.000 transaksi dummy)
- [ ] Perbaikan UX dari uji coba nyata: coba kelola 1 transaksi lengkap dari HP
- [ ] UAT dengan 1 calon tenant (vertical percetakan) — catat feedback untuk Phase 4

#### Kriteria Selesai Phase 2 (Definisi Selesai)

- [ ] Tenant dapat CRUD pelanggan, produk, jasa dari panel
- [ ] Transaksi dapat dibuat, dibayar (penuh/parsial), dan dibatalkan dengan stok & piutang yang konsisten
- [ ] Invoice PDF dengan nomor unik per tenant dapat dibuat dan dicetak
- [ ] Semua laporan inti tampil sesuai filter periode dan tenant
- [ ] Dashboard menampilkan angka yang benar dan nyaman dibuka dari HP
- [ ] Semua modul baru lolos test isolasi tenant

## PHASE 3 — WEBSITE BUILDER (P2)

Estimasi: 4–5 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 14 issue (ISS-046 … ISS-059) di `issues/phase-3-website-builder/` — lihat `issues/README.md`.

* Landing page
* Template
* Logo
* Galeri
* Produk
* Artikel
* Kontak
* WhatsApp (wa.me)
* Google Maps
* SEO

### Checklist Mingguan — Phase 3

> Centang langsung di file ini. Estimasi inti 4 minggu; minggu 5 sebagai buffer (promo, polish, audit). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Fondasi Website Publik & Routing Tenant

- [ ] Routing website publik per tenant (subdomain/domain) + controller publik terpisah dari panel admin
- [ ] Layout template dasar responsive (Blade + Tailwind/Bootstrap), mobile-first
- [ ] Struktur data `websites` (tenant_id, template_type, tema/warna, konfigurasi section)
- [ ] Halaman Home: hero (judul, tagline, CTA WhatsApp), keunggulan, section dinamis dari `websites`
- [ ] Halaman Tentang + Kontak (mengambil data dari settings tenant: alamat, telepon, email, sosmed)
- [ ] Meta dasar + favicon per tenant
- [ ] Test: dua tenant menampilkan konten masing-masing di subdomainnya

#### Minggu 2 — Template Engine, Logo & Galeri

- [ ] Template engine sederhana: `template_type` (kuliner/percetakan/bengkel) menentukan menu & section yang tampil
- [ ] 4 varian tema (1 netral `default` + 3 sesuai vertical — warna/tata letak)
- [ ] Upload & kelola logo + identitas visual (warna brand) dari panel
- [ ] CRUD Galeri: upload multi-foto, urutan, keterangan + halaman galeri publik
- [ ] Section Testimoni: CRUD sederhana + tampilan di Home
- [ ] Kompresi gambar otomatis saat upload (mis. Intervention Image)
- [ ] Feature test CRUD + isolasi tenant

#### Minggu 3 — Katalog Produk/Jasa & Artikel

- [ ] Halaman Produk/Jasa publik: daftar per kategori, detail, opsi tampil/sembunyikan harga
- [ ] Tombol WhatsApp per produk/jasa (pesan berkonteks: "Saya tertarik dengan {nama_produk}")
- [ ] Tampilan katalog per template: menu kuliner per kategori, portfolio percetakan, layanan bengkel
- [ ] Migrasi `articles` (tenant_id, judul, slug, isi, featured image, status, tanggal) + CRUD panel
- [ ] Halaman artikel publik: daftar + detail + pagination
- [ ] Seeder konten demo untuk 3 tenant (kuliner, percetakan, bengkel)
- [ ] Feature test + isolasi tenant

#### Minggu 4 — Kontak, WhatsApp, Maps & SEO

- [ ] Google Maps embed dari koordinat/alamat di settings
- [ ] Tombol WhatsApp mengapung di semua halaman (wa.me, nomor & template pesan dari settings)
- [ ] Form kontak/penawaran: simpan ke `contact_messages`, tampil di panel owner + email notifikasi opsional
- [ ] SEO dasar: title & meta description per halaman, sitemap.xml, robots.txt, Open Graph, URL slug bersih
- [ ] Google Analytics / Search Console (opsional, kunci via settings)
- [ ] Performance: cache config/route, ukuran halaman ringan di jaringan lambat
- [ ] Review Phase 3, buat backlog Phase 4 (Vertical Percetakan)

#### Minggu 5 (Buffer, opsional)

- [ ] Halaman Promo + banner (CRUD sederhana)
- [ ] Uji coba nyata: isi konten asli untuk 1 tenant demo (logo, foto, artikel) dari HP
- [ ] Audit Lighthouse mobile: skor ≥ 80 untuk performa & SEO
- [ ] Ceklis onboarding: dari isi data → website online, siap dipakai di Phase 7

#### Kriteria Selesai Phase 3 (Definisi Selesai)

- [ ] Setiap tenant punya website publik online di subdomainnya sendiri
- [ ] Semua konten (hero, tentang, produk, galeri, testimoni, kontak) dapat diatur dari panel tanpa sentuh kode
- [ ] Tombol WhatsApp berfungsi dari semua halaman dengan pesan berkonteks
- [ ] Website nyaman dibuka di HP (mobile-first, gambar terkompresi)
- [ ] SEO dasar terpasang: sitemap, meta, Open Graph
- [ ] 3 template vertical tampil berbeda sesuai `template_type`

## PHASE 4 — VERTICAL PERCETAKAN (P3)

Estimasi: 3–4 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 11 issue (ISS-060 … ISS-070) di `issues/phase-4-vertical-percetakan/` — lihat `issues/README.md`.

* Bahan
* Ukuran
* Finishing
* Quotation
* Order
* Produksi
* Deadline
* Piutang
* Pengiriman

### Checklist Mingguan — Phase 4

> Centang langsung di file ini. Estimasi inti 3 minggu; minggu 4 sebagai buffer (laporan produksi, penyesuaian lapangan). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Master Data Percetakan

- [ ] Migrasi `print_materials` (tenant_id, nama, satuan, stok, harga dasar) + CRUD panel
- [ ] Migrasi `print_sizes` (tenant_id, nama, panjang, lebar, satuan) + CRUD panel
- [ ] Migrasi `print_finishings` (tenant_id, nama, harga tambahan, deskripsi) + CRUD panel
- [ ] Kombinasi harga: bahan + ukuran + finishing + jumlah → kalkulasi estimasi harga otomatis
- [ ] Relasi ke `products` core (produk percetakan = kombinasi bahan/ukuran/finishing)
- [ ] Feature test: kalkulasi harga kombinasi + isolasi tenant

#### Minggu 2 — Quotation & Order

- [ ] Migrasi `quotations` (tenant_id, customer_id, status, total, valid_until, catatan) + `quotation_items` (bahan/ukuran/finishing/jumlah/harga)
- [ ] Alur quotation: buat penawaran → kirim ke pelanggan (PDF + wa.me) → status DRAFT/TERKIRIM/DITERIMA/DITOLAK/KADALUWARSA
- [ ] Konversi quotation → transaksi pesanan (1 klik, data items ikut pindah)
- [ ] Input pesanan langsung tanpa quotation (order kilat) via transaksi core
- [ ] Upload file desain pelanggan di pesanan (validasi tipe/ukuran, simpan `/storage/tenant-{id}/designs/`)
- [ ] Feature test: konversi quotation → order, upload file desain

#### Minggu 3 — Produksi, Pengiriman & Status

- [ ] Status produksi pesanan: MENUNGGU → DICETAK → FINISHING → QC → SIAP DIAMBIL/DIKIRIM → SELESAI (+ catatan per status)
- [ ] Deadline & prioritas pesanan: tanggal deadline, penanda telat/tercepat
- [ ] Notifikasi internal: pesanan mendekati/melewati deadline (list di dashboard)
- [ ] Halaman produksi (kanban sederhana / list per status) untuk staf
- [ ] Surat jalan PDF (nomor unik, item, penerima)
- [ ] Piutang percetakan: integrasi dengan receivables core (DP saat order, pelunasan saat ambil)
- [ ] Laporan khusus: penjualan & produksi per periode, per jenis produk
- [ ] Review Phase 4, buat backlog Phase 5

#### Minggu 4 (Buffer, opsional)

- [ ] Kalkulator harga publik di website (pelanggan hitung sendiri, hasil masuk quotation DRAFT)
- [ ] Riwayat pesanan ulang (repeat order 1 klik dari riwayat)
- [ ] Laporan kinerja mesin/jenis cetak (opsional, dari data produksi)
- [ ] Uji coba nyata: 1 order lengkap dari quotation sampai pengambilan, dari HP
- [ ] Perbaikan UX berdasarkan uji coba

#### Kriteria Selesai Phase 4 (Definisi Selesai)

- [ ] Tenant percetakan dapat membuat quotation, mengirimnya, dan mengonversinya jadi pesanan
- [ ] Kalkulasi harga otomatis dari kombinasi bahan + ukuran + finishing + jumlah
- [ ] Status produksi berjalan sampai selesai dengan deadline terpantau
- [ ] Surat jalan PDF dapat dicetak saat pengambilan/pengiriman
- [ ] DP dan pelunasan tercatat di piutang core
- [ ] File desain pelanggan tersimpan rapi per tenant dan bisa diunduh

## PHASE 5 — VERTICAL KULINER (P3)

Estimasi: 2–3 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 10 issue (ISS-071 … ISS-080) di `issues/phase-5-vertical-kuliner/`.

* Menu
* Variant
* Add-on
* Order
* Kitchen status
* Payment
* Struk

### Checklist Mingguan — Phase 5

> Centang langsung di file ini. Estimasi inti 2 minggu; minggu 3 sebagai buffer (integrasi meja & QR, polish). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Menu, Varian & Add-on

- [ ] Migrasi `menu_items` (tenant_id, kategori, nama, deskripsi, harga, foto, tersedia) + CRUD panel
- [ ] Migrasi `menu_variants` (tenant_id, menu_item_id, nama varian, harga tambahan) — level pedas, ukuran, dsb
- [ ] Migrasi `menu_addons` (tenant_id, menu_item_id, nama, harga) + pilihan multiple saat order
- [ ] Tersedia/Habis toggle cepat di panel (langsung tampil di website publik)
- [ ] Relasi menu_items ke products core untuk transaksi
- [ ] Feature test: varian & add-on ikut ke perhitungan transaksi, isolasi tenant

#### Minggu 2 — Order, Kitchen Status & Struk

- [ ] Flow order kuliner: pilih menu → varian/add-on → catatan per item → simpan transaksi
- [ ] Tipe order: DI TEMPAT / BUNGKUS / ANTAR (tipe antar menyimpan alamat)
- [ ] Kitchen status: MENUNGGU → DIMASAK → SIAP → DIANTAR/DIAMBIL → SELESAI (tampilan dapur sederhana, layar besar)
- [ ] Halaman dapur auto-refresh (mis. polling/JS sederhana)
- [ ] Struk PDF thermal (58mm/80mm) via DomPDF + struk printer-friendly
- [ ] Pembayaran cepat: bayar saat order atau bayar saat selesai
- [ ] Laporan khusus: penjualan per menu, per varian, per tipe order, jam sibuk
- [ ] Review Phase 5, buat backlog Phase 6

#### Minggu 3 (Buffer, opsional)

- [ ] Meja & QR order (tabel `dining_tables`, kode meja, order dari QR web publik) — modal untuk fitur lanjutan
- [ ] Resep/dummy bahan (opsional, dari modul bahan kuliner di fase lanjutan)
- [ ] Integrasi struk ke WhatsApp (kirim struk digital ke pelanggan)
- [ ] Uji coba nyata: 1 hari operasional penuh (order → dapur → struk → laporan) dari HP
- [ ] Perbaikan UX dari uji coba

#### Kriteria Selesai Phase 5 (Definisi Selesai)

- [ ] Menu dengan varian & add-on dapat diatur dan ikut terhitung di transaksi
- [ ] Order DI TEMPAT/BUNGKUS/ANTAR berjalan dengan status dapur yang benar
- [ ] Struk thermal tercetak/dapat dicetak
- [ ] Laporan per menu/varian/tipe order tersedia
- [ ] Toggle tersedia/habis menu langsung terlihat di website publik

## PHASE 6 — VERTICAL BENGKEL (P3)

Estimasi: 3–4 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 11 issue (ISS-081 … ISS-091) di `issues/phase-6-vertical-bengkel/`.

* Customer
* Vehicle
* Mechanic
* Sparepart
* Service
* Work Order
* Service history
* Invoice

### Checklist Mingguan — Phase 6

> Centang langsung di file ini. Estimasi inti 3 minggu; minggu 4 sebagai buffer (pengingat servis, QR riwayat). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Master Data Bengkel

- [ ] Migrasi `vehicles` (tenant_id, customer_id, nomor polisi, merk, tipe, tahun, warna, catatan) + CRUD panel
- [ ] Unik per tenant: nomor polisi, riwayat servis terikat kendaraan (bukan cuma pelanggan)
- [ ] Migrasi `mechanics` (tenant_id, nama, spesialis, status aktif) + CRUD panel
- [ ] Migrasi `spareparts` (tenant_id, nama, kode, harga jual, stok, min-stok) + CRUD panel
- [ ] Relasi spareparts ke products core (agar pembelian & laporan stok core terpakai)
- [ ] Feature test isolasi tenant

#### Minggu 2 — Work Order & Servis

- [ ] Migrasi `service_orders` (tenant_id, vehicle_id, mechanic_id, keluhan, status, km, estimasi biaya, total) + `service_order_items` (sparepart/jasa, qty, harga)
- [ ] Alur bengkel: BOOKING → CHECK-IN → PEMERIKSAAN → ESTIMASI → PERSETUJUAN → SERVIS → QC → PEMBAYARAN → SELESAI
- [ ] Booking online dari website publik (form booking bengkel → service order BOOKING)
- [ ] Estimasi biaya → persetujuan pelanggan (checkbox/CTA pelanggan setuju)
- [ ] Pekerjaan tanpa persetujuan tidak bisa lanjut ke SERVIS
- [ ] Feature test: alur status, total = sparepart + jasa, isolasi tenant

#### Minggu 3 — Riwayat, Laporan & Notifikasi WA

- [ ] Riwayat servis per kendaraan (tampil di panel + lihat riwayat pelanggan via WA wa.me)
- [ ] Laporan: pendapatan per mekanik, per jenis servis, per periode; sparepart terlaris
- [ ] Kirim notifikasi WA: pesanan selesai, mobil siap diambil (wa.me + template pesan)
- [ ] QC checklist sederhana sebelum status PEMBAYARAN
- [ ] Review Phase 6, buat backlog Phase 7

#### Minggu 4 (Buffer, opsional)

- [ ] Pengingat servis berkala (km/waktu, dari riwayat) via WA terjadwal — modal untuk fase lanjutan
- [ ] QR riwayat servis pelanggan (halaman publik per kendaraan, akses via link unik)
- [ ] Uji coba nyata: 1 servis lengkap dari booking sampai mobil diambil, dari HP
- [ ] Perbaikan UX dari uji coba

#### Kriteria Selesai Phase 6 (Definisi Selesai)

- [ ] Work order mengikuti alur BOOKING sampai SELESAI dengan persetujuan estimasi wajib
- [ ] Riwayat servis terikat kendaraan dan dapat dilihat mekanik/owner
- [ ] Laporan per mekanik & jenis servis tersedia
- [ ] Notifikasi WA "mobil siap diambil" berfungsi
- [ ] Booking online dari website publik masuk sebagai service order

## PHASE 7 — SaaS (P4)

Estimasi: 4–6 minggu · PJ: Developer

> **Implementasi**: dipecah menjadi 15 issue (ISS-092 … ISS-106) di `issues/phase-7-saas/` — penutup roadmap Phase 1–7.

* Subscription
* Plans
* Payment gateway
* Domain management
* Subdomain automation
* Tenant provisioning
* Automated onboarding

### Checklist Mingguan — Phase 7

> Centang langsung di file ini. Estimasi inti 5 minggu; minggu 6 sebagai buffer (hardening billing, penyesuaian harga/paket). Asumsi solo developer, 1 minggu ≈ 5 hari kerja.

#### Minggu 1 — Plans & Subscription

- [ ] Migrasi `subscription_plans` (nama STARTER/BUSINESS/PRO/CUSTOM, harga sesuai bagian Model Harga, fitur, limit user/storage, aktif)
- [ ] Migrasi `subscriptions` (tenant_id, plan_id, status TRIAL/AKTIF/EXPIRED/CANCELLED, mulai, berakhir)
- [ ] Seeder 3 paket + harga dari tabel Model Harga (#21)
- [ ] Middleware cek langganan: tenant expired diblokir panel (mode read-only atau paywall)
- [ ] Trial 14 hari otomatis untuk tenant baru
- [ ] Feature test: status langganan memblokir/mengizinkan akses dengan benar

#### Minggu 2 — Billing & Pembayaran Langganan

- [ ] Migrasi `subscription_invoices` (tenant_id, subscription_id, jumlah, jatuh tempo, status)
- [ ] Tagihan bulanan/tahunan otomatis (scheduler/queue)
- [ ] Halaman billing tenant: riwayat tagihan, status, cara bayar
- [ ] Konfirmasi pembayaran manual (upload bukti transfer) + verifikasi super admin
- [ ] Perpanjangan otomatis vs manual (setting per tenant)
- [ ] Grace period 7 hari setelah expired (email/WA pengingat)
- [ ] Feature test: siklus tagihan, grace period, verifikasi manual

#### Minggu 3 — Payment Gateway

- [ ] Pilih & daftar payment gateway (kandidat: Midtrans / Xendit / Duitku) — bandingkan biaya MDR & payout
- [ ] Integrasi pembayaran tagihan langganan: QRIS dinamis, VA bank, e-wallet
- [ ] Webhook verifikasi pembayaran → status tagihan otomatis PAID
- [ ] Halaman checkout sederhana untuk tagihan langganan
- [ ] Idempotency & validasi signature webhook (keamanan)
- [ ] Feature test dengan mode sandbox gateway

#### Minggu 4 — Provisioning, Domain & Subdomain Automation

- [ ] Provisioning tenant otomatis: daftar → pilih paket → bayar → tenant aktif + seeder konten awal
- [ ] Subdomain otomatis: buat record DNS/wildcard `*.umkmku.id` per tenant baru (tanpa setup manual)
- [ ] Alur pilih subdomain saat onboarding + validasi ketersediaan
- [ ] Domain sendiri: input domain → instruksi DNS (A/CNAME) → verifikasi kepemilikan → SSL otomatis (mis. Caddy/Certbot)
- [ ] Middleware resolusi tenant dari domain kustom
- [ ] Feature test: provisioning end-to-end dari pendaftaran sampai website online

#### Minggu 5 — Onboarding Otomatis & Admin Master

- [ ] Alur onboarding mandiri: DAFTAR → pilih jenis usaha → pilih template → isi data → pilih domain/subdomain → bayar → aktif (sesuai bagian Onboarding Tenant)
- [ ] Template data awal per vertical (menu contoh, layanan contoh) agar website tidak kosong saat aktif
- [ ] Email/WA selamat datang + panduan singkat untuk owner baru
- [ ] Panel super admin: monitoring langganan (aktif/expired/trial), MRR sederhana, daftar tagihan
- [ ] Audit log untuk aktivitas billing & provisioning
- [ ] Review Phase 7, siapkan daftar vertical berikutnya untuk Phase 8

#### Minggu 6 (Buffer, opsional)

- [ ] Uji bebas-regensi: siklus penuh trial → bayar → expired → perpanjang, dua kali
- [ ] Hardening webhook (retry, logging, alerting gagal)
- [ ] Opt-in pengingat perpanjangan via WA (terjadwal)
- [ ] Dokumentasi operasional: cara verifikasi bayar, handle refund, handle komplain tenant
- [ ] Simulasi 10 tenant baru menyala bersamaan (provisioning tidak tabrakan)

#### Kriteria Selesai Phase 7 (Definisi Selesai)

- [ ] Tenant baru dapat mendaftar, memilih paket, membayar, dan langsung diproses tanpa campur tangan manual
- [ ] Tagihan langganan terbit otomatis dan dapat dibayar via payment gateway
- [ ] Subdomain baru aktif otomatis; domain kustom terverifikasi & SSL jalan
- [ ] Tenant expired terblokir panel sesuai kebijakan (paywall/grace period)
- [ ] Super admin memantau MRR & status semua langganan dari satu panel
- [ ] Siklus penuh trial → bayar → expired → perpanjang teruji tanpa bug

## PHASE 8 — ADDITIONAL VERTICAL

Estimasi: 2–4 minggu per vertical · PJ: Developer

```text
Laundry
↓
Toko
↓
Fashion
↓
Jasa
↓
Travel
```

---

# 36. MODEL PRODUK & POSITIONING

Produk akhir:

```text
                    DIGITALISASI UMKM
                           │
              ┌────────────┴────────────┐
              │                         │
          WEBSITE                 BUSINESS SYSTEM
              │                         │
        Profil & Katalog        Transaksi & Laporan
              │                         │
              └────────────┬────────────┘
                           │
                     CORE PLATFORM
                           │
        ┌──────────┬───────┼───────┬──────────┐
        │          │       │       │          │
     Kuliner   Percetakan Bengkel Laundry    Toko
```

## Positioning

Jangan menjual:

> "Jasa membuat website."

Jual:

> **"Sistem Digitalisasi UMKM Siap Pakai."**

Value proposition:

> **Dari pencatatan manual menjadi sistem digital dalam satu platform.**

---

# 37. STRATEGI PENGEMBANGAN

## Jangan membangun semuanya sekaligus.

Tahap awal:

```text
CORE ENGINE
      ↓
PERCETAKAN
      ↓
KULINER
      ↓
BENGKEL
```

Setelah core stabil:

```text
LAUNDRY
TOKO
FASHION
JASA
TRAVEL
```

---

# 38. TARGET JANGKA PANJANG

Platform diharapkan berkembang dari:

```text
JASA PEMBUATAN WEBSITE
```

menjadi:

```text
PLATFORM DIGITALISASI UMKM
```

kemudian:

```text
SAAS UMKM
```

dan akhirnya:

```text
ECOSYSTEM UMKM
```

Dengan sumber pendapatan:

```text
Setup
Subscription
Domain
Hosting
Maintenance
Custom Module
Storage
User tambahan
Integrasi
Payment
Support
```

---

# 39. PRINSIP UTAMA PRODUK

### SIMPLE

UMKM tidak perlu memahami teknologi.

### MODULAR

Setiap usaha mendapatkan modul sesuai kebutuhan.

### SCALABLE

Satu core engine dapat melayani banyak tenant.

### SECURE

Data setiap usaha harus terisolasi.

### RESPONSIVE

Prioritas penggunaan melalui smartphone.

### REUSABLE

Template dan modul dapat digunakan berulang kali.

### RECURRING

Model bisnis diarahkan pada pendapatan berulang.

---

# 40. KESIMPULAN

Produk yang dibangun bukan:

> **Website murah untuk UMKM.**

Tetapi:

> # SISTEM DIGITALISASI UMKM SIAP PAKAI

Dengan konsep:

```text
1 CORE ENGINE
        +
MULTI TENANT
        +
MULTI VERTICAL
        +
WEBSITE
        +
BUSINESS SYSTEM
        +
SUBSCRIPTION
        =
SAAS UMKM
```

Target akhir:

> **Satu platform yang dapat digunakan oleh berbagai jenis UMKM tanpa harus membuat aplikasi baru dari awal untuk setiap pelanggan.**
