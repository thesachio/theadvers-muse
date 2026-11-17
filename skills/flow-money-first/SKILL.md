# flow-money-first

Skill pintu masuk metode Flow Money First — audit keamanan smart contract yang mempartisi berdasarkan aliran uang, bukan wadahnya (berkas). Dipakai bila user menyebut flow money first / alur-first, meminta audit dengan Tahap A–G, atau memulai proyek audit baru dengan metode ini. Unit kerja = satu aliran uang ujung-ke-ujung; tiap aliran diukur nilai live-nya sejak awal, diburu per segmen dan handoff, dibuktikan di fork live dengan P&L terukur.

## Bootloader (wajib, sebelum kerja apa pun)

Baca berkas-berkas ini (jalur relatif terhadap root repo ini):

1. `SOUL.md` — identitas + 7 Hukum Kognitif Besi + lantai bawah (versi flow, di root repo ini).
2. `methodology/aturan-alur-first.md` — aturan kanonik (A1–A5, B1–B16, C1–C11, Tahap C–G). **Ini sumber kebenaran.** Bila SKILL.md ini berbeda dengannya, berkas aturan yang berlaku.
3. `methodology/resume-aturan-preferensi-alur-first.md` — ringkasan aturan + preferensi mengikat.
4. `methodology/acuan-workflow-tahap-B.md` — bila mengerjakan Tahap B.
5. `methodology/rancangan-hemat-fase-d.md` — bila mengerjakan Fase D/E (aturan hemat + prinsip efisiensi + lantai bawah).
6. `AGENTS.md` — konstitusi operasional flow (delegasi, gerbang) di root repo ini.

Catatan: `SOUL.md` / `AGENTS.md` di root repo ini adalah versi flow. Versi Metode Segitiga-nya diarsipkan sebagai `archive/segitiga-v2/SOUL-segitiga-v2.md` dan `archive/segitiga-v2/AGENTS-segitiga-v2.md`.

## Peta tahap (ringkas)

A INTEL (brief 9 butir, pin commit + blok referensi) → B Mapping (sensus 100% + klaster + jahitan + ledger sisa + registry) → C pemetaan mendalam per klaster (inventaris alur + segmentasi + kontrak handoff deskriptif) → D eksplorasi per-alur (gelombang 1 buta → gelombang 2 berpengetahuan → kartu lead + invariant) → E uji test (PoC fork-live + P&L terukur, batch maks 3) → F komposisi (bersyarat — hanya bila penting dan urgent; uji pakai data terbaru, tanpa fork/PoC ulang) → G verdict (06/07/08/09 + distribusi akhir). Rolling per klaster/alur: C → D → E. Tahap F lintas-klaster + Tahap G di akhir.

## Gerbang yang tidak boleh dilanggar

- B13 scope lock sebelum sensus (daftar berkas cocok persis, diff kosong, commit terverifikasi).
- B8 live-match diulang di awal fase uji per klaster, sebelum PoC. Tidak live-match → wadah TUNDA-LIVE (tercatat di ledger, nol waktu eksekusi).
- Gate finansial di Tahap D (C10): lead masuk Tahap E bila kerugian yang bisa dibuktikan — mekanisme jalan di fork/test + plafon terukur + pemicu terjangkau tanpa peran istimewa/state eksotis. Plafon live 0 TIDAK membuat lead DORMAN.
- Batch Tahap E maksimal 3 kartu.
- Lantai bawah: baca kode, ukur live, verifikasi parent atas bukti penentu. Tidak boleh dipangkas.
- HOLD: nol submisi / kontak program / draf laporan tanpa persetujuan eksplisit per kandidat.
- `/stop` atau perintah berhenti eksplisit mencabut otorisasi round seketika: hentikan subagent, tulis nol berkas.

## Katalog (basis pengetahuan)

Katalog 52 plugin ada di `references/` (CHAIN 10 + M 10 + POV 15) + `methodology/attack-forms/` (AF 10) + `methodology/domains/` (DOM 7). Aturan pakai: gelombang 1 (buta) DILARANG membuka katalog; gelombang 2 baru memuat plugin yang gejalanya cocok. Temuan di luar katalog = kandidat plugin baru.

## Delegasi

Brief berdiri sendiri (membawa semua konteks yang dibutuhkan). Sekuensial, satu anak aktif per antrean. Deep-run per jalur (diputus 22:33 WIB 16 Nov 2026): satu perintah berdiri hulu→hilir sampai verdict per lead; berlaku mulai jalur berikut (P4). Koordinator memverifikasi verdict dari berkas bukti sebelum menerapkannya ke antrean bersama. Induk satu-satunya penulis berkas kanonik + ledger sisa. Brief Tahap E memakai bahasa verifikasi invariant, bukan bahasa serangan. Penolakan safety classifier tidak boleh diulang, diparafrase, atau diputar lewat jalur lain.

## Yang belum ada

Skill per tahap (A, B, C, D, E, F, G) + autopilot SUDAH DITULIS (`skills/flow-a-intel/`, `flow-b-map/`, `flow-c-inventory/`, `flow-d-explore/`, `flow-e-test/`, `flow-f-compose/`, `flow-g-verdict/`, `flow-autopilot/`). Skill ini adalah pintu masuk runtime; kerja tiap tahap mengikuti berkas aturan + skill tahapnya, bukan SKILL.md ini.
