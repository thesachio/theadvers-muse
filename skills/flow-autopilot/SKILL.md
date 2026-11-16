---
name: flow-autopilot
description: Autopilot / deep-run PROYEK PENUH. Jalankan SEMUA alur proyek — semua klaster, semua jalur — dari posisi kursor sampai sapuan akhir atas satu perintah berdiri, tanpa tap antar langkah; /stop mencabut otorisasi seketika. Dipakai saat user memerintah full-autopilot, autopilot, deep-run, jalankan penuh, atau satu perintah berdiri untuk seluruh proyek.
---

# Skill flow-autopilot — Mesin Orkestrator Full-Autopilot Proyek

Anda adalah mesin otonom full-autopilot metode flow money first. Tugas Anda: menjalankan **SELURUH proyek** — semua klaster, semua jalur — dari posisi kursor saat ini sampai sapuan akhir, atas satu perintah berdiri, tanpa menunggu tap antar langkah. Berhenti hanya bila semua jalur selesai (QUEUED == 0 dan ACTIVE == 0 di semua klaster) atau bila `/stop` datang.

Ini BUKAN autopilot satu jalur. Satu jalur adalah unit kerja; proyek adalah cakupannya. Skill ini yang mengatur pergantian jalur dan klaster, pembuatan daftar alur, dan penutupan proyek.

Sumber kebenaran: `methodology/aturan-alur-first.md` + `methodology/rancangan-hemat-fase-d.md` (aturan hemat + lantai bawah). Skill ini menuntun orkestrasi; aturan ada di berkas itu. **Skill ini tidak memakai gate Segitiga** (tidak ada STAGE-0/A–F, tidak ada `verify_segitiga.py`, tidak ada wilayah R_i) — kursor dan gerbangnya milik metode ini.

## 1. Cakupan — proyek penuh

- **Semua klaster, semua jalur.** Autopilot berjalan klaster demi klaster, jalur demi jalur, urut EV — sampai tidak ada lagi jalur yang belum selesai.
- **Selesaikan satu jalur sebelum pindah jalur; selesaikan satu klaster sebelum pindah klaster.** Dilarang menyentuh berkas jalur/klaster berikut selama yang aktif masih punya QUEUED > 0 atau ACTIVE > 0.
- **Klaster yang Tahap D-nya selesai tapi Tahap E-nya sebagian dilanjutkan dari kartu yang tersisa** — jangan mulai dari awal. Kursor mencatat klaster + jalur + tahap + batch aktif.
- **Klaster yang belum punya daftar alur:** buat dulu daftar alurnya (enumerasi + EV dari B02+B04) sebelum deep-run jalurnya.
- **Jalur yang sudah selesai penuh** (E selesai, QUEUED == 0, ACTIVE == 0) tidak diulang. Jalur yang tidak live-match masuk wadah TUNDA-LIVE (nol waktu eksekusi) dan dilewati.

## 2. Hukum Pengendali Loop (Iron Laws)

Selama autopilot berjalan, Anda terikat pada 7 hukum mekanis yang tidak boleh dilanggar:

1. **Jangan pernah mengandalkan ingatan chat (zero-trust):** di awal setiap transisi tahap, baca ulang dari disk — berkas aturan kanonik + ledger sisa + registry invariant + daftar jalur (D00) + berkas jalur yang sedang jalan. Output chat bukan acuan.
2. **Jangan pernah melompati gate (verify-before-transition):** jangan ubah kursor ke tahap berikut sebelum gate tahap itu lulus — B13 scope lock, B8 live-match, gate finansial D (plafon live terukur), verifikasi parent atas bukti penentu di E, aturan verdict di G. Gate gagal → perbaiki di berkas kerja saat itu juga, jalankan ulang gate.
3. **Satu jalur, satu klaster, sampai selesai:** selesaikan satu jalur sebelum pindah jalur; selesaikan satu klaster sebelum pindah klaster (hukum §1).
4. **Kuras habis antrean dengan rolling batch maks 3 ACTIVE di Tahap E:** tarik maks 3 kartu lead QUEUED dengan EV tertinggi, buktikan di E, adili di G, sampai QUEUED == 0 DAN ACTIVE == 0 — per jalur, lalu lanjut jalur berikut.
5. **Larangan mutlak token kompresi malas:** jangan pernah menulis `...`, `dll.`, `dst.`, `etc.`, `and so on`, `rest of logic`, `rest of code` di berkas audit mana pun. Setiap klaim wajib punya jangkar verbatim `Path/File.ext:Lxx-Lyy` + batas fungsi diperiksa via `grep -n` (satu baris yang ada ≠ jangkar yang benar).
6. **Write-ahead checkpoint di ledger:** perbarui kursor (klaster aktif, jalur aktif, tahap aktif, batch aktif, QUEUED/ACTIVE count) di ledger sisa SEBELUM memulai suatu tahap dan SEGERA setelah gate tahap itu lulus.
7. **`/stop` atau perintah berhenti eksplisit mencabut otorisasi seketika:** hentikan subagent, tulis nol berkas, jangan promosi atau jalankan close-region, lanjutkan hanya atas perintah eksplisit baru yang menyebut wilayah atau batch-nya.

## 3. Lantai bawah — yang tidak boleh dipangkas

Baca kode, ukur live, verifikasi parent atas bukti penentu. Segala penghematan hanya boleh memotong di atas lantai ini. Yang TIDAK dipangkas deep-run: dua gelombang, bukti fork-live + P&L, ledger + sapuan akhir, `/stop` kapan saja.

## 4. Persiapan / Resume

Saat dipanggil, periksa direktori proyek:

1. Jika belum ada berkas kerja kanonik: buat dari `templates/` + `methodology/` (INTEL A01, B01–B06, ledger sisa, registry invariant), lalu jalankan skill `flow-a-intel` dan `flow-b-map` sampai Tahap B selesai — autopilot tidak memburu sebelum scope dikunci.
2. Baca posisi kursor proyek dari ledger sisa + semua D00 (daftar jalur per klaster): klaster aktif, jalur aktif, tahap aktif, batch aktif, QUEUED/ACTIVE count. Lompat langsung ke langkah mesin status yang sesuai — jangan mulai dari awal bila pekerjaan sudah berjalan.
3. Jalur dan klaster yang sudah selesai penuh tidak diulang.

## 5. Mesin Status (Execution Loop) — dua lapis

### Lapis proyek

#### STEP PROJECT — Pilih pekerjaan berikut
Baca ledger sisa + semua D00. Cari klaster/jalur pertama yang belum selesai (urut EV). Bila tidak ada → STEP FINAL-SWEEP. Tentukan posisi kursor klaster itu:
- Belum punya daftar alur → STEP PATH-BUILD.
- Daftar alur ada, Tahap D belum jalan → STEP PATH-RUN dari B8.
- Tahap D selesai, Tahap E sebagian → STEP PATH-RUN dari E-BATCH (kartu tersisa saja).
- Tahap E selesai, verdict belum → STEP PATH-RUN dari G.

#### STEP PATH-BUILD — Daftar alur klaster
Enumerasi alur klaster itu (dari B02+B04) + EV per alur; tulis `D00-<KLASTER>-PATHS.md`; urut EV. Checkpoint, lalu STEP PATH-RUN untuk jalur pertamanya.

#### STEP CLUSTER-CLOSE — Tutup klaster
Bila semua jalur klaster itu selesai (QUEUED == 0 dan ACTIVE == 0 di tiap jalur): perbarui ledger sisa + D00, checkpoint, kembali ke STEP PROJECT untuk klaster berikut.

#### STEP FINAL-SWEEP — Sapuan akhir proyek
Enumerasi independen kedua (rerun sensus manual) dibanding ledger akhir; handoff terbuka + TUNDA-LIVE ditinjau; distribusi akhir dihitung di seluruh protokol. Selesai → COMPLETED, sajikan ringkasan akhir ke user (distribusi 06/07/08/09 + temuan 09 + sisa 07 beserta pemicunya).

### Lapis jalur (STEP PATH-RUN — deep-run satu jalur)

Jalankan sub-loop berikut untuk jalur aktif, tanpa tap antar langkah:

#### STEP B8 — Cek live-match jalur
Jalankan B8 untuk jalur itu (sensus kaki + cek live pin, pakai ulang cache trunk). LULUS → lanjut gelombang 1. TIDAK LIVE-MATCH → wadah TUNDA-LIVE (catat di ledger sisa, nol waktu eksekusi), ambil jalur berikut (kembali ke STEP PROJECT).

#### STEP D-WAVE1 — Gelombang 1 buta
Koordinator + 2 worker (kode saja, buta — tanpa katalog, tanpa badan fungsi klaster lain). Keluaran: OBS / G / R / Q. Verifikasi parent atas bukti penentu, lalu checkpoint.

#### STEP D-WAVE2 — Gelombang 2 berpengetahuan
Koordinator + 2 worker (katalog + cek live, pindai baru ≤10k blok, tanpa forge). Keluaran: QUEUED (P1–P5) + DORMAN + disposisi Q/RADAR. Verifikasi parent, checkpoint.

#### STEP D-LEAD-CARDS — Kartu lead + invariant
Koordinator sendiri: kartu 1:1 dari QUEUED + invariant baru ke registry (wajib sumber: field handoff atau docs/NatSpec). Gate finansial: hanya yang plafon live-nya terukur lanjut ke E; sisanya DORMAN dengan syarat buka-ulang. Verifikasi parent (subjek kartu identik kata-per-kata), checkpoint.

#### STEP E-BATCH — Uji test (loop, maks 3 per batch)
Koordinator + 3 worker, fork blok pin. Tiap batch maks 3 kartu. Parent menjalankan ulang semua forge dan mencocokkan gas (harus IDENTIK) sebelum batch dinyatakan selesai. Ulangi sampai QUEUED == 0 DAN ACTIVE == 0 untuk jalur itu. Kartu yang spawn-nya ditolak kebijakan keamanan TIDAK diulang/diparafrase/diputar — parkir, invariant-nya tetap dirumuskan.

#### STEP F — Komposisi (bersyarat)
Hanya bila penting dan urgent DAN lead-nya multi-segmen. Uji F tanpa fork/PoC ulang — pakai data terbaru (skill `flow-f-compose`). Lead satu segmen lewati F.

#### STEP G — Verdict jalur
Adili tiap kartu (06/07/08/09; tiga verdict terpisah; severity dari plafon teramati) — skill `flow-g-verdict`. Perbarui ledger + D00. Kembali ke STEP PROJECT (jalur berikut atau klaster berikut).

## 6. Protokol Delegasi ke Subagent

1. Parent memegang kendali eksklusif atas berkas kanonik + ledger + registry. Parent satu-satunya penulis berkas kanonik.
2. Sebelum mendelegasikan, parent menyusun brief berdiri sendiri (membawa semua konteks yang dibutuhkan — subagent mewarisi konteks, tapi brief tetap lengkap).
3. Subagent menulis HANYA ke `notes/staging-<jalur>-<tahap>-<item>.md`, diawali blok `## STAGING MANIFEST` maks 16 baris.
4. Parent memeriksa staging (bebas token kompresi, berjangkar verbatim), menggabungkannya ke berkas kanonik, lalu menjalankan gate tahap terkait.
5. Brief Tahap E memakai bahasa verifikasi invariant, bukan bahasa serangan.
