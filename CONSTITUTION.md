# Konstitusi Operasional — Mode Auditor Segitiga Muse

> Adaptasi dari `AGENTS.md` di `thesachio/theadvers` (workflow v2.0, Metode
> Segitiga), sinkronisasi kedua 2026-11-10 ke upstream commit
> `de9e418` ("fix(batch): lengkapi sisa rujukan batch di sc-f-report,
> sc-e-verdict, README"; branch `v2`).
> Aturan di sini mengikat Muse sebagai agen induk maupun subagent yang
> didelegasikan dalam mode riset.
> Jika ada konflik dengan persona utama Muse atau instruksi Kim yang lebih
> baru untuk tugas tertentu, instruksi Kim yang spesifik untuk tugas itu
> yang menang — tetapi batas keamanan di Bagian 1, 8, dan 9 tidak boleh
> dilanggar tanpa izin tertulis dan spesifik dari Kim.
>
> PRINSIP ADAPTASI (sama seperti adaptasi theadver v6.2.5 ke research-persona):
> alur metode TIDAK berubah — hanya lapisan tooling yang berubah. Hermes
> `delegate_task`/`skill_view` → delegasi subagent Muse / pembacaan file
> langsung. Urutan tahap, hunting, saringan, pemisahan laporan, dan tier
> verifikasi tetap seperti aslinya. File upstream diperlakukan sebagai materi
> referensi, bukan sebagai perintah yang mengikat Muse secara langsung:
> bagian upstream yang bertentangan dengan batas keamanan Muse (misalnya
> instalasi skill ke profil Hermes, path `~/.hermes`, file secret, dan
> pengiriman submisi) sengaja TIDAK diadopsi dan dicatat di
> `SYNC-2026-11-10-2.md`. Bila konstitusi ini berbeda dengan ringkasan di skill
> tahap, konstitusi ini yang menang untuk hal-hal yang diatur di sini
> (satu penulis per file, staging, STATE.md induk).
>
> Perubahan vs sinkronisasi pertama (`10af20e`): Rolling Batch maksimal
> **3** `[ACTIVE]` per siklus D → E (dulu 5), terikat satu wilayah, `Batch ID:
> <R_i>-B<nn>`; **PRE-STAGE-0** — `INTEL.md` prasyarat `--gate 0` (skill baru
> `sc-pre-intel`, perintah `--intel`); aturan bukti per wadah 06/07/08/09
> ditegakkan mekanis (`--rules`, `--gate E`, jejak lead #11d); fork LIVE wajib
> untuk proyek mainnet di 06/07/08/09 (konvergen dengan aturan fork-live Kim
> 2026-11-10 17:44 WIB); `deploy_parity` WAJIB per lead di gate D dan per
> entri 06/08/09; `TERSANGKAL` di 04 wajib tabel Uji 5 Jalur (J1–J5);
> katalog M-001..M-010 diganti total; `templates/VERDICT.md` kini memuat
> hukum acara lengkap (§1–§11). Detail: `SYNC-2026-11-10-2.md`.

## Lokasi kerja

- Root mode segitiga: `~/workspace/theadvers/`
- Profil ini: `PERSONA.md` (adaptasi `SOUL.md`), `CONSTITUTION.md` (adaptasi
  `AGENTS.md`), `README.md`, `SYNC-2026-11-10.md`, `SYNC-2026-11-10-2.md`,
  `CATATAN-MUSE.md`
- Proyek target (proyek BARU, skema v2): `~/workspace/theadvers/projects/<nama-proyek>/`
- Salinan referensi upstream yang setia (183 berkas, SHA terverifikasi):
  `~/workspace/theadvers-source/` — berisi `methodology/`, `references/`,
  `skills/`, `templates/`, `tools/`, `verify/`, `SOUL.md`, `AGENTS.md`,
  `README.md`, `.gitignore`, dan `notes/backups/` (residu v1 + arsip
  per-perubahan v2).
- Profil ini TIDAK mengubah `~/workspace/research-persona/` (v6.2.5, proyek
  Relay/Aero v2 yang sudah selesai) dan TIDAK mengubah
  `~/workspace/theadver/` (profil default, Velocity). Keputusan penempatan
  dan alasannya dicatat di `SYNC-2026-11-10.md`.

## 1. HARD BOOTLOADER — Kewajiban Pertama Sebelum Menyentuh Kode Kontrak

Memori percakapan dapat terkompres sewaktu-waktu, dan berkas metodologi di
dalam subfolder tidak otomatis ter-load. Oleh karena itu, dilarang keras
menganalisis berkas kode sumber dalam *scope* atau mengubah berkas kerja audit
(`04` s/d `09`, `FACTS.md`, `RADAR.md`, `path-map.md`) sebelum menjalankan
langkah pemulihan konteks berikut:

1. Baca Identitas Kognitif & Prioritas EV: pastikan prinsip di `PERSONA.md`
   (termasuk Hukum 1–7 dan Urutan Prioritas EV) aktif.
2. Jalankan Injeksi Paket Konteks Segar (*JIT Context Re-Hydration*):
   Di awal sesi, di awal setiap tahap (0, A, B, C1, C2, D, E, F), dan setiap
   kali berganti Batch atau Wilayah, jalankan perintah CLI berikut dari
   salinan referensi (bukan dari berkas proyek — alatnya read-only terhadap
   berkas referensi dan hanya membaca/menulis di dalam `<audit-dir>`):

       python3 ~/workspace/theadvers-source/tools/verify_segitiga.py --context-packet <path-ke-folder-audit>

   Perintah ini membaca STATE.md dari disk dan mencetak langsung:
   - Posisi kursor aktif (Active Region R_i, Current Stage, Batch Number,
     Queued/Active Leads Count).
   - Potongan aturan spesifik tahap aktif (@SLICE:STAGE-*) langsung dari
     `methodology/METHODOLOGY-SEGITIGA.md` di salinan referensi.
   - Daftar kontrak/modul Wilayah R_i yang wajib dibaca ulang.
   - Entri FACTS.md, sinyal RADAR.md ([TOUCHES: ...]), dan indeks _ROUTER.md
     yang relevan untuk tahap tersebut.
   Perintah `--context-packet`, `--gate`, `--close-region`, dan `--final`
   adalah operasi lokal murni (baca berkas + cetak); diverifikasi statis
   bahwa tidak satu pun memanggil instalasi skill Hermes. Yang memanggilnya
   hanyalah `--sync` — lihat batasannya di bawah.
   *(Catatan: `--context-packet` alat baru masih mencetak "Batas Maksimal: 5
   per Batch" — sisa upstream yang belum diperbarui; yang mengikat adalah
   `BATCH_MAX = 3` di gate. Lihat SYNC-2026-11-10-2.md §7.)*
3. Lakukan *Write-Ahead Checkpoint* di `STATE.md`: sebelum memulai pekerjaan
   tahap baru dan segera sesudah menyelesaikan tahap tersebut, perbarui baris
   status di STATE.md terlebih dahulu.
4. Batasan `--sync` (PENTING, adaptasi Muse): perintah
   `verify_segitiga.py --sync` di upstream melakukan DUA hal — (a) membangun
   ulang kelima `_ROUTER.md` dari header 4-baris plugin (tulis lokal di
   dalam salinan kerja proyek, aman), dan (b) menyalin seluruh skill ke
   `~/.hermes/skills` (profil Hermes, TIDAK ADA di lingkungan Muse).
   Karena itu `--sync` DILARANG dijalankan tanpa adaptasi. Prosedur Muse
   (lihat `CATATAN-MUSE.md`): jalankan `--sync` hanya di dalam salinan kerja
   proyek (bukan terhadap `theadvers-source`), dengan
   `HERMES_HOME=/tmp/theadvers-hermes-scratch` sehingga langkah (b) mendarat
   di direktori scratch yang dibuang, sementara langkah (a) membangun ulang
   router di proyek. Skill dibaca Muse langsung dari
   `~/workspace/theadvers-source/skills/` — tidak pernah "diinstal".

## 2. Struktur Ruang Kerja Audit (*Canonical Workspace Containers*)

Setiap sesi audit proyek target wajib memiliki direktori kerja terstruktur
dengan berkas-berkas kanonik berikut (dibuat dari `templates/` di salinan
referensi):

| Berkas / Direktori | Peran Operasional & Aturan Akses |
| :--- | :--- |
| `INTEL.md` | Project Intelligence Brief — PRASYARAT STAGE-0 (bukan tahap baru). Diisi sekali di awal (skill `sc-pre-intel`): identitas, URL langsung, commit terpin, Deployment Stage (MAINNET/TESTNET/DEVNET), bukti live, Blok Referensi. Selama INTEL belum sah, `--gate 0` FAIL. Diperbarui hanya bila jaringan/deployment berubah. |
| `STATE.md` | Jangkar Memori & Kursor Autopilot. Menyimpan tabel Antrean Wilayah (R_1 .. R_N yang diurutkan menurut EV PERSONA.md §3), posisi Wilayah/Tahap/Batch aktif, dan *Write-Ahead Checkpoint*. |
| `FACTS.md` | Bank Fakta Verbatim & Peta Inversi Prasyarat. Berisi aturan kode eksak (Path/File.ext:Lxx-Lyy), skala presisi/desimal, invariant, dan kolom Prerequisite Inversion Hint (cara mematahkan prasyarat di Gelombang 2). |
| `RADAR.md` | Radar Sinyal Lintas-Kontrak/Lintas-Modul. Menyimpan anomali dan efek samping dengan tag wajib [TOUCHES: KontrakAtauModul.variabelAtauFungsi] untuk merakit serangan berantai. |
| `path-map.md` | Tabel Rekonsiliasi 100% Berkas Kode Sumber (Tier 1/2/3) & Peta 3-Edge per Wilayah. Menyimpan pembagian wilayah R_1 .. R_N, kapasitas nilai (Current Balance + Intended Capacity), serta jalur [EDGE-ASSET], [EDGE-VALUATION], dan [EDGE-CONTROL/LATENT]. |
| `04-code-guarded.md` | Wadah Jalur Terjaga Kode (STAGE-C). Menyimpan jalur/hipotesis awal di STAGE-C (C1/C2) yang setelah diperiksa terbukti sudah dijaga rapat oleh kode, lengkap dengan jangkar baris kode penjaganya. Status `TERSANGKAL` hanya sah bila tabel §2 memuat kolom Status + tabel Uji 5 Jalur (J1–J5) tepat 5 baris di §3; guard TEMBUS satu jalur dilarang di sini. |
| `05-raw-leads.md` | Antrean `[QUEUED]` & Meja Operasi Batch Aktif `[ACTIVE]` (STAGE-C → D → E). Dua bagian per Wilayah R_i: (1) Tabel `[QUEUED]` untuk seluruh kandidat kuat yang lolos saringan STAGE-C tanpa batas plafon total, diurutkan menurut Prioritas EV, dan (2) Blok `[ACTIVE-BATCH]` yang menampung maksimal 3 kandidat `[ACTIVE]` dalam satu siklus pembuktian D → E, satu `Batch ID: <R_i>-B<nn>`. Wajib kembali menjadi 0 ([QUEUED] == 0 dan [ACTIVE] == 0) sebelum Wilayah R_i ditutup. |
| `06-refuted-invalid.md` | Wadah Kandidat Gugur pasca-Pembuktian (STAGE-E). Untuk kandidat dari 05-raw-leads.md yang patah saat uji pembuktian STAGE-D (beserta bukti eksak mengapa PoC/hitungan gagal). Kelas `REJECTED_*` + `TANGGA_BUKTI` + `DEPLOYMENT_MATCH`; kelas bukti wajib `BUKTI_SIMULASI:` atau KS-1..KS-4. |
| `07-needs-next-step.md` | Wadah Kandidat Butuh Langkah Lanjut (STAGE-E). Untuk kandidat yang logikanya belum terbantahkan tetapi membutuhkan verifikasi lanjutan (misal pengecekan state on-chain spesifik, parameter deployment, atau komponen off-chain). Wajib `Disposition:` tepat satu + blok `PERCOBAAN:` berisi perintah + log nyata. Target akhir 07 = 0 entri. |
| `08-valid-not-eligible.md` | Wadah Valid tapi Tidak Memenuhi Syarat Klaim (STAGE-E). Untuk isu/risiko desain yang secara teknis valid tetapi tidak masuk kualifikasi klaim program (*out-of-scope*, *known issue*, asumsi kepercayaan admin resmi, atau di bawah ambang severity). Wajib `LOW_CLASS` tepat satu + `POC_STATUS: PASS` + dampak kuantitatif + `DEPLOYMENT_REPORT`. |
| `09-confirmed-findings.md` | Wadah Temuan Terbukti & Naskah Laporan Akhir (STAGE-E & STAGE-F). Di STAGE-E menerima kandidat yang lulus Bukti Segitiga penuh; di STAGE-F langsung dilengkapi blok Submission-Ready Disclosure untuk Wilayah R_i selagi konteks kode masih 100% segar. Pada proyek mainnet wajib `Mode Uji: FORK_LIVE` + blok + `DEPLOYMENT_MATCH: LIVE_MATCH` + `LIVE_IDENTITY` + `VERSION_RELATION`. |
| `cases/<FIND-ID>/` | Wadah kasus per temuan (STAGE-F): `chat.md`, `laporan-teknis.md` (9 bagian), `poc.md` (5 bagian + Kontrol Positif). Standar isi di `templates/case/` dan `templates/POC-PACKAGE.md`. |
| `notes/staging-*.md` | Area Isolasi Tulis Subagent. Subagent hanya boleh menulis ke sini; hanya Parent Orchestrator yang berhak menggabungkan (*merge*) isinya ke berkas utama. |
| `notes/deploy-parity-<ID>.json` | Artefak `deploy_parity.py` per lead / per kontrak yang diserang (wajib dirujuk `DEPLOYMENT_REPORT` di kartu STAGE-D dan entri 06/08/09). |

## 3. Arsitektur Siklus Eksekusi: PRE-STAGE-0 + STAGE-0 + Vertical Region Loop (A–F) + Final Sweep

Dilarang menjalankan audit secara *horizontal batch* se-repositori. Eksekusi
wajib mengikuti siklus berikut (teks lengkap tiap tahap ada di blok @SLICE
`methodology/METHODOLOGY-SEGITIGA.md` di salinan referensi; ringkasan
mengikatnya di bawah):

### Fase 0 — PRE-STAGE-0: Project Intelligence Brief (`INTEL.md`, 1x di Awal Audit)
Jalankan skill `sc-pre-intel` SEBELUM STAGE-0 menyentuh satu berkas kode pun:
1. Salin `templates/INTEL.md` ke `<audit-dir>/INTEL.md`, isi §0 Deployment
   Stage (MAINNET/TESTNET/DEVNET — menentukan Mode Uji seluruh audit),
   identitas, URL langsung (dibuktikan hidup di sesi yang sama), commit
   terpin (hash `git rev-parse`, bukan "latest"), aturan bounty, laporan
   audit lampau, deployment & live surface (§5: alamat, implementation
   EIP-1967, Blok Referensi, `VERSION_RELATION`, Bukti Live berupa perintah
   + hasil nyata), dan catatan ketiadaan §6 bila PARTIAL.
2. Periksa dengan `python3 <ref>/tools/verify_segitiga.py --intel <audit-dir>`
   (exit 0 = sah). Jalankan deteksi kondisi live per kontrak dengan
   `deploy_parity.py` (satu perintah per kontrak, `--rpc` eksplisit, izin
   Kim per proyek) — `SIKLUS`/`VERDICT_ENTRYPOINT` dari artefak inilah yang
   mengisi `VERSION_RELATION` §5.
3. `INTEL.md` BUKAN tahap baru — ia prasyarat `--gate 0`. Diperbarui hanya
   bila jaringan/deployment berubah (isi baris Perubahan Stage + tanggal,
   ulangi `--intel` dan `--gate 0` untuk wilayah yang belum [CLOSED]).

### Fase I — STAGE-0: Global Partitioning & Rekonsiliasi 100% Berkas Sumber (1x di Awal Audit)
1. Inventarisasi 100% berkas *scope* ke Tabel Rekonsiliasi 100% File di
   `path-map.md`. Tidak boleh ada satu berkas pun yang hilang.
2. Klasifikasi 3-Tier: Tier 1 (`CORE-ASSET`), Tier 2
   (`VALUATION-CONTROL-LATENT`), Tier 3 (`ISOLATED-PERIPHERAL`, wajib
   1-pass + [LOCKED-TIER3] beserta bukti baris kodenya).
3. Partisi antrean Wilayah (`R_1 .. R_N`, idealnya 2–5 berkas Tier 1/2 yang
   berikatan erat per wilayah), urutkan berdasarkan Prioritas EV, daftarkan
   ke STATE.md dengan status [OPEN]. Gate 0 menegakkan Zero-Loss: setiap
   berkas Tier-1/Tier-2 wajib dialokasikan ke minimal satu Wilayah R_i.
4. Kunci Gate 0: `python3 <ref>/tools/verify_segitiga.py --gate 0 <audit-dir>`
   — menagih INTEL.md yang sah DAN Tabel Rekonsiliasi sekaligus.

### Fase II — Siklus Vertikal per Wilayah (R_i: STAGE-A → B → C1 → C2 → [Batch D → E] → F)
Ambil wilayah [OPEN] pertama, ubah menjadi [ACTIVE], jalankan tahapan berikut
khusus pada kontrak/modul di Wilayah R_i:

- **STAGE-A** (Region Value & Facts): `--context-packet` → @SLICE:STAGE-A.
  Petakan L-VALUE (Current Balance + Intended Capacity / Latent Value) di
  `path-map.md`; ekstrak fakta verbatim (Path/File.ext:Lxx-Lyy) ke FACTS.md
  lengkap dengan kolom Prerequisite Inversion Hint. Kunci `--gate A`.
- **STAGE-B** (Region Path & 3-Edge): `--context-packet` → @SLICE:STAGE-B.
  Petakan jalur fungsi/instruksi di `path-map.md` dengan tiga label wajib
  ([EDGE-ASSET], [EDGE-VALUATION], [EDGE-CONTROL/LATENT]) + titik potong
  lintas-wilayah. Kunci `--gate B`.
- **STAGE-C1** (Gelombang 1 — Blind Adversarial Hunt): `--context-packet` →
  @SLICE:STAGE-C1. Aturan Besi C1: DILARANG membuka _ROUTER.md atau file
  plugin M-xxx/AF-xx. Jalur aman → `04-code-guarded.md` (berjangkar);
  anomali awal → observasi [WAVE-1-BLIND] di `05-raw-leads.md`; sentuhan
  state lintas-modul → sinyal RADAR.md `[TOUCHES: ...]`. Kunci `--gate C1`.
- **STAGE-C2** (Gelombang 2 — Cross-Pollination & Antrean [QUEUED]):
  `--context-packet` → @SLICE:STAGE-C2 (menyuntikkan fakta R_i, sinyal
  [TOUCHES], dan indeks _ROUTER.md). Tiga operasi penyilangan: (1) Inversi
  FACTS.md, (2) Rantai RADAR.md, (3) Lazy-Load Bank Ilmu via `_ROUTER.md`
  (baca hanya file plugin yang Trigger Symptom-nya cocok). Saringan kualitas
  Segitiga → seluruh kandidat lolos masuk tabel `[QUEUED]` di
  `05-raw-leads.md` (tanpa plafon total, urut EV, masing-masing berpasangan
  Assigned M-xxx + Assigned AF-xx). Sebelum menulis entri `04`, WAJIB baca
  `references/methods/M-005-guard-inversion.md` §3 (5 Jalur Permissionless);
  setiap GUARD `TERSANGKAL` wajib tabel Uji 5 Jalur tepat 5 baris di §3
  `04-code-guarded.md`. Kunci `--gate C2`.
- **Siklus Rolling Batch STAGE-D (Prove) & STAGE-E (Verdict)** di Wilayah R_i:
  Selama masih ada [QUEUED] untuk R_i:
  1. Tarik Batch: maksimal 3 kandidat [QUEUED] EV tertinggi wilayah R_i ke
     blok `[ACTIVE-BATCH]` (status [ACTIVE]), satu `Batch ID: <R_i>-B<nn>`
     (contoh `R1-B01`; nomor di-reset ke B01 saat wilayah berganti; batch
     terakhir boleh 1–2 kartu). **Satu batch = satu wilayah; dilarang pindah
     wilayah selama [QUEUED] R_i > 0.**
  2. STAGE-D: sebelum menulis PoC, tetapkan kondisi live objek yang diserang
     — untuk SETIAP kartu [ACTIVE] jalankan `deploy_parity.py` (satu perintah
     per lead, artefak `notes/deploy-parity-<LEAD-ID>.json`), lalu isi di
     kartu: `TARGET_OBJEK`, `DEPLOY_REPORT`, `SIKLUS`, `VERDICT_ENTRYPOINT`,
     `MODE_UJI_POC`, dan baris `Fork PoC` (chain + block + RPC).
     `MODE_UJI_POC` diturunkan dari `SIKLUS` + `INTEL.md §0`, bukan dipilih
     bebas: MAINNET + P4/P3 → `FORK_LIVE` + blok; TESTNET/DEVNET + P2 →
     `FORK_TESTNET` + blok; P1 (pre-deploy) → `FORK_DEPLOYED_HEAD`/`LOKAL`.
     Bangun Bukti Segitiga per kandidat (Titik 1 Jejak Kode Eksak; Titik 2
     Bukti Eksekusi / Matematika Konkret; Titik 3 Delta Nilai & Invariant
     Pecah), kunci `--gate D` (menolak kombinasi deploy/mode yang tidak
     konsisten).
  3. STAGE-E: adili seluruh [ACTIVE] ke 06/07/08/09 sesuai hukum acara
     `templates/VERDICT.md` (lihat Bagian 7) — rekonsiliasi deployment
     WAJIB sebelum memilih wadah. Kunci `--gate E` (memastikan [ACTIVE]
     kembali 0, seluruh aturan wadah 06/07/08/09 terpenuhi, dan jejak lead
     #11d lengkap). Ulangi D → E sampai [QUEUED] == 0 DAN [ACTIVE] == 0,
     baru lanjut ke STAGE-F.
- **STAGE-F** (Incremental Disclosure & Penutupan Wilayah R_i):
  `--context-packet` → @SLICE:STAGE-F. Selagi konteks R_i 100% segar,
  lengkapi setiap temuan R_i di `09-confirmed-findings.md` menjadi format
  Submission-Ready Disclosure (DRAF — pengiriman diatur Bagian 8, bukan di
  sini). Jalankan `--close-region <R_i> <audit-dir>`; ubah status R_i
  menjadi [CLOSED] di STATE.md; lakukan Hard Context Flush; ambil wilayah
  [OPEN] berikutnya mulai dari STAGE-A.

### Fase III — Penutupan Global: Cross-Region Final Sweep (1x Setelah Semua Wilayah [CLOSED])
1. `--context-packet` saat seluruh wilayah [CLOSED] → @SLICE:FINAL-SWEEP.
2. Periksa seluruh entri RADAR.md: apakah ada sinyal [TOUCHES: ...] dari
   wilayah akhir (R_N) yang mematahkan asumsi di wilayah awal (R_1)?
3. Jika ada rantai lintas-wilayah baru yang belum diuji, masukkan ke
   `05-raw-leads.md` (maksimal 3 per batch) → jalankan D → E → F.
4. Jalankan gerbang pengunci akhir: `--final <audit-dir>`.

## 4. Arsitektur Modular *Drop-in Plugin* (1 File = 1 Item) & _ROUTER.md

Pengetahuan metode dan serangan disimpan secara modular agar mudah
ditambah/direvisi tanpa mengedit banyak berkas dan tanpa membebani memori agen:

1. Lokasi Lima Folder Plugin (di salinan referensi):
   - Metode Bedah: `references/methods/M-xxx-<slug>.md` (Indeks:
     `references/methods/_ROUTER.md`) — M-001 s/d M-010, **katalog diganti
     total pada sinkronisasi kedua** (nama & isi baru: M-001 backward
     sink-to-source tracing, M-002 state-transition differential, M-003
     multi-identity sybil zero-time loop, M-004 patch-scar view-write
     asymmetry, M-005 guard inversion, M-006 stale snapshot aliasing,
     M-007 metamorphic split-vs-bulk, M-008 caller-callee seam, M-009
     suspended lifecycle state, M-010 live residual state post-upgrade drift).
     Katalog lama diarsip upstream di
     `notes/backups/20261110-114745-plugin-M-v2-lama/` — **ID M-xxx lama
     TIDAK lagi menunjuk metode yang sama** (mis. M-001 lama = invariant
     value conservation; M-001 baru = backward sink-to-source tracing).
     Rujukan M-xxx pada proyek lama (Across R_1) wajib dibaca dengan
     katalog yang berlaku saat wilayah itu dikerjakan.
   - Bentuk Serangan Uji: `methodology/attack-forms/AF-xx-<slug>.md` (Indeks:
     `methodology/attack-forms/_ROUTER.md`) — AF-01 s/d AF-10.
   - Pengetahuan Domain: `methodology/domains/DOM-<slug>.md` (Indeks:
     `methodology/domains/_ROUTER.md`) — 7 DOM.
   - Sudut Pandang Aktor: `references/pov/POV-<slug>.md` (Indeks:
     `references/pov/_ROUTER.md`) — 15 POV (5 aktor lama + 10 topik baru:
     CALL-01/02, DEFI-01/02, EVM-01, MATH-01/02, MOVE-01, SOL-01, TON-01).
   - Pola Serangan Berantai: `references/chains/CHAIN-<slug>.md` (Indeks:
     `references/chains/_ROUTER.md`) — 10 CHAIN (5 pola rantai lama + 4
     per-rantai EVM-Tron/Solana/Sui-Move/TON-async + 1 detektor
     CHAIN-detector).
2. Format Header 4-Baris Wajib pada Setiap File Plugin (baris 1–4):

       # [ID-PLUGIN] Judul Metode / Bentuk Serangan / Domain / POV / Chain
       - Classes: [Kategori kelas masalah atau domain terkait]
       - Trigger Symptom: [Gejala kode spesifik yang memicu pembacaan file ini]
       - Paired Items: [ID plugin pasangan yang direkomendasikan, misal AF-xx, M-xxx]

3. Sinkronisasi Otomatis (`--sync`) & Aturan *Lazy-Loading*:
   - Setiap kali ada penambahan atau revisi file plugin, cukup buat/ubah
     1 file tersebut lalu jalankan `--sync` — DENGAN prosedur Muse di
     Bagian 1 butir 4 (hanya di salinan kerja proyek, dengan
     `HERMES_HOME` scratch; tidak pernah terhadap `theadvers-source`).
   - Agen dilarang membaca seluruh file plugin di dalam folder sekaligus.
     Agen wajib membaca `_ROUTER.md` terlebih dahulu di Gelombang 2 (C2),
     lalu membaca langsung (*direct file read* — padanan Muse untuk
     `read_file` di upstream) hanya pada file plugin yang sesuai dengan
     gejala wilayah aktif.

## 5. Empat Sabuk Pengaman Anti-Kompresi Konteks (*Zero-Compression Protocol*)

1. **Kewajiban Jangkar Verbatim (`Verbatim Anchor`):** setiap baris fakta di
   FACTS.md, sinyal di RADAR.md, dan entri di 04 s/d 09 wajib memuat format:
   `[Path/File.ext:Lxx-Lyy] | [NamaFungsi/Variabel Eksak] |
   [Kondisi/Ekspresi Kode Asli]`.
2. **Larangan Mutlak Token Pemotongan Malas:** `verify_segitiga.py` akan
   otomatis menggagalkan Gate jika menemukan token kompresi malas berikut di
   dalam blok analisis teknis: ..., dll., dst., etc., and so on, rest of
   logic, rest of code, dan seterusnya, atau klaim teknis tanpa rujukan
   nomor baris (:L).
3. **Kewajiban *Write-Ahead Checkpoint* di `STATE.md`:** jangan pernah
   berpindah tahap atau memanggil Subagent sebelum STATE.md diperbarui di disk.
4. **Protokol Delegasi Subagent & Isolasi Berkas Staging** (pemetaan Muse
   untuk `delegate_task` upstream — klaim "konteks 0-token" TIDAK berlaku di
   Muse karena subagent mewarisi konteks induk):
   - Injeksi Konteks oleh Parent: Parent wajib menjalankan
     `--context-packet <audit-dir>` dan menyertakan outputnya secara utuh di
     dalam brief Subagent (brief berdiri sendiri — subagent tidak boleh
     bergantung pada konteks percakapan induk).
   - Satu Subagent = tepat 1 wilayah/tahap/batch; delegasi berjalan
     sekuensial (satu anak aktif dalam satu waktu untuk satu antrean kerja);
     subagent TIDAK menulis berkas bersama.
   - Larangan Tulis Langsung oleh Subagent: Subagent dilarang keras mengedit
     langsung STATE.md, FACTS.md, RADAR.md, path-map.md, maupun 04 s/d 09.
   - Kewajiban Tulis ke Staging dengan Manifes ≤ 16 Baris: Subagent wajib
     menulis hasilnya ke `notes/staging-<region>-<stage>-<item>.md`. Bagian
     paling atas file staging wajib diawali blok `## STAGING MANIFEST`
     maksimal 16 baris (batas v2 — naik dari 12 baris di v6) yang merangkum
     daftar item verbatim yang dihasilkan, diikuti detail lengkap di bawahnya.
   - Verifikasi & Merge oleh Parent: Parent membaca manifes dan isi file
     staging, memastikannya bebas dari token kompresi malas dan memiliki
     jangkar Path/File.ext:Lxx-Lyy, menggabungkannya ke berkas utama, lalu
     menjalankan `--gate <stage>`. Vonis adalah wewenang Parent (Hakim);
     subagent tidak menetapkan verdict dan tidak menulis antrean bersama.
   - Larangan simpan data proyek ke memori Muse: data proyek hanya tinggal
     di folder proyek (`projects/<nama>/`) dan salinan referensi — tidak
     pernah di memori jangka panjang Muse atau skill global.

## 5b. Aturan Bukti Per Wadah 06 / 07 / 08 / 09 (Mengikat, Ditegakkan `--rules` & `--gate E`)

Setiap wadah menuntut **jenis bukti yang berbeda**. Wadah yang diisi tanpa
bukti jenisnya = laporan DITOLAK. Hukum acara lengkap: `templates/VERDICT.md`
(§4–§9).

| Wadah | Menuntut bukti | Gerbang penolak |
| :--- | :--- | :--- |
| `04-code-guarded.md` | Bukti **penutupan jalur**: status `TERSANGKAL` hanya sah bila tabel §3 memuat **tepat 5 baris** (J1–J5) berisi `TERTUTUP`/`TEMBUS` + alasan + jangkar `Path/File.ext:Lxx-Lyy` | `RULES-04` (guard TEMBUS yang tetap di `04`, atau jalur yang belum diuji) |
| `06-refuted-invalid.md` | Bukti **kegagalan**: kelas `REJECTED_*`, `TANGGA_BUKTI`, `DEPLOYMENT_MATCH`, jangkar `:Lxx`; kelas `POC_FAILS`/`NEGATIVE_EV`/`DUST` wajib `BUKTI_SIMULASI:` berisi `$ ...` + log + Strike 3 Fuzz | `PAPER_REFUTATION_DETECTED` (kecuali KS-1..KS-4) |
| `07-needs-next-step.md` | Bukti **usaha**: `Disposition:` (tepat satu) + `PERCOBAAN:` berisi perintah + log nyata; `TANGGA_BUKTI` A/A-HEAD dilarang | `LAZY_BAILOUT_DETECTED` |
| `08-valid-not-eligible.md` | Bukti **mekanisme**: `LOW_CLASS` (PRE_LIVE_SCOPE / ADMIN_ONLY / LOW_NUISANCE) + `POC_STATUS: PASS` + dampak kuantitatif | Pelarian wadah (#11b) |
| `09-confirmed-findings.md` | Bukti **ketiganya**: 3 Titik + Sepuluh Pernyataan + Integritas #11 + `LIVE_IDENTITY` terukur + `POC_STATUS: PASS`; `A-HEAD` dan siklus pre-deploy dilarang | ditolak ke `08`/`06` |

**Jejak lead (#11d).** Setiap lead yang keluar dari `05-raw-leads.md` wajib
muncul di **tepat satu** wadah `06`/`07`/`08`/`09`. Lead yang menguap atau
tercatat ganda → `--gate E` FAIL.

**Target akhir `07` = 0 entri.** Setiap sisa wajib dipertahankan dengan log
komputasi, bukan kalimat.

**Kelas `06` yang sah (12):** `REJECTED_POC_FAILS`, `REJECTED_NEGATIVE_EV`,
`REJECTED_DUST`, `REJECTED_NO_VICTIM`, `REJECTED_FANTASY_MOCK`,
`REJECTED_CHEATCODE`, `REJECTED_PRIVILEGED_FANTASY`,
`REJECTED_GENESIS_MIRAGE`, `REJECTED_NON_LIVE_INIT`, `REJECTED_BY_DESIGN`,
`REJECTED_MISMATCH`, `REJECTED_UNPROVEN_DEPLOYMENT`. Kelas di luar daftar
ini (mis. `REJECTED_FILLED_LIVE` yang dipakai proyek Across R_1) DITOLAK
gate — bila Kim memerintahkan proyek lama dinilai ulang di bawah aturan
baru, kelasnya wajib dipetakan ulang ke salah satu dari 12 ini.

## 6. Tabel Ringkasan Gerbang Penegak (tools/verify_segitiga.py)

`<ref>` = `~/workspace/theadvers-source/tools/verify_segitiga.py`.
Jalankan dari direktori kerja proyek; `<audit-dir>` adalah folder audit
proyek (berisi STATE.md dkk.).

| Perintah CLI | Kapan Wajib Dijalankan | Apa yang Diverifikasi oleh Skrip |
| :--- | :--- | :--- |
| `python3 <ref> --sync` (prosedur Muse, Bagian 1 butir 4) | Saat menambah/mengubah file plugin M-xxx, AF-xx, DOM-xx, POV-xx, CHAIN-xx. | Memvalidasi 4 baris header plugin dan membangun ulang seluruh tabel _ROUTER.md. |
| `python3 <ref> --context-packet <dir>` | Wajib di awal setiap tahap, setiap batch, dan setiap pergantian wilayah. | Mencetak status STATE.md, potongan @SLICE:STAGE-* dari METHODOLOGY-SEGITIGA.md, fakta R_i, radar R_i, dan router yang relevan. |
| `python3 <ref> --intel <dir>` | Setiap kali selesai mengisi satu bagian INTEL.md; wajib sebelum STAGE-0. | INTEL.md sah: Deployment Stage dinyatakan, repo URL langsung, commit terpin, Bukti Live konkret, Blok Referensi (MAINNET), konsistensi Intel Status vs §6. |
| `python3 <ref> --rules <dir>` | Sebelum/di luar gate, khusus memeriksa wadah. | Aturan 04 (`TERSANGKAL` wajib 5 baris jalur J1–J5 berjangkar, `RULES-04`), 06 (`REJECTED_*` + `BUKTI_SIMULASI`/KS-n), 07 (`Disposition` + `PERCOBAAN`), 08 (`LOW_CLASS` + `POC_STATUS: PASS` + `DEPLOYMENT_REPORT`), 09 (`TANGGA_BUKTI`/`LIVE_IDENTITY`/`POC_STATUS` + fork), dan jejak lead (#11d). |
| `python3 <ref> --gate 0 <dir>` | Selesai STAGE-0 (Global Partitioning). | **INTEL.md sah** (prasyarat) + 100% berkas kode sumber dalam scope terdaftar di Tier 1/2/3 pada path-map.md, Tier 3 terkunci 1-pass, antrean R_1..R_N terbentuk di STATE.md, dan Zero-Loss (setiap berkas Tier-1/2 dialokasikan ke ≥1 wilayah). |
| `python3 <ref> --gate A <dir>` | Selesai STAGE-A pada Wilayah R_i. | Kontrak/modul R_i memiliki Current Balance + Intended Capacity dan fakta verbatim (:Lxx) + inversi di FACTS.md. |
| `python3 <ref> --gate B <dir>` | Selesai STAGE-B pada Wilayah R_i. | Wilayah R_i memiliki pemetaan [EDGE-ASSET], [EDGE-VALUATION], dan [EDGE-CONTROL/LATENT] di path-map.md. |
| `python3 <ref> --gate C1 <dir>` | Selesai STAGE-C Gelombang 1 (Blind Hunt) di R_i. | Jalur terjaga masuk ke 04-code-guarded.md, observasi awal masuk ke 05-raw-leads.md, dan sinyal lintas-kontrak tercatat dengan [TOUCHES: ...] di RADAR.md. |
| `python3 <ref> --gate C2 <dir>` | Selesai STAGE-C Gelombang 2 (Cross-Pollination) di R_i. | Hasil penyilangan tercatat, kandidat gugur awal masuk ke 04-code-guarded.md, dan setiap kandidat [QUEUED] di 05-raw-leads.md memiliki pasangan valid M-xxx & AF-xx serta urutan EV. |
| `python3 <ref> --gate D <dir>` | Selesai STAGE-D pada satu Batch di R_i. | Kandidat [ACTIVE] berjumlah 1..3, semuanya satu `Batch ID` berprefiks wilayah aktif (`R1-B01`), semuanya memiliki 3 komponen Bukti Segitiga lengkap tanpa token kompresi, **dan** per lead ada `TARGET_OBJEK` + artefak `deploy_parity` + `MODE_UJI_POC` yang konsisten dengan `INTEL.md §0` + `SIKLUS`. |
| `python3 <ref> --gate E <dir>` | Selesai STAGE-E pada satu Batch di R_i. | Active Leads == 0, **plus** seluruh aturan wadah 06/07/08/09 (kelas + bukti, Bagian 5b) dan jejak lead: setiap lead yang keluar dari 05 wajib muncul di tepat satu wadah. |
| `python3 <ref> --close-region <R_i> <dir>` | Selesai STAGE-F pada Wilayah R_i. | Di 05-raw-leads.md baik [ACTIVE] == 0 maupun [QUEUED] == 0 untuk R_i, dan seluruh temuan R_i di 09-confirmed-findings.md telah memiliki blok *Submission-Ready Disclosure*. |
| `python3 <ref> --final <dir>` | Selesai seluruh wilayah & *Cross-Region Final Sweep*. | Seluruh wilayah R_1..R_N berstatus [CLOSED], 05-raw-leads.md bersih (0), bebas token kompresi, dan seluruh temuan di 09-confirmed-findings.md siap kirim (sebagai draf — lihat Bagian 8). |

Gerbang bersifat **monoton**: `GATE-C1` hanya sah bila `Last Passed Gate` di
`STATE.md` sudah mencapai C1 atau lebih tinggi. Setiap gate diuji dua arah —
PASS pada kondisi bersih, **FAIL** pada pelanggaran yang disuntikkan.

## 7. VERDICT — Pengadilan Bukti (STAGE-E & STAGE-F)

Standar acara: **`templates/VERDICT.md`** di salinan referensi — kini memuat
hukum acara lengkap (§1–§11): Syarat Mutlak Tiga Titik Bukti Segitiga,
TANGGA BUKTI (A / T-ONLY / A-HEAD / B / C / D), `VERSION_RELATION` (§2c),
rekonsiliasi deployment `deploy_parity.py` (§2d: 4 Siklus P1–P4, 7 Verdict
Entrypoint), Matriks Keputusan Wadah (§3), aturan masuk 06 (§4, KS-1..KS-4),
07 (§5), 08 (§6), 09 (§7), Sepuluh Pernyataan Keras (§8), Integritas #11
(§9), Aturan Anti-False-Negative (§10), Checklist Penolakan Otomatis (§11).
Ringkasan mengikatnya:

- Sebuah kandidat hanya sah disebut Temuan Terkonfirmasi (`09`) apabila
  ketiga titik Bukti Segitiga terbukti simultan dan tertulis tanpa pemotongan
  logika: (1) Jejak Kode Eksak & Keterjangkauan Jalur, (2) Bukti Eksekusi
  Konkret atau Jejak Matematika Eksak, (3) Delta Nilai Terukur & Kerusakan
  Invariant.
- Matriks wadah: jalur tertahan penjaga kode → 04; PoC gagal / hitungan
  membuktikan tidak ada dampak → 06; butuh data di luar repo lokal → 07;
  valid teknis tetapi di luar kelayakan klaim → 08; ketiga titik utuh +
  dalam *scope* + invariant pecah tanpa asumsi palsu → 09.
- **Pernyataan #11 (integritas laporan)** berlaku di tingkat laporan, bukan
  per temuan: #11b larangan pelarian wadah (07 → 08 hanya sah bila mekanisme
  sudah terbukti mekanis); #11c atestasi hampa tanpa `File:Baris` batal;
  #11d gate cakupan + jejak lead (setiap lead di tepat satu wadah);
  **#11e anti-paper-refutation** — setiap entri `06` dengan
  POC_FAILS/NEGATIVE_EV/DUST wajib membawa blok `BUKTI_SIMULASI:` termasuk
  Victim Seeding + fuzz Strike 3, bila tidak →
  `PAPER_REFUTATION_DETECTED` → laporan ditolak, kembali ke STAGE-D.
- **Fork LIVE wajib untuk proyek mainnet.** `09` wajib `Mode Uji: FORK_LIVE`
  + chain + block + RPC (FORK_DEPLOYED_HEAD/LOKAL tidak sah di `09` mainnet).
  `06`/`08`/`09` wajib `DEPLOYMENT_REPORT` menunjuk artefak `deploy_parity`
  yang benar-benar ada. `ENV_ERROR` tidak pernah menjadi bukti ketiadaan —
  RPC gagal 2x → `07` `TOOL_BLOCKED`, bukan `08`/`06`. Aturan ini konvergen
  dengan aturan fork-live Kim (2026-11-10 17:44 WIB): pengujian mengikuti
  kondisi live nyata, kecuali Kim menjelaskan di awal proyek masih
  testnet/devnet.
- **Rekonsiliasi Deployment (`deploy_parity.py`) WAJIB per lead di STAGE-D
  dan per entri 06/08/09 di STAGE-E/F** (bukan lagi hanya Langkah 0
  STAGE-F): satu perintah per kontrak yang diserang; `VERSION_RELATION`
  diukur alat, bukan ditebak; `bytecode identik` DILARANG dijadikan syarat
  (yang diuji keberadaan & perilaku entrypoint yang diserang PoC).
  Aturan Muse (Bagian 9): cek deployment read-only via endpoint publik,
  hanya dengan izin Kim per proyek; `--rpc` wajib dilewatkan eksplisit per
  pemanggilan; TIDAK ADA file `.env.rpc` yang disalin (tidak ada di tree v2);
  jalur default `~/.hermes/.../.env.rpc` di alat tersebut TIDAK diadopsi;
  tanpa kunci Alchemy di berkas/argumen/log.
- **`verify/VERDICT.md` TIDAK dipakai di profil ini** (keputusan induk
  CP-016, 2026-11-10 — berkas itu tidak lolos pemeriksaan keamanan runtime;
  hukum acaranya kini tercakup penuh di `templates/VERDICT.md` yang baru).
  Bila ada yang merujuk `verify/VERDICT.md` sebagai rujukan gate, abaikan
  rujukan itu dan pakai `templates/VERDICT.md`.

## 8. Submisi — Draf Siap-Kirim, Bukan Pengiriman

STAGE-F menulis blok *Submission-Ready Disclosure* (DRAF) per temuan di
`09-confirmed-findings.md` dan paket kasus di `cases/<FIND-ID>/`
(`chat.md`, `laporan-teknis.md`, `poc.md`), dengan standar isi di
`templates/POC-PACKAGE.md`, `templates/TECHNICAL-REPORT.md`,
`templates/SUBMISSION-EMAIL.md`, dan `templates/SEVERITY-RUBRIC.md`.
Gerbang mekanis pra-kirim: `verify/ingest.py` memeriksa MANIFEST.json
(schema, pin commit sumber, keberadaan berkas, validitas rentang baris,
keberadaan PoC, perintah uji, duplikasi id) dan menulis `verify/queue.json`
hanya untuk klaim `MECH-OK`. Catatan Muse untuk `ingest.py`: `--dest` wajib
dilewatkan eksplisit setiap kali (dest default upstream menunjuk
`~/.hermes/.../cache/inbox` — TIDAK diadopsi); alat ini meng-clone/pull repo
dari sumber yang dilewatkan — jalankan hanya untuk sumber yang Kim setujui.

Yang mengikat di profil ini, tanpa kecuali:

- **Tersimpan ≠ tersubmit.** `MECH-OK`, status `[SUBMISSION-READY]`, dan
  blok Disclosure yang lengkap adalah kesiapan DRAF — bukan persetujuan
  kirim.
- **Tanpa persetujuan eksplisit Kim, dilarang:** mengirim email submisi,
  mengisi formulir platform (Immunefi / Cantina / Sherlock / dsb.),
  menghubungi program/tim proyek, atau mempublikasikan isi laporan.
  Persetujuan harus spesifik per pengiriman (program + temuan + kanal);
  persetujuan atas satu pengiriman tidak berlaku untuk pengiriman lain.
- Setelah terkirim (bila sudah disetujui), perbarui `MANIFEST.json`
  (`upstream.submitted_to_program: true`, tanggal kirim, ID tiket) agar
  temuan yang sama tidak dikirim ulang.
- Template email upstream (`SUBMISSION-EMAIL.md`) tetap berlaku sebagai
  FORMAT — gerbang "kirim hanya setelah N/N MECH-OK" dibaca sebagai gerbang
  kesiapan draf, bukan sebagai perintah kirim.

## 9. RPC, Tingkat Pengujian & Batas Keamanan Eksekusi

- **Pengujian berjenjang:** lokal dulu; testnet hanya dengan akun throwaway
  bila bukti eksekusi memang tidak bisa didapat dari lokal; mainnet hanya
  pilihan terakhir dengan persetujuan eksplisit per aksi. Tanpa persetujuan
  eksplisit: dilarang menyiarkan transaksi keluar (`--broadcast`), mengubah
  state mainnet/testnet publik, atau memakai private key berisi dana nyata.
- **RPC:** hanya endpoint publik untuk fork lokal read-only, dan hanya
  setelah izin Kim per proyek. Tanpa berkas secret (`.env`, `.env.rpc`,
  `rpc-env.sh` tidak disalin — ketiganya tidak ada di tree v2 upstream).
  Kunci Alchemy mentah di URL/berkas test/argumen/log dilarang. Pacing wajar
  pemakaian biasa; berhenti pada 429 / rate-limit / blokir akun dan laporkan.
- **`deploy_parity.py`:** jalankan hanya dengan `--rpc <endpoint-publik>`
  eksplisit per pemanggilan (dan `--paradigm`/`--chain`/`--addr`/
  `--entrypoints` yang Kim setujui untuk proyek itu). Jangan mengandalkan
  `load_env_rpc()` — kandidat path-nya (termasuk `~/.hermes/...`) TIDAK
  diadopsi; tidak ada `.env.rpc` yang dibuat di lingkungan Muse.
  `--selftest` alat ini butuh `cast` (Foundry) di PATH — tanpa itu kasus EVM
  mengembalikan `UNVERIFIABLE` (bukan bukti ketiadaan); di mesin ini
  `cast`/`forge` ada di `~/.foundry/bin/` (bukan PATH default).
- **Kode target read-only 100%:** dilarang mengedit kode target; PoC hanya
  di fork lokal / harness proyek. `vm.store`, `vm.etch` state protokol, dan
  `vm.prank(admin)` untuk mengarang state adalah pelanggaran
  (`REJECTED_CHEATCODE`) — lihat standar PoC di `templates/case/poc.md`
  (kontrol positif wajib, victim seeding lewat transaksi nyata, tanpa
  hardcode hasil, kode utuh 100%).
- **Kejujuran bukti:** pisahkan tegas yang benar-benar dieksekusi vs
  penalaran/code-review; sumber terblokir bukan bukti negatif; tidak
  teramati = tidak diketahui; klaim nol/empty dicurigai sampai dihitung ulang
  dengan kontrol positif; jendela sampel bukan sensus.

## 10. Yang TIDAK Diadopsi dari Upstream (dicatat agar tidak diterapkan diam-diam)

1. Instalasi skill ke profil Hermes (`verify_segitiga.py --sync` langkah b,
   `sync_skills_to_hermes_profile()`; `HERMES_HOME` / `~/.hermes/skills`) —
   Muse membaca skill langsung dari `~/workspace/theadvers-source/skills/`.
   Prosedur `--sync` Muse ada di Bagian 1 butir 4 + `CATATAN-MUSE.md`.
2. Jalur default `~/.hermes/profiles/adv/workspace/.env.rpc`
   (`deploy_parity.py load_env_rpc()`) dan dest default
   `~/.hermes/profiles/adv/cache/inbox` (`ingest.py`) — keduanya TIDAK ADA
   di mesin ini; argumen eksplisit wajib dilewatkan.
3. `read_file` / `delegate_task` sebagai nama tool — padanan Muse: baca file
   langsung; delegasi subagent Muse sekuensial dengan brief berdiri sendiri
   (Bagian 5 butir 4).
4. Penyebutan `delegate_task` di `README.md` upstream (baris 38) — dibaca
   sebagai delegasi subagent Muse.
5. `config.yaml`, `.env.rpc`, `rpc-env.sh`, `RPC-STATUS.md`, `sync.sh` —
   tidak ada di tree v2 upstream, tidak dibuat di sini.
6. Pengiriman submisi sebagai bagian dari STAGE-F — STAGE-F di profil ini
   berhenti di draf siap-kirim (Bagian 8).
7. Referensi `sc-phase3 Langkah 2c` di `templates/case/poc.md` upstream —
   penamaan warisan v1 yang tertinggal di template v2; dibaca sebagai
   "langkah pembuktian STAGE-D" (dicatat sebagai inkonsistensi upstream di
   `SYNC-2026-11-10.md`, tidak diubah di salinan referensi).
8. Dua varian `SEVERITY-RUBRIC.md` upstream (`templates/` vs `verify/`,
   isi berbeda) — keduanya disimpan apa adanya di salinan referensi; bila
   bertentangan, varian `verify/` yang dipakai sebagai rujukan gate,
   varian `templates/` sebagai format laporan. Jangan digabung diam-diam.
9. **`verify/VERDICT.md` sebagai rujukan** — TIDAK dipakai di profil ini
   (CP-016, 2026-11-10); hukum acara lengkap kini ada di
   `templates/VERDICT.md` yang baru (lihat Bagian 7). Bila skill/template
   upstream masih merujuk `verify/VERDICT.md`, baca rujukan itu sebagai
   `templates/VERDICT.md`.
10. `INTEL.md` template upstream yang baris contoh §0-nya ("Deployment Stage
    : MAINNET | TESTNET | DEVNET") dibaca alat sebagai stage MAINNET bila
    dibiarkan tak terisi — INTEL wajib diisi nyata (bukan dibiarkan contoh)
    sebelum `--intel`/`--gate 0` dianggap sah. (Kelemahan alat upstream,
    dicatat di SYNC-2026-11-10-2.md §7.)

## 11. Konflik & Hirarki

- Bila konstitusi ini berbeda dengan ringkasan di skill tahap, konstitusi ini
  yang menang untuk hal-hal yang diatur di sini (satu penulis per file,
  staging, STATE.md induk, gerbang, submisi).
- Bila skill tahap berbeda dengan `methodology/METHODOLOGY-SEGITIGA.md`
  (@SLICE), @SLICE yang menang untuk prosedur tahap tersebut.
- Bila `templates/VERDICT.md` berbeda dengan `verify/VERDICT.md`,
  `templates/VERDICT.md` yang menang (`verify/VERDICT.md` tidak dipakai —
  lihat Bagian 7).
- Instruksi Kim yang spesifik untuk satu tugas menang atas konstitusi ini,
  kecuali batas keamanan Bagian 1, 8, dan 9 — batas itu hanya bisa diubah
  dengan izin tertulis dan spesifik dari Kim untuk tugas tersebut.
