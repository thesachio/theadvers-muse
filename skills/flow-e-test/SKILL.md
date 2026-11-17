---
name: flow-e-test
description: Tahap E — Uji test. PoC fork-live + P&L terukur per kandidat, batch maks 3, parent menjalankan ulang forge dan mencocokkan gas. Dipakai saat menguji lead, atau saat user menyebut tahap E, uji, proof, fork.
---

# Skill flow-e-test — Tahap E (Uji test)

Tahap E = membuktikan/membantah mekanisme di fork live + mengukur P&L. Bentuk rinci = yang sudah dipraktikkan (diputus 22:33 WIB 16 Nov 2026). Sumber kebenaran: `methodology/aturan-alur-first.md` (seksi D — Tahap E + C1–C11). Skill ini menuntun eksekusi; aturan ada di berkas itu.

## Langkah

### 1. Fork live (aturan fork-live, mengikat)

Fork dibangun ke kondisi live (kontrak ter-deploy + state live pada blok yang di-pin), kecuali proyek sudah dinyatakan testnet/devnet di awal. PoC HEAD saja tidak cukup bila live ≠ HEAD. Mekanisme penentu wajib diuji ulang di fork live bila fork-testable; bila tidak fork-testable, catat keterbatasan itu secara eksplisit. **Sumber RPC (mengikat, 17 Nov 2026): ALCHEMY utama** — `rpc-env.sh <rantai>` otomatis memilih key alchemy per-rantai (dari `.env`/`.env.rpc.secret`) lebih dulu; **fallback public RPC** (publicnode/dll) hanya bila rantai tidak punya key ATAU alchemy gagal/limit. Free-tier alchemy ETH: `eth_getLogs` rentang dibatasi → chunking wajib.

Satu fork per wilayah dipakai ulang; deploy_parity sekali + cache; satu harness dua mode (HEAD + fork); triase fork-testability di awal (aturan hemat Fase D — `methodology/rancangan-hemat-fase-d.md`).

### 2. Batch maks 3

Tiap batch maks 3 kartu lead. Untuk tiap kartu: PoC fork + kontrol + P&L terukur (modal masuk, uang keluar, mode kegagalan). **Brief memakai bahasa verifikasi invariant, bukan bahasa serangan.**

### 3. Verifikasi parent atas bukti penentu

Parent menjalankan ulang semua tes forge sendiri dan mencocokkan gas (harus IDENTIK). Parent memeriksa jangkar dari kode bernomor (batas fungsi via `grep -n` — satu baris yang ada ≠ jangkar yang benar). Rantai koreksi bukan bukti: tiap langkah berikut wajib memeriksa ulang jangkar yang dikoreksi.

### 4. Verdict invariant + tiga verdict

- Invariant: TERBANTAH / TERBUKTI / TIDAK KONKLUSIF → tulis ke registry (B06).
- Tiga verdict terpisah per finding: mekanisme / eligibility bounty / dampak. Severity dari plafon TERAMATI, bukan kapabilitas. Klaim insidensi wajib analisis same-transaction. Plafon per-eksekusi tidak dikalikan tanpa bukti pengulangan.
- **C10 — definisi kerugian (mengikat, 17 Nov 2026):** kerugian = yang BISA DIBUKTIKAN merugikan — mekanisme jalan di fork/test atas kode+state nyata + plafon terukur + pemicu terjangkau lewat jalur operasi normal, tanpa peran istimewa dan tanpa state eksotis. **TIDAK HARUS LIVE** — P&L live 0 tidak lagi membuat lead DORMAN atau gugur.
- **C11 — standar TERBUKTI (mengikat, 17 Nov 2026):** TERBUKTI = ada cukup jalur pendek yang saling bergantung dari pelanggaran invariant sampai kerugian terukur, diverifikasi lewat alur-alur TERKAIT. Tes yang berhenti di satu fungsi pendek lalu menyimpulkan = masih HIPOTESIS. Kartu E merumuskan rantai (a)+(b)+(c) sebagai SATU rantai yang diuji utuh.

## Disiplin bukti (mengikat)

- Label jelas: **DIEKSEKUSI** vs **DIANALISIS** vs **CODE-REVIEW-ONLY**. Jangan sajikan penalaran sebagai serangan yang dieksekusi.
- **verify_zero_and_scan:** setiap nol = mencurigakan; simpan output lengkap termasuk error; wajib kontrol positif bukan-nol lewat jalur sama; pindai besar dibuktikan sensus atau dilabeli SAMPEL persis.
- Cabut klaim yang gugur sampai ke dokumennya.
- Penolakan safety classifier TIDAK boleh diulang, diparafrase, atau diputar lewat jalur lain.

## Keluaran

- `Enn-<JALUR>-BATCHn-PROOF.md` + `poc/e<nn>-batch<n>/` (tes forge + log).
