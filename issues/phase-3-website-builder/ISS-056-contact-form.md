# ISS-056 — Form Kontak/Penawaran (Publik → Panel)

- **Tag**: `[AI-friendly]`
- **Depends on**: ISS-055
- **Perkiraan**: 0.5–1 hari
- **Area**: `database/migrations/`, `app/Http/Controllers/Public/`, `resources/views/admin/messages/`, `tests/`

## Konteks

Pengunjung website mengirim pesan/pertanyaan penawaran → masuk panel owner → difollow-up via WA. Ini jalur leads utama sebelum ada order online.

## Tasks

- [ ] Migration `create_contact_messages_table`: `id`, `tenant_id`, `name`, `phone`, `email` nullable, `subject` nullable, `message` text, `is_read` bool default false, `created_at` (tanpa updated_at).
- [ ] Halaman kontak publik: form (nama, telepon, pesan wajib; email/subject opsional) + honeypot field anti-bot sederhana + rate limit 5/menit/IP; sukses → halaman terima kasih + tombol "Chat Langsung via WA".
- [ ] Panel: halaman "Pesan Masuk" (badge unread di sidebar), list (nama, telepon, potongan pesan, waktu, status baca), detail + tandai dibaca, tombol WA ke pengirim (WaLink dengan pesan salam balasan), hapus.
- [ ] Notifikasi opsional ke email owner (config on/off, Mail::fake di test).
- [ ] Test: submit sukses tersimpan & unread; honeypot terisi → ditolak diam-diam; rate limit; WA link nomor benar; isolasi (pesan masuk ke tenant yang benar sesuai host).

## Acceptance Criteria

- [ ] `php artisan test --filter=ContactMessage` lulus.
- [ ] Manual dari HP: kirim pesan → muncul di panel → klik WA berfungsi.
- [ ] `pint` + `phpstan` lolos.

## Jangan

- Jangan kirim SMS/WA otomatis ke owner (fase lanjutan).
- Jangan simpan data lebih dari yang dibutuhkan (privasi).
