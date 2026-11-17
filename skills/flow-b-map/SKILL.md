---
name: flow-b-map
description: Tahap B — Mapping. Sensus statis 100% peristiwa pemindah nilai (B1/B6, tanpa AST), sensus live ringan B7 wajib, cek live-match B8 + wadah TUNDA-LIVE, ledger sisa (B10), handoff dari batas unit (B11), scope lock (B13). Dipakai saat memetakan scope proyek, sensus, atau saat user menyebut tahap B, mapping, sensus, scope.
---

# Skill flow-b-map — Tahap B (Mapping)

Tahap B = tahu scope menyeluruh di awal (sensus utuh + peta alur), tanpa buruan. Sumber kebenaran: `methodology/aturan-alur-first.md` (B1–B16) + `methodology/acuan-workflow-tahap-B.md` (acuan kerja Tahap B — paling lengkap). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Keluaran (lapis depan, sekali untuk seluruh protokol)

- `B01-SCOPE.md` — daftar scope + scope lock (B13).
- `B02-CENSUS-STATIC.md` + staging JSON — sensus statis 100%.
- `B03-CENSUS-LIVE.md` — inventaris alamat live (daftar) + sensus live ringan B7.
- `B04-CLUSTERS.md` — identifikasi klaster + daftar handoff (B11).
- Ledger sisa (B10) — seed.
- Registry invariant (B3) — struktur + penomoran (kosong; diisi Tahap D).

## Langkah

### 1. Sensus statis 100% (B1 + B6)

Sensus = 100% peristiwa pemindah nilai: entrypoint + transfer langsung/donasi/force-feed + pergerakan level-sistem + multicall/dispatch dinamis transitif + proxy→implementation + callback/hook + transfer SOL/SPL ke PDA + langkah off-chain dilabeli asumsi. Fungsi = unit sensus; segmen = unit buruan.

**B6: baca manual penuh semua fungsi/semua berkas + verifikasi parent + kontrol positif. TIDAK pakai AST** (ditolak Kim). Baca bertingkat DITOLAK untuk sensus statis. Konvensi sensus: B14 (7 konvensi universal — getter publik = entrypoint ya; receive badan kosong = value_moves no; dst.).

### 2. Sensus live ringan B7 — WAJIB (B15)

Rantai utama proyek: getCode 1–3 alamat inti di blok referensi + satu view murah. **Anggaran maks 5 menit.** TANPA jejak event/hitung log/verifikasi inventaris penuh. Rantai tidak merespons (timeout/403/429) → langsung pindah, jangan tunggu. Sensus live PENUH tetap opsional; cek onchain mendalam wajib hanya di B8.

### 3. Cek live-match B8 + TUNDA-LIVE

B8 diulang di awal fase uji per klaster sebelum PoC. Tidak live-match → wadah **TUNDA-LIVE**: tercatat di ledger sisa, nol waktu eksekusi, dibuka lagi bila deployment live muncul atau Kim memerintah cek ulang, ditinjau di sapuan akhir.

**Cara kerja B8 (diputus 17 Nov 2026 08:26 + 08:33 WIB, mengikat):**
- **Utamakan alur, log untuk verifikator terakhir:** tahu suatu mekanisme work atau tidak dari tes tool (baca kode + cast/cek live + uji fork di Tahap E), BUKAN dari pindai log tx orang lain di blockchain. Pindai log = pilihan terakhir yang mahal, hanya untuk verifikasi akhir (Tahap G / sapuan akhir). Preseden: B8 P4 memakan ~8,5 jam pindai RelayedRootBundle per rantai — jangan ulangi.
- **Cek desain kode dulu:** untuk tahu suatu nilai 0 atau bukan, cek dari desain kode apakah DISET 0 oleh kode atau MEMANG BISA MEMUAT — jangan simpulkan dari log. Contoh: nilai yang di-set dari parameter input tanpa require/validasi = BISA MEMUAT non-zero by design; live 0 berasal dari off-chain, bukan batasan kode.

### 4. Penutupan graf + sapuan akhir (B9)

Scope mapping = usaha yang cukup + 4 mekanisme deteksi miss di eksplorasi: (1) baca ulang mendalam per klaster; (2) cek handoff + konservasi; (3) jejak event live sebagai enumerasi independen; (4) cek penutupan graf. **Enumerasi independen kedua = rerun sensus manual** (diputus 22:33 WIB 16 Nov 2026; pilot: rerun menemukan 15 baris yang sensus pertama lewatkan). Bukan AST, bukan regex yang sama. Cara parser independen tetap terbuka sebagai riset.

### 5. Ledger sisa + handoff + scope lock

- **B10:** ledger sisa sesudah tiap round; round selesai hanya bila sisa menyusut; target buruan harus ada di sensus.
- **B11:** handoff dienumerasi dari batas unit (N unit → N−1 handoff); simpul fork/join jadi unit.
- **B13:** scope lock (daftar berkas cocok persis inventaris kanonik/diff kosong, commit terverifikasi, contracts/ bersih).
- **B12:** Tahap B TIDAK membuat invariant — hanya yang riil/teramati (sensus + klaster + daftar handoff). Invariant dirumuskan di Tahap D.

## Yang TIDAK dilakukan di Tahap B (11 larangan)

1. Inventaris mendalam semua klaster sekaligus. 2. Menulis invariant. 3. Buruan saat memetakan. 4. Eksekusi/verifikasi saat memetakan. 5. Sensus live penuh. 6. Baca mendalam semua fungsi saat sensus. 7. Menyelesaikan semua unknown saat sensus. 8. Menilai klaster saat memetakan. 9. Rerun sensus penuh sebagai syarat tetap. 10. Memoles artefak/laporan akhir. **11. Filter di DALAM klaster saat inventory** (hanya data yang dibutuhkan alur live) — dinilai BERISIKO oleh Kim, tidak diadopsi; ditulis ke berkas kanonik 22:33 WIB 16 Nov 2026. Filter yang aman hanya utuh per klaster/alur.

## Catatan B16

**B16 TIDAK DIPAKAI** (diputus Kim 22:33 WIB 16 Nov 2026 — 'B16 tidak perlu'). Field 'keluar' tiap fungsi EP+VM TIDAK wajib; daftar jahitan SM-xx lengkap TIDAK wajib. Urutan klaster default tetap EV live (B2). Daftar jahitan pilot Across tetap artefak eksekusi, bukan kewajiban metode.
