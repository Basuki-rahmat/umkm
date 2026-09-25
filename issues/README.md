# Issues — Sistem Digitalisasi UMKM

Pecahan plan.md menjadi issue kecil yang self-contained, dirancang agar bisa dikerjakan oleh **junior programmer** atau **model AI murah/free-quota** dalam satu sesi pendek per issue.

## Prinsip

1. **1 issue = 1 deliverable kecil** (3–6 task, ±2–3 hari kerja, atau 1 sesi AI).
2. **Self-contained**: semua konteks ada di dalam file issue. Eksekutor TIDAK perlu membaca plan.md.
3. **Urutan eksekusi ketat**: jalankan sesuai nomor ID. Issue tidak boleh dikerjakan sebelum dependency-nya selesai (cek bagian "Depends on" tiap issue).
4. **Definisi selesai terukur**: setiap issue punya Acceptance Criteria yang bisa diverifikasi dengan perintah konkret (test, command, atau langkah manual).

## Struktur Folder

```text
issues/
├── README.md                      ← file ini
├── CONVENTIONS.md                 ← aturan kode & proyek yang wajib dibaca eksekutor SEBELUM issue pertama
├── phase-1-foundation/            ← ISS-001 … ISS-017 (17 issue)
├── phase-2-core-business/         ← ISS-018 … ISS-045 (28 issue)
├── phase-3-website-builder/       ← ISS-046 … ISS-059 (14 issue)
├── phase-4-vertical-percetakan/   ← ISS-060 … ISS-070 (11 issue)
├── phase-5-vertical-kuliner/      ← ISS-071 … ISS-080 (10 issue)
├── phase-6-vertical-bengkel/      ← ISS-081 … ISS-091 (11 issue)
└── phase-7-saas/                  ← ISS-092 … ISS-106 (15 issue)
```

## Alur Kerja (AI Native Engineering)

Untuk setiap issue:

```text
1. Pilih issue berikutnya yang dependency-nya sudah selesai
2. Prompt ke model AI:
   "Baca issues/CONVENTIONS.md lalu kerjakan issues/phase-1-foundation/ISS-0XX-*.md.
    Kerjakan HANYA issue itu. Selesaikan semua Acceptance Criteria."
3. Review hasil: jalankan semua command di bagian Acceptance Criteria
4. Commit per issue (1 issue = 1 commit, format pesan: "ISS-0XX: <ringkasan>")
5. Centang checklist task di file issue, lanjut issue berikutnya
```

### Tips Khusus Model Free-Quota Harian

* **1 sesi = 1 issue.** Jangan gabungkan dua issue dalam satu sesi — kualitas output menurun drastis saat konteks membengkak.
* Mulai prompt dengan dua file saja: `CONVENTIONS.md` + issue yang dikerjakan. Jangan berikan plan.md.
* Jika model kehabisan kuota di tengah issue: commit apa yang sudah jadi, lanjutkan sisa task di sesi baru dengan prompt yang sama + "lanjutkan task yang belum selesai".
* Issue dengan tanda `[AI-friendly]` aman 100% untuk AI murah (pola berulang, contoh sudah lengkap). Tanda `[AI+riviu]` berarti butuh manusia review hasilnya sebelum commit (biasanya keputusan desain/UX).

## Template Prompt Siap Pakai

```text
Kamu adalah developer pada proyek Laravel "Sistem Digitalisasi UMKM".
1. Baca issues/CONVENTIONS.md — patuhi semua aturannya.
2. Kerjakan issues/phase-1-foundation/ISS-0XX-<nama>.md — HANYA issue ini.
3. Kerjakan task berurutan di bagian "Tasks". Jangan menambah fitur di luar issue.
4. Selesai = semua kotak di "Acceptance Criteria" lolos. Jalankan command-nya untuk membuktikan.
5. Jangan ubah file lain selain yang disebut issue. Jangan refactor kode lain.
6. Laporkan: task selesai, command yang dijalankan + output ringkas, dan kendala jika ada.
```

## Indeks GitHub

Semua 106 issue sudah di-mirror ke GitHub dan **berurutan identik dengan nomor ID**: `ISS-0XX` ↔ issue `#0XX` (ISS-001 → #1 … ISS-106 → #106; diverifikasi total 106, min #1, max #106).

| Fase | File lokal | GitHub issue |
| --- | --- | --- |
| Phase 1 — Foundation | `phase-1-foundation/ISS-001-*.md` … `ISS-017-*.md` | https://github.com/Basuki-rahmat/umkm/issues/1 … `/17` |
| Phase 2 — Core Business | `phase-2-core-business/ISS-018-*.md` … `ISS-045-*.md` | https://github.com/Basuki-rahmat/umkm/issues/18 … `/45` |
| Phase 3 — Website Builder | `phase-3-website-builder/ISS-046-*.md` … `ISS-059-*.md` | https://github.com/Basuki-rahmat/umkm/issues/46 … `/59` |
| Phase 4 — Vertical Percetakan | `phase-4-vertical-percetakan/ISS-060-*.md` … `ISS-070-*.md` | https://github.com/Basuki-rahmat/umkm/issues/60 … `/70` |
| Phase 5 — Vertical Kuliner | `phase-5-vertical-kuliner/ISS-071-*.md` … `ISS-080-*.md` | https://github.com/Basuki-rahmat/umkm/issues/71 … `/80` |
| Phase 6 — Vertical Bengkel | `phase-6-vertical-bengkel/ISS-081-*.md` … `ISS-091-*.md` | https://github.com/Basuki-rahmat/umkm/issues/81 … `/91` |
| Phase 7 — SaaS | `phase-7-saas/ISS-092-*.md` … `ISS-106-*.md` | https://github.com/Basuki-rahmat/umkm/issues/92 … `/106` |

**URL langsung per issue**: `https://github.com/Basuki-rahmat/umkm/issues/<nomor>` dengan nomor = ID pada nama file (`ISS-042` → `/42`). Label GitHub mengikuti fase (`phase-1-foundation`, dst.); body = isi file issue (tasks + acceptance criteria).

## Status

Update bagian ini setiap selesai satu issue:

```text
Phase 1: 0/17 selesai
Phase 2: 0/28 selesai
Phase 3: 0/14 selesai
Phase 4: 0/11 selesai
Phase 5: 0/10 selesai
Phase 6: 0/11 selesai
Phase 7: 0/15 selesai
```

**Total: 106 issue** (roadmap Phase 1–7 lengkap). Phase 8 (vertical tambahan) dibuat dari backlog setelah MVP berjalan.
