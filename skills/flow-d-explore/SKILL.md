---
name: flow-d-explore
description: Tahap D — Eksplorasi per-alur, dua gelombang (buta → berpengetahuan + katalog 52), kartu lead + invariant. Dipakai saat memburu alur, atau saat user menyebut tahap D, eksplorasi, gelombang, kartu lead.
---

# Skill flow-d-explore — Tahap D (Eksplorasi)

Tahap D = buruan per alur. Unit round = **per-alur**, dua gelombang (diputus 22:33 WIB 16 Nov 2026). Sumber kebenaran: `methodology/aturan-alur-first.md` (seksi D — Tahap D + B12). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Prasyarat

Daftar alur (enumerasi + EV per alur, dari B02+B04) sudah ada sebelum D. Tanpa daftar alur, D tidak mulai.

## Langkah

### 1. Gelombang 1 — buta

- Bekal: L-VALUE + L-PATH + inventaris alur klaster (C01) + premis parent.
- **DILARANG:** membuka katalog (`_ROUTER.md` atau plugin apa pun), membaca badan fungsi klaster lain, memakai laporan audit lama.
- Pengecualian wajib: baca M-005 §3 sebelum entri 04.
- Keluaran: observasi (OBS) + jalur terjaga (G) + sinyal RADAR (R) + pertanyaan gelombang 2 (Q).

### 2. Gelombang 2 — berpengetahuan + katalog

- Silang: inversi handoff + konservasi + jejak event live + katalog 52 plugin (baca hanya plugin yang gejalanya cocok; `[NO-CATALOG]` bila tidak ada yang cocok — kandidat DILARANG gugur hanya karena tidak cocok katalog).
- Temuan di luar katalog = kandidat plugin baru (niat C1: jangan batasi eksplorasi).
- Keluaran: kandidat QUEUED (QD..) dengan prioritas P1–P5 + DORMAN + disposisi Q/RADAR.

### 3. Kartu lead + invariant

- Kartu lead 1:1 dari kandidat QUEUED; **tiap kartu wajib klaim finansial sendiri**.
- Invariant dirumuskan bersama kartu leadnya. Tiap invariant **WAJIB mencantumkan sumbernya**: field kontrak handoff, atau baris docs/NatSpec. Tiga lapis: L1 dalam segmen (INV-Sx), L2 handoff + simpul (INV-Hx, INV-Jx), L3 ujung-ke-ujung/komposisi (INV-Fx). Tiap invariant wajib akibat finansial bila pecah.
- Registry invariant satu penomoran global (B3), diisi mulai Tahap D.
- **Gate finansial:** lead masuk Tahap E HANYA bila punya plafon live terukur; plafon live 0 → DORMAN (dengan syarat buka-ulang konkret).

## Aturan yang mengikat

- **Bahasa brief:** bahasa verifikasi invariant, bukan bahasa serangan. Brief Tahap E memakai bahasa verifikasi.
- **Tugas agen dibatasi ALUR, bukan klaster:** boleh baca lintas klaster hanya sepanjang alur yang dilewati.
- **Definisi alur:** satu alur = KELUARGA jalur yang berbagi segmen + kontrak handoff yang sama. Bila inti beda, pecah jadi kartu lead sendiri atau round sendiri.
- **Deep-run per jalur** (diputus 22:33 WIB 16 Nov 2026): satu perintah berdiri hulu→hilir sampai verdict per lead, tanpa tap antar langkah; berlaku mulai jalur berikut (P4 — P3 sudah jalan step-by-step).

## Keluaran

- `Dnn-<JALUR>-WAVE1-BLIND.md`, `Dnn-<JALUR>-WAVE2-QUEUED.md`, `Dnn-<JALUR>-LEAD-CARDS.md` + registry invariant terisi.
