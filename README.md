> **Metode aktif: Flow Money First.** Sejak 22:33 WIB 16 Nov 2026 (keputusan Kim), metode kerja aktif repo ini adalah Flow Money First — aturan kanonik di `methodology/aturan-alur-first.md` dan skill di `skills/flow-*`. Metode Segitiga v2.0 (STAGE-0 + A–F) sudah tidak dipakai sebagai metode aktif; seluruh berkasnya diarsipkan utuh di `archive/segitiga-v2/` sebagai rujukan/legacy.

# theadvers — Profil Muse (Metode Segitiga v2.0)

Profil audit adversarial untuk Muse, diadaptasi dari workflow
[`thesachio/theadvers`](https://github.com/thesachio/theadvers) (branch `v2`,
HEAD `de9e418`, 2026-11-10 — sinkronisasi kedua; pertama `10af20e`). Metode
inti: **Segitiga** — `L-VALUE` (di mana nilai) → `L-PATH` (jalur sentuh) →
`L-PROVE` (bukti eksekusi). Tiga titik wajib terhubung; anomali tanpa dampak
nilai bukan temuan.

## Isi profil ini

| Berkas | Isi |
| :--- | :--- |
| `PERSONA.md` | Identitas auditor, 7 Hukum Kognitif Besi, prioritas EV P1–P5, pantangan mutlak (adaptasi `SOUL.md`). |
| `CONSTITUTION.md` | Konstitusi operasional: bootloader, wadah kanonik, PRE-STAGE-0 (INTEL.md), siklus STAGE-0 + A–F + FINAL-SWEEP, plugin, zero-compression, delegasi, gerbang, aturan bukti 06/07/08/09, VERDICT, submisi, RPC (adaptasi `AGENTS.md`). |
| `SYNC-2026-11-10.md` | Catatan sinkronisasi pertama: HEAD `10af20e`, 140 berkas, keputusan penempatan, pemetaan adaptasi, yang tidak diadopsi, verifikasi. |
| `SYNC-2026-11-10-2.md` | Catatan sinkronisasi kedua: HEAD `de9e418`, 183 berkas, perubahan metodologi (batch 3, INTEL.md, aturan wadah mekanis, fork LIVE, deploy_parity per lead, katalog M baru), uji dampak ke proyek Across. |
| `CATATAN-MUSE.md` | Catatan operasional khusus Muse: path, prosedur `--sync`, pemetaan tool, batas keamanan. |
| `projects/` | Proyek audit (skema v2) — `projects/across/` (R_1 [CLOSED] 2026-11-10 di bawah aturan sinkronisasi pertama; lihat SYNC-2026-11-10-2.md §8 untuk uji dampak aturan baru). |

Salinan referensi upstream yang setia (183 berkas, SHA terverifikasi):
`~/workspace/theadvers-source/` — `methodology/`, `references/`, `skills/`
(8 skill: sc-pre-intel + A–F + autopilot), `templates/` (termasuk
`INTEL.md` baru), `tools/`, `verify/`, `SOUL.md`, `AGENTS.md`, `README.md`,
`.gitignore`, `notes/backups/` (residu v1 + arsip per-perubahan v2).

## Memulai audit baru

```bash
# 0. PRE-STAGE-0 — isi INTEL.md dulu (prasyarat --gate 0, bukan tahap baru)
cp ~/workspace/theadvers-source/templates/INTEL.md <audit-dir>/INTEL.md
#    isi: Deployment Stage (MAINNET/TESTNET/DEVNET), identitas, URL langsung,
#    commit terpin, bukti live, Blok Referensi; lalu periksa:
python3 ~/workspace/theadvers-source/tools/verify_segitiga.py --intel <audit-dir>

# 1. Salin mesin + template ke folder proyek (tanpa notes/backups)
mkdir -p ~/workspace/theadvers/projects/<nama-proyek>/audit
cd ~/workspace/theadvers/projects/<nama-proyek>
cp ~/workspace/theadvers-source/templates/STATE.md ~/workspace/theadvers-source/templates/FACTS.md \
   ~/workspace/theadvers-source/templates/RADAR.md ~/workspace/theadvers-source/templates/path-map.md audit/
cp ~/workspace/theadvers-source/templates/0*.md audit/
cp -r ~/workspace/theadvers-source/tools ~/workspace/theadvers-source/methodology \
      ~/workspace/theadvers-source/references ~/workspace/theadvers-source/skills \
      ~/workspace/theadvers-source/verify ~/workspace/theadvers-source/templates .

# 2. Isi identitas proyek di audit/STATE.md, lalu mulai STAGE-0
python3 ~/workspace/theadvers-source/tools/verify_segitiga.py --context-packet audit
```

Atau minta Muse menjalankan `sc-pre-intel` lalu `sc-autopilot` (skill di
`~/workspace/theadvers-source/skills/sc-pre-intel/SKILL.md` dan
`sc-autopilot/SKILL.md`) — induk mengorkestrasi PRE-STAGE-0 → STAGE-0 →
A–F per wilayah → FINAL-SWEEP.

## Yang TIDAK berubah vs upstream

Metodologi (Hukum 1–7, EV P1–P5, dua gelombang C1/C2, zero-compression,
pemisahan wadah 04–09, 11 Pernyataan VERDICT termasuk #11e
anti-paper-refutation, gerbang mekanis) TIDAK diubah. Yang berubah hanya
lapisan tooling — lihat `SYNC-2026-11-10-2.md` §5 dan
`CONSTITUTION.md` §10. Perhatikan perubahan metodologi pada sinkronisasi
kedua (diterapkan di profil ini): **Rolling Batch maksimal 3** kandidat
`[ACTIVE]` per siklus D → E (dulu 5), terikat satu wilayah dengan
`Batch ID: <R_i>-B<nn>`; `INTEL.md` prasyarat `--gate 0`; aturan bukti
06/07/08/09 ditegakkan mekanis (`--rules`, `--gate E`); fork LIVE wajib
untuk proyek mainnet; `deploy_parity` wajib per lead di gate D; katalog
metode M-001..M-010 diganti total (ID lama tidak lagi menunjuk metode yang
sama — rujukan M-xxx pada proyek lama dibaca dengan katalog yang berlaku
saat wilayah itu dikerjakan).

## Status

- Profil dibuat 2026-11-10 (sinkronisasi pertama theadvers v2, HEAD
  `10af20e`); sinkronisasi kedua 2026-11-10 (HEAD `de9e418`, 183 berkas).
- Proyek pertama: Across (`projects/across/`), R_1 [CLOSED] 2026-11-10
  (distribusi akhir 06=3, 07=0, 08=4, 09=0) di bawah aturan sinkronisasi
  pertama. Uji dampak aturan baru terhadap proyek itu (read-only, tanpa
  perbaikan): `--gate 0` FAIL (belum ada INTEL.md), `--gate D` FAIL
  (Deployment Stage belum dinyatakan), `--gate E`/`--rules` FAIL
  (53 pelanggaran format wadah baru) — R_1 TIDAK dibuka kembali; delta ini
  menunggu keputusan Kim. STAGE-A R_2 belum diperintahkan.
- Profil `~/workspace/research-persona/` (v6.2.5 — Relay, Aero v2) dan
  `~/workspace/theadver/` (default — Velocity) TIDAK disentuh.
- Tanpa draft/submisi/kontak program tanpa persetujuan eksplisit Kim.
  Tersimpan ≠ tersubmit.
