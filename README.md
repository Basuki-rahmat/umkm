# CONVENTIONS — Aturan Wajib Proyek

> Eksekutor (junior dev atau model AI) WAJIB membaca file ini sebelum mengerjakan issue apa pun. Jika aturan di sini bertentangan dengan kebiasaan model, **aturan di sini yang menang**.

## 1. Stack (Tidak Boleh Diganti)

| Komponen | Pilihan |
| --- | --- |
| PHP | 8.3+ |
| Framework | Laravel 13 |
| Database | MySQL / MariaDB |
| Frontend panel | Blade + Tailwind CSS (+ Alpine.js bila perlu interaktivitas ringan) |
| Admin panel | Filament (opsional; jika dipakai, konsisten dari issue pertama) |
| Paket izin | spatie/laravel-permission |
| PDF | DomPDF |
| QA statis | laravel/pint + larastan (wajib lolos) |
| Test | PHPUnit atau Pest (pilih satu di ISS-002, jangan campur) |

## 2. Aturan Multi-Tenant (PALING KRITIS)

1. **Setiap tabel milik tenant WAJIB punya kolom `tenant_id`** (foreign key ke `tenants.id`).
2. **Setiap model milik tenant WAJIB memakai trait `BelongsToTenant`** (dibuat di ISS-003) — jangan pernah mengandalkan filter manual di controller.
3. **DILARANG** membuat query ke tabel tenant tanpa lewat model yang memakai trait tersebut.
4. Route/laporan lintas tenant hanya boleh ada di panel SUPER ADMIN (guard terpisah).
5. Setiap fitur baru WAJIB disertai test isolasi: data tenant A tidak boleh terlihat oleh user tenant B (harus 403/404).

```php
// BENAR — trait menangani scope & pengisian otomatis
class Product extends Model { use BelongsToTenant; }

// SALAH — query manual tanpa trait
Product::where('tenant_id', auth()->user()->tenant_id)->get();
```

## 3. Konvensi Kode

* Bahasa kode: **Inggris** (nama class, method, variabel, komentar). Bahasa UI: **Indonesia**.
* Format otomatis: `vendor/bin/pint` — wajib dijalankan sebelum selesai.
* Statis: `vendor/bin/phpstan` (larastan level yang diset di ISS-002) — wajib 0 error.
* Controller tipis: logika bisnis di class Action/Service (`app/Actions`, `app/Services`).
* Validasi lewat Form Request class, bukan `$request->validate()` panjang di controller.
* Nama tabel jamak (plural), model tunggal. Migration berawalan timestamp bawaan artisan.
* Semua kolom uang: `decimal(16,2)`, bukan float.
* Soft delete (`softDeletes()`) untuk master data (customer, product, dll).

## 4. Struktur Storage per Tenant

```text
storage/app/public/tenant-{id}/   ← semua file tenant di sini
    logo/
    products/
    designs/
```

Validasi upload wajib: batasi mime type + ukuran (maks 5 MB kecuali disebut lain).

## 5. Git & Commit

* 1 issue = 1 commit. Format pesan: `ISS-0XX: ringkasan singkat`.
* Branch: `main` untuk kode stabil; setiap issue dikerjakan di `feature/ISS-0XX` lalu di-merge (squash) setelah Acceptance Criteria lolos.
* Jangan commit file `.env`, `node_modules`, atau isi `storage/app/public/tenant-*`.
* Jangan pernah mengubah file di luar cakupan issue, termasuk refactor "sekalian".

## 6. Definisi Selesai (berlaku untuk SEMUA issue)

1. Semua task di bagian "Tasks" selesai.
2. Semua Acceptance Criteria lolos — jalankan command-nya, jangan menebak.
3. `vendor/bin/pint --dirty` dan `vendor/bin/phpstan` lolos tanpa error.
4. Test lulus: `php artisan test`.
5. Tidak ada file yang berubah di luar cakupan issue (`git status` bersih dari perubahan tak terkait).

## 7. Jika Terblokir

* Jangan mengarang solusi di luar stack. Catat kendala di laporan akhir (bagian "Kendala").
* Jika requirement issue ambigu: pilih interpretasi paling sederhana yang memenuhi Acceptance Criteria, catat asumsinya di laporan.
