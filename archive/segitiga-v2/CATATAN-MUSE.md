# CATATAN-MUSE — theadvers v2.0 di Lingkungan Muse

Catatan operasional khusus Muse. Metodologi ada di `PERSONA.md` /
`CONSTITUTION.md`; berkas ini hanya mencatat pemetaan tooling dan batas
yang spesifik untuk mesin ini.

## Path

- Profil: `~/workspace/theadvers/` (`PERSONA.md`, `CONSTITUTION.md`,
  `README.md`, `SYNC-2026-11-10.md`, `SYNC-2026-11-10-2.md`, berkas ini)
- Referensi upstream (183 berkas, jangan diedit — salinan setia):
  `~/workspace/theadvers-source/` (sinkronisasi kedua, HEAD `de9e418`)
- Proyek audit baru: `~/workspace/theadvers/projects/<nama-proyek>/`
  (berisi salinan kerja `tools/`, `methodology/`, `references/`, `skills/`,
  `templates/`, `verify/` + folder `audit/` dengan INTEL.md, STATE/FACTS/RADAR/
  path-map/04–09)
- Alat gate: `~/workspace/theadvers-source/tools/verify_segitiga.py`
  (dibaca dari referensi; TIDAK disalin per proyek kecuali diperlukan —
  salinan kerja proyek boleh membawa salinannya sendiri, lihat README profil)

## Prosedur `--sync` (router plugin) di Muse

Upstream `--sync` melakukan dua hal: (a) membangun ulang kelima
`_ROUTER.md` dari header 4-baris plugin, (b) menyalin skill ke
`~/.hermes/skills`. Langkah (b) TIDAK berlaku di Muse.

- JANGAN jalankan `--sync` terhadap `~/workspace/theadvers-source/` —
  langkah (a) akan menimpa berkas referensi.
- Jalankan `--sync` hanya di dalam salinan kerja proyek, dengan:

      HERMES_HOME=/tmp/theadvers-hermes-scratch python3 tools/verify_segitiga.py --sync

  Langkah (b) mendarat di direktori scratch yang dibuang; langkah (a)
  membangun ulang router di proyek. Hapus scratch sesudahnya bila mau
  (`rm -rf /tmp/theadvers-hermes-scratch`).
- Diverifikasi statis (baca `main()` + pemanggil
  `sync_skills_to_hermes_profile()`): HANYA `--sync` yang memanggil instalasi
  skill Hermes. `--context-packet`, `--gate`, `--close-region`, dan
  `--final` adalah operasi lokal murni (baca berkas audit + cetak) dan boleh
  dijalankan normal dari salinan referensi.

## Pemetaan tool

| Upstream | Muse |
| :--- | :--- |
| `delegate_task` | `subagent.spawn` sekuensial (1 anak aktif per antrean kerja), brief berdiri sendiri berisi output `--context-packet` utuh + kutipan aturan tahap + batas tulis staging. Klaim "konteks 0-token" TIDAK berlaku (subagent mewarisi konteks). |
| `skill_view` / `read_file` | `muse.read` langsung. Skill dibaca dari `~/workspace/theadvers-source/skills/<nama>/SKILL.md`; plugin dibaca dari `methodology/` / `references/` di referensi. Skill ada 8: `sc-pre-intel` (PRE-STAGE-0) + `sc-a-scope`, `sc-b-map`, `sc-c-hunt`, `sc-d-prove`, `sc-e-verdict`, `sc-f-report`, `sc-autopilot`. |
| `read_file` di _ROUTER | Sama: baca langsung hanya file plugin yang Trigger Symptom-nya cocok (lazy-load). |
| Manifes staging | `## STAGING MANIFEST` ≤16 baris di `notes/staging-<region>-<stage>-<item>.md` (v2; bukan 12 seperti v6). |
| STATE.md | Hanya induk yang menulis (Parent Orchestrator / Hakim). Subagent dilarang menyentuhnya. |

## Alat deployment & ingest

- `deploy_parity.py`: jalankan hanya dengan `--rpc <endpoint-publik>`
  eksplisit + `--paradigm`/`--chain`/`--addr`/`--entrypoints` yang Kim
  setujui untuk proyek itu. Jangan mengandalkan `load_env_rpc()`; tidak ada
  `.env.rpc` di lingkungan ini. RPC gagal 2x → `ENV_ERROR`, bukan bukti
  ketiadaan (aturan alat itu sendiri). Sejak sinkronisasi kedua,
  `deploy_parity` WAJIB per lead di STAGE-D (artefak
  `notes/deploy-parity-<LEAD-ID>.json`, dirujuk `DEPLOY_REPORT` di kartu) dan
  per entri 06/08/09 di STAGE-E — `--gate D`/`--gate E` menolak tanpa artefak.
  `--selftest` butuh `cast` di PATH (`~/.foundry/bin/`, bukan PATH default);
  tanpa itu kasus EVM mengembalikan `UNVERIFIABLE` (terverifikasi 2026-11-10:
  12 pass / 0 fail / 2 skip dengan cast di PATH; 5/7/2 tanpanya).
- `ingest.py`: `--dest` wajib eksplisit setiap kali (default upstream
  `~/.hermes/.../cache/inbox` tidak ada). Alat ini meng-clone/pull repo dari
  sumber yang dilewatkan — jalankan hanya untuk sumber yang Kim setujui.
  Output `MECH-OK` = lolos saringan mekanis, BUKAN izin kirim.

## Batas keamanan yang tetap mengikat (dari CONSTITUTION.md §1/§8/§9)

- Lokal dulu; testnet akun throwaway bila bukti eksekusi tidak bisa didapat
  lokal; mainnet hanya dengan persetujuan eksplisit per aksi. Tanpa itu:
  tanpa `--broadcast`, tanpa ubah state mainnet/testnet, tanpa private key
  dana nyata.
- RPC: endpoint publik saja, izin Kim per proyek. Tanpa secret di berkas /
  argumen / log. Tanpa kunci Alchemy.
- Submisi: STAGE-F berhenti di draf siap-kirim. Email/platform/kontak program
  butuh persetujuan eksplisit Kim per pengiriman. Tersimpan ≠ tersubmit.
- Data proyek hanya di folder proyek — tidak pernah di memori jangka panjang
  Muse atau skill global.
- Kejujuran bukti: eksekusi vs penalaran dipisah tegas; sumber terblokir
  bukan bukti negatif; klaim nol dicurigai sampai ada kontrol positif.

## Perbedaan penamaan vs v6.2.5 (research-persona) — jangan tertukar

- Tahap: STAGE-0/A/B/C1/C2/D/E/F + FINAL-SWEEP (bukan Fase 0–4).
- Wilayah: R_1..R_N (bukan unit S-01..S-14); antrean di STATE.md.
- Wadah kerja: STATE + FACTS + RADAR + path-map + 04–09 + cases/
  (bukan 00/00b/01/02/03) + `INTEL.md` sebagai prasyarat STAGE-0
  (bukan tahap baru; skill `sc-pre-intel`, perintah `--intel`).
- Skill: sc-pre-intel, sc-a-scope, sc-b-map, sc-c-hunt, sc-d-prove,
  sc-e-verdict, sc-f-report, sc-autopilot (bukan sc-phase1..4).
- Batch: maksimal **3** `[ACTIVE]` per batch, satu `Batch ID`
  `<R_i>-B<nn>` per wilayah (dulu ≤5 tanpa ID batch).
- Validator: `verify_segitiga.py` — perintah baru `--intel` dan `--rules`;
  `--gate 0` menagih INTEL.md; `--gate D` menagih artefak deploy_parity per
  lead; `--gate E` menegakkan aturan wadah 06/07/08/09 + jejak lead #11d.
- Profil v6.2.5 tetap hidup terpisah di `~/workspace/research-persona/`
  untuk proyek yang sudah selesai — jangan mencampur berkas antar profil.
