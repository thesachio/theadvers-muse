---
name: flow-a-intel
description: Tahap A — INTEL proyek baru. Susun brief 9 butir (nama, lingkup, stage, repo, website, docs, audit, bugbounty, severity), jalankan tangga penemuan A4 (maks 3 cek per butir, status DITEMUKAN / TIDAK ADA / TIDAK KETEMU), kunci pin commit + blok referensi (A5). Dipakai saat memulai proyek audit baru atau saat user menyebut INTEL, brief proyek, penemuan proyek.
---

# Skill flow-a-intel — Tahap A (INTEL)

Tahap A = brief proyek, bukan dokumen teknis. Sumber kebenaran: `methodology/aturan-alur-first.md` (A1–A5). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Keluaran

- `A01-INTEL.md` — brief 9 butir (A1) + status tiap butir + tabel Catatan Ketiadaan bila PARTIAL.
- Pin: commit + blok referensi + stage (A5).

## Langkah

### 1. INTEL = brief 9 butir (A1)

1. Nama proyek (singkat).
2. Deskripsi lingkup proyek (bridge/swap/LP dll, singkat).
3. Deployment stage (MAINNET / TESTNET / DEVNET).
4. Repo (alamat github atau setara).
5. Website (tempat user akses dan interaksi UI aktif).
6. Docs (link).
7. Audit (singkat: siapa + link).
8. Bugbounty (URL).
9. Brief severity (singkat bila ada).

INTEL = brief, bukan dokumen teknis. Pre-0 (A2) = daftar docs pendukung saja, BUKAN verifikasi teknis.

### 2. Tangga penemuan A4

Mulai hanya dari nama proyek (+ tautan yang user beri). Tiap butir dicari lewat tangga sumber berurutan, **maks 3 cek per butir**; tangga habis tanpa hasil → **TIDAK KETEMU** (bukan bukti tidak ada). Tiga status:

- **DITEMUKAN** (dengan tautan).
- **TIDAK ADA** (sumber resmi menyatakan tidak ada).
- **TIDAK KETEMU** (wajib catat apa yang sudah dicoba).

Dua butir penahan beban: **stage (3) dan repo (4)** — bila salah satunya TIDAK KETEMU, Tahap A berhenti dan eskalasi ke Kim (tidak ditebak). Anggaran total penemuan **maks 30 menit**; bila habis, berhenti dengan hasil yang ada.

**INTEL COMPLETE** hanya bila semua butir DITEMUKAN (atau TIDAK ADA dengan sumber); bila ada TIDAK KETEMU → **PARTIAL + tabel Catatan Ketiadaan**.

### 3. Stage = filter pertama (A3)

Stage (MAINNET/TESTNET/DEVNET) menentukan Mode Uji di Tahap E (FORK LIVE_MATCH). HEAD bisa lebih lama atau lebih baru dari live — bukan patokan kebenaran.

### 4. Kunci pin (A5)

INTEL dikunci dengan pin (commit + blok referensi + stage); kanonik read-only selama audit. Bila Tahap D menemukan INTEL salah, koreksi dicatat eksplisit dan terbuka, tidak diam-diam.

Diputus 22:33 WIB 16 Nov 2026: (a) commit yang dipin di run dari-0 = **HEAD saat penemuan**; (b) blok referensi ditentukan di **Tahap B** (sensus live).

## Yang tidak dilakukan di Tahap A

- Cek onchain (itu Tahap B, B7/B8).
- Verifikasi teknis atas docs (pre-0 hanya daftar).
- Menebak butir penahan beban (stage/repo) bila tidak ketemu.
