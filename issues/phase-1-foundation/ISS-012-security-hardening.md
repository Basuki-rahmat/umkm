# ISS-012 — Security Hardening (Login Rate Limit, Upload, Headers)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-011
- **Perkiraan**: 1 hari
- **Area**: `app/Http/Controllers/Auth/`, `app/Http/Middleware/`, `bootstrap/app.php`, `tests/`

## Konteks

Penguatan keamanan minimum sesuai plan (#33 Keamanan Wajib, subset fase foundation): rate limiting login, pembatasan upload, security headers, session security. Item HTTPS/CSRF/XSS sudah di-handle Laravel by default — issue ini menambah sisanya.

## Tasks

- [ ] Rate limit login: maks 5 percobaan per menit per email+IP → throttle response dengan pesan Indonesia yang jelas.
- [ ] Validation rule reusable `AllowedUpload` (file gambar/pdf/desain: jpg/png/webp/pdf/ai/psd/cdr) — ukuran maks **default 5MB**, dapat disetel per pemanggil; logo khusus gambar maks 2MB; file desain percetakan khusus 10MB — dipakai di ISS-010, ISS-022, ISS-062, ISS-086, ISS-088.
- [ ] Middleware security headers: `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` minimal.
- [ ] Session: `SESSION_SECURE_COOKIE=true` (prod), `SESSION_SAME_SITE= lax` dipastikan di config; cookie lifetime default.
- [ ] Pastikan password hashing (bcrypt default) — tulis test bahwa password tidak pernah disimpan plain.
- [ ] Test: percobaan login ke-6 dalam 1 menit → 429/423 dengan pesan; upload file `.exe` ditolak; logo 3MB ditolak.

## Acceptance Criteria

- [ ] `php artisan test --filter=Security` lulus.
- [ ] `curl -I` localhost menampilkan security headers.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan implementasi 2FA (di luar MVP).
- Jangan ubah struktur tabel yang sudah ada.
