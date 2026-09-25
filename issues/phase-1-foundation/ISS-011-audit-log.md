# ISS-011 — Audit Log

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-010
- **Perkiraan**: 1 hari
- **Area**: `database/migrations/`, `app/Support/`, `app/Observers/`, `resources/views/platform/audit-logs/`, `tests/`

## Konteks

Sistem mencatat aktivitas penting (LOGIN, CREATE, UPDATE, DELETE, LOGIN FAILED) untuk keamanan. Log dibaca SUPER ADMIN. Format contoh dari plan: `24-09-2026 10:15 · Admin · Update Produk · Produk: Banner 3x2`.

## Tasks

- [ ] Migration `audit_logs` (id, tenant_id nullable (log platform tanpa tenant), user_id nullable, action string (LOGIN/CREATE/UPDATE/DELETE/LOGIN_FAILED/EXPORT/PRINT), subject_type nullable, subject_id nullable, description text, ip_address, user_agent, created_at).
- [ ] Class `App\Support\Audit` dengan static `log(string $action, Model|string $subject = null, string $description = '')` mengambil user & tenant dari context.
- [ ] Observer untuk model `User`, `Tenant`, `Setting` (create/update/delete → tulis audit log; deskripsi berisi field yang berubah secara ringkas).
- [ ] Hook login sukses & gagal → audit log (event listener).
- [ ] Halaman `/platform/audit-logs`: filter tanggal, action, user, tenant; pagination; detail expandable sederhana.
- [ ] Rotasi sederhana: command artisan `audit:prune --days=90` (hapus lebih lama dari N hari).
- [ ] Test: login gagal 3x → 3 log LOGIN_FAILED; update setting → log UPDATE berisi key yang diubah.

## Acceptance Criteria

- [ ] `php artisan test --filter=Audit` lulus.
- [ ] Halaman platform menampilkan log dengan filter berfungsi.
- [ ] `php artisan audit:prune --days=90` berjalan tanpa error.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan catat password/mutation sensitif di description.
- Jangan log request body penuh.
