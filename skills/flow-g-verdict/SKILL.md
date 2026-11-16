---
name: flow-g-verdict
description: Tahap G — Verdict. Verdict 06/07/08/09 per kandidat, tiga verdict terpisah, severity dari plafon teramati, distribusi akhir. Dipakai saat mengadili hasil, atau saat user menyebut tahap G, verdict.
---

# Skill flow-g-verdict — Tahap G (Verdict)

Tahap G = mengadili tiap kandidat dan menghitung distribusi akhir di seluruh protokol. Bentuk rinci = yang sudah dipraktikkan (diputus 22:33 WIB 16 Nov 2026). Sumber kebenaran: `methodology/aturan-alur-first.md` (seksi D — Tahap G + C1–C9) + `verify/VERDICT.md` + `verify/SEVERITY-RUBRIC.md`. Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Langkah

### 1. Verdict per kandidat: 06 / 07 / 08 / 09

- **09 — temuan terkonfirmasi:** mekanisme TERBUKTI + P&L live > 0 / kerugian live terukur.
- **08 — valid tapi tidak memenuhi syarat:** mekanisme terbukti, tapi out-of-scope / known / peran terpercaya / debu.
- **07 — butuh langkah lanjut:** logika belum terbantahkan, tapi butuh verifikasi lanjutan (state on-chain spesifik, komponen eksternal).
- **06 — terbantahkan / tidak valid:** mekanisme patah saat pembuktian.

### 2. Aturan 07 vs 08 (mengikat)

07 TIDAK dilarang — bila ragu antara 07 dan 08, lead tetap di 07 dan wajib punya PERCOBAAN. Yang dilarang keras: (a) **pelarian wadah** — memindahkan lead yang mekanismenya belum terbukti ke 08 lalu melabelinya Low; (b) **parkir di 07 sebagai jalan malas tanpa usaha** (LAZY_BAILOUT_DETECTED, laporan ditolak).

### 3. Aturan 06 (mengikat)

Verdict 06 melarang penolakan kertas — tiap verdict 06 wajib melampirkan bukti eksekusi live yang benar-benar dijalankan.

### 4. Tiga verdict terpisah + severity

Tiap finding: verdict mekanisme / verdict eligibility bounty / verdict dampak — terpisah. Severity dari **plafon kerugian teramati**, bukan dari kapabilitas mekanisme. Klaim insidensi wajib analisis same-transaction. Plafon per-eksekusi tidak dikalikan tanpa bukti pengulangan.

### 5. Distribusi akhir + HOLD

Hitung distribusi akhir (06/07/08/09) di seluruh protokol. **HOLD (mengikat): NOL submisi/kontak program tanpa persetujuan eksplisit per kandidat.** Tahap F/G berhenti di draf siap-kirim; pengiriman butuh persetujuan eksplisit per pengiriman.

## Keluaran

- `G01-VERDICT.md` — verdict per kandidat + distribusi akhir.
- Draf temuan 09 (format `templates/` — laporan teknis + email, identitas pengirim belum diisi sampai Kim memerintah).
