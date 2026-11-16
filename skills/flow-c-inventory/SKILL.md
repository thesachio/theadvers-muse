---
name: flow-c-inventory
description: Tahap C — Pemetaan mendalam per klaster. Inventaris alur + segmentasi + kontrak handoff deskriptif per klaster (C01-Kn), tanpa invariant, tanpa buruan, tanpa PoC. Dipakai saat memetakan klaster, atau saat user menyebut tahap C, pemetaan mendalam, inventaris alur.
---

# Skill flow-c-inventory — Tahap C (Pemetaan mendalam per klaster)

Tahap C membangun yang riil/teramati per klaster: inventaris alur + segmentasi + kontrak handoff DESKRIPTIF. Sumber kebenaran: `methodology/aturan-alur-first.md` (seksi D — Tahap C). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Keluaran

- `C01-Kn-FLOW-INVENTORY.md` per klaster — inventaris alur klaster itu (segmen + handoff + simpul + EV).
- `C08-K8-PRIMITIVES.md` untuk klaster primitif (bukan inventaris alur).

## Langkah

### 1. Satu klaster per round

Kerja per klaster (K SAJA). Urutan default = EV live (B2) — prioritas, bukan pagar. Tarikan just-in-time lintas-klaster dibutuhkan di **Tahap D**; di Tahap C, jahitan adalah batas — jangan baca badan fungsi klaster lain.

### 2. Isi inventaris alur

- Segmen-segmen alur (S1..Sn) + fungsi/berkas tiap segmen.
- Kontrak handoff antar-segmen (produsen, konsumen, field nama+tipe+satuan, jaminan produsen, asumsi konsumen — sebagaimana teramati di kode).
- Simpul fork/join.
- EV klaster (jejak event live bila ada).

### 3. Klaster khusus

- **K8 (primitif):** inventaris PRIMITIF, bukan inventaris alur — jaminan produsen tiap primitif (mis. wrap/unwrap 1:1, peran, cap, event).
- **K9:** DILEWATI — mock test, bukan kode live.

## Yang tidak dilakukan di Tahap C

- Menulis invariant (itu Tahap D — B12).
- Buruan saat memetakan.
- PoC / verifikasi / verdict.
- Menutup jahitan yang konsumennya di luar kode (mis. TokenMessenger eksternal) tanpa keputusan Kim — jahitan seperti itu tetap dienumerasi dan dibuka saat alur yang diburu menyentuhnya (SM-05: tetap dieksplorasi ketika terkait, diputus 22:33 WIB 16 Nov 2026).

## Bukti bentuk (pilot Across)

8 round (K1–K8) selesai ~3–8 menit per round; masuk alur 145/169; SM 11/12 TERTUTUP.
