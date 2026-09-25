# ISS-046 — Routing Website Publik per Tenant

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-045 (Phase 2 selesai)
- **Perkiraan**: 0.5–1 hari
- **Area**: `routes/`, `app/Http/Controllers/Public/`, `bootstrap/app.php`, `tests/`

## Konteks

Website publik per tenant di subdomain (`tokomaju.umkmku.id`) — terpisah total dari panel admin. Middleware `IdentifyTenant` (ISS-004) sudah menyelesaikan tenant dari host; route publik memakai konteks itu.

## Tasks

- [ ] Route group publik (host tenant, bukan central domain): `/` → home, `/tentang`, `/produk`, `/artikel`, `/kontak`, `/galeri` — semua controller di namespace `App\Http\Controllers\Public\` (prefix `Public_` atau folder terpisah), tanpa auth.
- [ ] Fallback: request tenant tanpa website aktif (`websites` tidak ada ATAU `is_published=false` — kondisi disatukan dengan ISS-048) → tampil halaman "segera hadir" sederhana (tetap 200, bukan error).
- [ ] Central domain (`umkmku.id`) tetap melayani landing platform + `/admin` + `/platform` (tidak tertimpa route tenant).
- [ ] Test: dua tenant berbeda host → konten berbeda (assert nama usaha dari settings masing-masing); host tidak dikenal → 404 halaman sopan; route admin tidak bocor di host tenant.

## Acceptance Criteria

- [ ] `php artisan test --filter=PublicRouting` lulus.
- [ ] Manual: akses subdomain demo lokal (`demo.localhost`) → home tenant tampil.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan render konten halaman lengkap (home dll di issue berikutnya) — hanya kerangka routing + halaman placeholder.
- Jangan expose route debug/test ke publik.
