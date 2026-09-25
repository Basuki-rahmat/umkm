# ISS-050 — Template Engine: 4 Tema Vertical + Identitas Visual

- **Tag**: `[AI+riviu]` — arah visual tema perlu review manusia
- **Depends on**: ISS-049
- **Perkiraan**: 1–1.5 hari
- **Area**: `resources/views/public/themes/`, `config/`, `resources/views/admin/settings/`, `tests/`

## Konteks

Sesuai plan (#10, #27): setiap vertical punya wajah berbeda. Implementasi praktis: 1 layout dasar + 4 tema (CSS variables + variasi section) dipilih via `websites.theme`, dan `template_type` menentukan daftar section yang tersedia.

## Tasks

- [ ] 4 tema: `default` (netral), `kuliner` (hangat, appetizing, section menu menonjol), `percetakan` (tegas, portfolio menonjol), `bengkel` (teknis, booking menonjol) — beda palet + tata letak hero + aksen komponen via CSS variables, tanpa duplikasi layout penuh.
- [ ] Pemetaan `template_type` → section tersedia (config `website-sections.php`): kuliner [menu, promo, galeri], percetakan [portfolio, harga, cara-order], bengkel [layanan, booking, riwayat-servis placeholder] + umum [keunggulan, testimoni, artikel, lokasi].
- [ ] **Aturan render section (global)**: config bersifat deklaratif — section yang aktif tapi belum ada implementasi/data pada tenant (Phase 3: `promo`, `harga`, `cara-order`, `booking`, `riwayat-servis`) merender **tanpa wrapper/konten** (bukan error, bukan placeholder kosong yang tampil). Test wajib menutup kasus ini.
- [ ] Panel: pemilih tema dengan preview thumbnail kecil (4 kartu, hanya tema yang cocok dengan template_type yang aktif), pilihan default bila belum memilih.
- [ ] Logo: pastikan dipakai di header + favicon + PDF (kop) konsisten; bila tenant belum upload logo → placeholder inisial nama usaha.
- [ ] Test: kuliner punya section "menu" tersedia, percetakan tidak; tema tukar warna CSS var; section di luar template_type ditolak dari request admin; section aktif tanpa implementasi/data tidak merender wrapper kosong.

## Acceptance Criteria

- [ ] `php artisan test --filter=WebsiteTheme` lulus.
- [ ] Manual: ganti tenant demo ke 3 template_type berbeda → tampilan & section berbeda jelas.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan buat template full berbeda per vertical (copy layout) — pakai CSS var + section config.
- Jangan hardcode section di view.
