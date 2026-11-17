# AGENTS — Konstitusi Operasional Flow Money First (moneyflow v1.0)

## 0. Bootloader (keras)

Sebelum kerja apa pun, baca `SOUL.md` dan `methodology/aturan-alur-first.md` sampai paham. Berkas aturan kanonik adalah sumber kebenaran metode; bila berkas lain (termasuk berkas ini) berbeda dengannya, berkas aturan yang berlaku. Preferensi mengikat ada di `methodology/resume-aturan-preferensi-alur-first.md` (ringkasan) — yang mengikat tetap berkas aturan.

Repo ini adalah MESIN/METODE, bukan proyek audit. Tidak memuat kode kontrak target dan tidak memuat temuan. Dipakai dengan cara disalin ke folder kerja audit baru, lalu dijalankan tahap demi tahap.

## 1. Tahap A–G

| Tahap | Nama | Output | Batas |
| :--- | :--- | :--- | :--- |
| A | INTEL | `A01-INTEL` (brief 9 butir), `A02-INTEL-discovery` | Brief saja (A1–A5); tanpa cek onchain |
| B | Mapping | `B01-SCOPE`, `B02-CENSUS-STATIC`, `B03-CENSUS-LIVE`, `B04-CLUSTERS`, `B05-LEDGER-SISA`, `B06-REGISTRY-INVARIANT` | Tanpa invariant, tanpa pemetaan mendalam, tanpa buruan/PoC/verdict |
| C | Pemetaan mendalam per klaster | `C<NN>-<KLASTER>-FLOW-INVENTORY` | Deskriptif saja; tanpa invariant/buruan/PoC/verdict; jahitan adalah batas |
| D | Eksplorasi | Kartu lead + invariant (`INV-Sx/Hx/Jx/Fx`) | Per-alur, dua gelombang (buta → berpengetahuan + katalog); tanpa PoC |
| E | Uji test | PoC fork-live + P&L per kandidat | Fork blok pin; P&L terukur; batch maks 3 |
| F | Komposisi (bersyarat — hanya bila penting dan urgent) | Lemma dari data terbaru (tanpa fork/PoC ulang) | Hanya lead multi-segmen |
| G | Verdict | Verdict 06/07/08/09 per kandidat + distribusi akhir | Tiga verdict terpisah |

Rolling per klaster: C → D → E. Tahap F lintas-klaster + Tahap G di akhir. Deep-run per jalur (diputus 22:33 WIB 16 Nov 2026): satu perintah berdiri hulu→hilir sampai verdict per lead, tanpa tap antar langkah; berlaku mulai jalur berikut (P4). Acuan kerja Tahap B: `methodology/acuan-workflow-tahap-B.md`. Aturan hemat Fase D + prinsip efisiensi + lantai bawah: `methodology/rancangan-hemat-fase-d.md`.

## 2. Delegasi (keras)

- Brief berdiri sendiri: tiap brief delegasi membawa semua konteks yang dibutuhkannya (identifikasi, jangkar, batasan) — subagent tidak mewarisi konteks parent.
- Sekuensial: satu anak aktif per antrean. Koordinator memverifikasi verdict dari berkas bukti sebelum menerapkannya ke antrean bersama; subagent tidak menulis antrean bersama secara langsung.
- Induk satu-satunya penulis berkas kanonik + ledger sisa.
- Brief Tahap E memakai bahasa verifikasi invariant, bukan bahasa serangan.
- Verifikasi parent atas bukti penentu wajib jalan (jangkar, angka, rerun forge) — ini lantai bawah, tidak boleh dipangkas.
- Penolakan safety classifier TIDAK boleh diulang, diparafrase, atau diputar lewat jalur lain. Pivot saja.
- Verifikasi jangkar: periksa FUNGSI-nya lewat `grep -n`, bukan hanya barisnya. Baris yang ada ≠ jangkar yang benar. Rantai koreksi bukan bukti — tiap langkah berikut wajib memeriksa ulang jangkar yang dikoreksi dari kode bernomor.

## 3. Gerbang (keras)

- B13 scope lock sebelum sensus: daftar berkas cocok persis inventaris kanonik (diff kosong), commit terverifikasi.
- B8 live-match diulang di awal fase uji per klaster, sebelum PoC. Tidak live-match → TUNDA-LIVE (tercatat di ledger, nol waktu eksekusi). Cara kerja B8 (17 Nov 2026, mengikat): utamakan alur + cek desain kode dulu; pindai log tx = verifikator TERAKHIR saja (Tahap G / sapuan akhir), bukan cara kerja B8.
- Gate finansial di Tahap D (C10, direvisi 17 Nov 2026): lead masuk Tahap E bila kerugian yang bisa dibuktikan — mekanisme jalan di fork/test atas kode+state nyata + plafon terukur + pemicu terjangkau lewat jalur operasi normal (tanpa peran istimewa/state eksotis). Plafon live 0 TIDAK lagi membuat lead DORMAN.
- Batch Tahap E maksimal 3 kartu.
- B10: round selesai hanya bila ledger sisa menyusut. Target buruan harus ada di sensus.

## 4. Preferensi mengikat (C1–C11, ringkas — lengkapnya di berkas aturan)

C1 fork-live. C2 kejujuran bukti (DIEKSEKUSI vs DIANALISIS vs CODE-REVIEW-ONLY). C3 verify_zero_and_scan. C4 ambang $500 bukan ambang batas. C5 HOLD — nol submisi/kontak program/draf tanpa persetujuan eksplisit per kandidat. C6 tiga verdict terpisah; severity dari plafon teramati; insidensi same-tx. C7 bahasa Indonesia ringan dan sederhana. C8 keep-hunting. C9 /stop mencabut otorisasi round seketika. C10 kerugian = yang bisa dibuktikan (mekanisme + plafon terukur + pemicu terjangkau; TIDAK harus live). C11 TERBUKTI = cukup jalur pendek saling bergantung dari pelanggaran invariant ke kerugian terukur; tes yang berhenti di satu fungsi pendek = masih HIPOTESIS.

## 5. Lantai bawah + hemat

Lantai bawah: baca kode, ukur live, verifikasi parent atas bukti penentu. Yang jangan dipangkas: bukti fork live, pemeriksaan parent atas bukti penentu, sensus 100%, jangan saring di HEAD dulu. Hemat yang diizinkan: satu fork per wilayah dipakai ulang, deploy_parity sekali + cache, satu harness dua mode, triase fork-testability di awal, pipeline (bukan paralel buta), cek mesin otomatis, pre-check murah sebelum gate. Prinsip efisiensi: bila satu tes penentu sudah bisa mengonfirmasi, jangan jalankan pindai sejarah panjang dulu — pindai hanya konteks pendukung, dibatasi atau lewat indexer.

## 6. Yang belum ada di repo ini (terbuka, bukan rule)

- Skill per Tahap A–G + autopilot SUDAH DITULIS (22:39 WIB 16 Nov 2026): `skills/flow-a-intel/`, `flow-b-map/`, `flow-c-inventory/`, `flow-d-explore/`, `flow-e-test/`, `flow-f-compose/`, `flow-g-verdict/`, `flow-autopilot/`. Autopilot ditulis ulang 22:47 WIB 16 Nov 2026 menjadi full-autopilot proyek penuh (semua klaster, semua jalur) — versi pertama hanya satu jalur. Skill `sc-*` theadvers sengaja TIDAK disalin — skill itu mengkodekan STAGE-0 + A–F Segitiga. Lihat `skills/README.md`. Skill pintu masuk `skills/flow-money-first/SKILL.md` SUDAH ADA.
- `tools/verify_flow.py` (verifier format untuk artefak A–G). `verify_segitiga.py` theadvers sengaja TIDAK disalin — pemeriksa itu terikat format Segitiga.
- Inventory per-alur (Tahap C) — dievaluasi, tidak diadopsi; bisa dibuka lagi. Cara parser independen sapuan akhir (B9) — terbuka sebagai riset.
- Katalog 52 plugin (`references/` + `methodology/attack-forms/` + `methodology/domains/`) diwarisi utuh dari theadvers dan tumbuh dari hasil jelajah (temuan di luar katalog = kandidat plugin baru).
