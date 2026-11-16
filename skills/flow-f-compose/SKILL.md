---
name: flow-f-compose
description: Tahap F — Komposisi (bersyarat). Hanya lead multi-segmen, hanya bila penting dan urgent; uji pakai data terbaru, tanpa fork/PoC ulang; bukti = lemma. Dipakai saat merangkai jalur ujung-ke-ujung, atau saat user menyebut tahap F, komposisi.
---

# Skill flow-f-compose — Tahap F (Komposisi)

Tahap F = merangkai segmen/handoff/simpul yang sudah terbukti jadi jalur ujung-ke-ujung. **DITERIMA BERSYARAT** (diputus Kim 22:33 WIB 16 Nov 2026: 'terima jika memang penting dan urgent'). Sumber kebenaran: `methodology/aturan-alur-first.md` (seksi D — Tahap F). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Gerbang masuk

F hanya dijalankan bila **penting dan urgent**. Bila tidak, lead langsung ke Tahap G dengan bukti Tahap E-nya. Lead satu segmen **LEWATI** Tahap F (PoC Tahap E sudah final).

## Langkah

### 1. Hanya lead multi-segmen

Rangkai segmen/handoff/simpul yang sudah terbukti (per klaster dulu, lintas-klaster di akhir) jadi jalur serangan ujung-ke-ujung.

### 2. Uji F tanpa fork/PoC ulang (diputus 22:33 WIB 16 Nov 2026)

Uji F **TIDAK menjalankan fork ulang dan TIDAK menjalankan PoC ulang — pakai data terbaru** (hasil Tahap E). Alasannya: bukti segmen sudah dieksekusi di Tahap E; F merangkai bukti itu, bukan mengulang eksekusinya.

### 3. Bukti komposisi = lemma

- Segmen terbukti (Tahap E) + handoff cocok + simpul terbukti + jalur gabungan dirangkai dari data terbaru.
- Bila rangkaiannya menemukan ketidakcocokan (mis. handoff yang tidak menutup, simpul yang macet), catat sebagai temuan komposisi dan kembalikan ke Tahap D/E yang bersangkutan — jangan diam-diam menambal.

## Keluaran

- `Fnn-<JALUR>-COMPOSITION.md` (atau seksi komposisi di verdict) — lemma per jalur + ketidakcocokan yang ditemukan.
