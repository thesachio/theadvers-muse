# Resume Aturan & Preferensi Alur-First

Status: resume ringkas aturan mengikat metode alur-first, 15 Nov 2026.
Revisi 15 Nov 2026 ~19:05 WIB (perintah Kim "benahi yang terkait pada
fase A B C"): Tahap C selesai, B16 dilaksanakan di pilot (status: TIDAK DIPAKAI sejak 22:33 WIB 16 Nov 2026), inventory ramping dinilai berisiko (tidak diadopsi), K9
dilewati, SM-05 + daftar alur + bentuk Tahap D masih terbuka.
Revisi 16 Nov 2026 22:33 WIB: 10 butir diputus Kim (lihat §10) — B16 TIDAK DIPAKAI; Tahap F diterima bersyarat; Tahap D per-alur + dua gelombang; Tahap E/G bentuk rinci = yang dipraktikkan; B9 rerun diterima; A5 dua celah diterima; arah metode = pakai paling mutakhir; deep-run per jalur; larangan filter ditulis; SM-05 dieksplorasi ketika terkait.
Revisi 17 Nov 2026 (perintah Kim): C10 — definisi kerugian = yang bisa dibuktikan (mekanisme jalan di fork/test atas kode+state nyata + plafon terukur + pemicu terjangkau operasi normal tanpa peran istimewa/state eksotis; TIDAK harus live) — gate D: plafon live 0 tidak lagi membuat lead DORMAN; verdict 09 tidak lagi mensyaratkan P&L live > 0.
Revisi 17 Nov 2026 (perintah Kim 12:14 WIB): C11 — standar bukti TERBUKTI = ada CUKUP jalur pendek yang saling bergantung (1 atau beberapa) dari pelanggaran invariant sampai ke kerugian yang terukur, lewat verifikasi alur-alur TERKAIT; tes yang berhenti di satu fungsi pendek lalu menyimpulkan = masih HIPOTESIS — kartu E yang buktinya cuma satu fungsi pendek harus dinilai ulang; Gelombang 1/2 tidak memberi verdict TERBUKTI.
Yang mengikat = berkas aturan kanonik `aturan-alur-first.md`; bila resume
ini berbeda dengan berkas aturan, berkas aturan yang berlaku. Acuan
workflow Tahap B: `acuan-workflow-tahap-B.md`. Draf usulan awal:
`usulan-alur-first.md` (bukan rule).

## 1. Tujuan metode

- Asal: code-first hanya fokus ke dugaan invariant yang tidak pas, akhirnya
  tidak ada jalur ke finansial/protokol. Segitiga memperbaiki itu tapi
  tiering banyak masalah.
- Metode baru harus: (a) mengarah ke efek finansial, (b) menjamin alur
  benar-benar tercover seluruhnya. Dan meminimalisir efek berantai:
  wilayah salah → peta nilai salah → buruan salah → bukti salah.
- Unit kerja = alur (jaliran uang ujung-ke-ujung), bukan berkas/wilayah.
  Wilayah/klaster = pembagian kerja, bukan kebenaran.

## 2. Struktur tahap A–G

- A INTEL, B Mapping, C Pemetaan mendalam per klaster, D Eksplorasi,
  E Uji test, F Komposisi (USULAN), G Verdict.
- Output tiap tahap: `<huruf><NN>-<NAMA>` (mis. B01-SCOPE,
  C01-K1-FLOW-INVENTORY).
- Rolling: C → D → E. Tahap C jalan per klaster (selesai 18:48 WIB);
  unit round Tahap D belum diputus (usulan asisten: per-alur — lihat
  §11). F lintas-klaster + G di akhir.
- Just-in-time lintas-klaster berlaku di Tahap D (eksplorasi): saat memburu
  satu alur, pemburu mengikuti alur itu melewati klaster. Di Tahap C, kerja
  per klaster; jahitan adalah batas.

## 3. Rule Tahap A — INTEL (A1–A5)

- A1: INTEL = brief 9 butir (nama; deskripsi lingkup; stage; repo; website;
  docs; audit; bugbounty; brief severity).
- A2: pre-0 = daftar docs pendukung saja, bukan verifikasi teknis.
- A3: stage (MAINNET/TESTNET/DEVNET) = filter pertama penentu Mode Uji
  (FORK LIVE_MATCH); HEAD bukan patokan kebenaran.
- A4: pencarian mulai dari nama proyek saja; tangga sumber berurutan, maks
  3 cek per butir; status DITEMUKAN / TIDAK ADA / TIDAK KETEMU (bukan bukti
  tidak ada); stage + repo = penahan beban (salah satu TIDAK KETEMU →
  berhenti, eskalasi); anggaran maks 30 menit; INTEL COMPLETE hanya bila
  semua DITEMUKAN (atau TIDAK ADA bersumber).
- A5: INTEL dikunci pin (commit + blok referensi + stage); kanonik
  read-only; koreksi dicatat eksplisit. Celah terbuka: commit mana dipin
  di run dari-0 + blok referensi belum dicakup tangga A4 (Kim menunda).

## 4. Rule Tahap B — Scope Mapping (B1–B16)

- B1: sensus = 100% peristiwa pemindah nilai (entrypoint + transfer
  langsung/donasi/force-feed + pergerakan level-sistem + multicall/dispatch
  dinamis transitif + proxy→implementation + callback/hook + transfer
  SOL/SPL ke PDA + langkah off-chain dilabeli asumsi). Fungsi = unit
  sensus; segmen = unit buruan.
- B2: scope menyeluruh di awal; pemetaan + eksplorasi round per round per
  klaster diurutkan EV live; rolling C → D → E. Urutan EV = default, bukan
  gerbang.
- B3: TETAP UTUH — sensus (statis + live), daftar scope, inventaris alamat
  live, cek penutupan graf akhir, registry invariant (satu penomoran, diisi
  mulai Tahap D), aturan/gerbang.
- B4: BISA DIPECAH per klaster — inventaris alur + segmentasi, kontrak
  handoff, penulisan invariant, uji invariant, komposisi, verdict.
- B5: peta utuh, bukti just-in-time — mapping awal menyeluruh; kerja
  spesifik klaster/fase tidak dijalankan di scope besar.
- B6: sensus statis = baca manual penuh semua fungsi/semua berkas +
  verifikasi parent + kontrol positif; TIDAK pakai AST (ditolak Kim); B9
  tidak berlaku untuk kedalaman sensus statis.
- B7: cek rantai utama, ringan: getCode 1–3 alamat inti di blok referensi +
  satu view murah; fallback rantai utama kedua; rantai tidak merespons →
  langsung pindah; anggaran maks 5 menit (target, belum terukur).
- B8: cek live-match diulang di awal fase uji per klaster sebelum PoC;
  tidak live-match → TUNDA-LIVE (tercatat di ledger, nol waktu eksekusi,
  dibuka lagi bila deployment live muncul atau Kim memerintah cek ulang,
  ditinjau di sapuan akhir). Utamakan alur, log untuk verifikator terakhir
  (diputus 17 Nov 2026 08:26 WIB, perintah Kim, MENGIKAT untuk semua B8
  berikut): mekanisme dibuktikan dari tes tool (baca kode + cast/cek live +
  uji fork di Tahap E), BUKAN dari pindai log tx orang lain — pindai log
  hanya untuk verifikator akhir (Tahap G / sapuan akhir). Pemicu: B8 P4
  ~8,5 jam memindai RelayedRootBundle per rantai. Cek desain kode dulu
  (diputus 17 Nov 2026 ~08:33 WIB, perintah Kim, MENGIKAT): nilai 0 atau
  bukan ditentukan dari desain kode (DISET 0 oleh kode atau BISA MEMUAT),
  bukan dari log — contoh P4: slowRelayRoot = input proposer :565,
  disimpan :579, diteruskan :685, tanpa require/validasi, jadi BISA
  MEMUAT non-zero by design; live 0 dari proposer/off-chain.
- B9: scope mapping usaha yang cukup + 4 mekanisme deteksi miss di
  eksplorasi (baca ulang mendalam per klaster; cek handoff + konservasi;
  jejak event live; cek penutupan graf) + sapuan akhir enumerasi independen
  kedua (caranya masih terbuka — bukan AST, bukan regex yang sama).
- B10: ledger sisa sesudah tiap round (entrypoint belum masuk alur, handoff
  terbuka, klaster belum dikerjakan); round selesai hanya bila sisa
  menyusut; target buruan harus ada di sensus.
- B11: handoff dienumerasi dari batas unit (rantai linear N unit → N−1
  handoff); simpul fork/join jadi unit sendiri.
- B12: invariant TIDAK dinyatakan di Tahap B atau C — dirumuskan di Tahap D
  bersama kartu lead, diuji di Tahap E. Tiap invariant di Tahap D WAJIB
  cantumkan sumber (field handoff atau docs/NatSpec). Tiga lapis: L1 segmen
  (INV-Sx), L2 handoff + simpul (INV-Hx, INV-Jx), L3 ujung-ke-ujung/
  komposisi (INV-Fx). Tiap invariant wajib akibat finansial bila pecah.
  Cek testability dicatat saat perumusan. Registry satu penomoran global,
  diisi mulai Tahap D.
- B13: scope lock sebelum sensus (daftar berkas cocok persis inventaris
  kanonik, diff kosong, commit terverifikasi, contracts/ bersih).
- B14: 7 konvensi sensus statis (getter publik = entrypoint ya; receive
  badan kosong = value_moves no; relayMessage adapter = yes + cek badan per
  adapter; dispatcher arbitrer = unknown; getter auto-generated dibariskan;
  entrypoint warisan tidak dibariskan + tahap alur perluas permukaan lewat
  implementasi; atribut berkas direct/proxy dua lapis).
- B15: B7 ringan WAJIB di Tahap B; cek onchain mendalam WAJIB di B8; Tahap A
  tidak cek onchain; sensus live penuh tetap opsional; pengecualian
  TESTNET/DEVNET: "live" = lingkungan itu.
- B16 TIDAK DIPAKAI (diputus Kim 22:33 WIB 16 Nov 2026 — 'B16 tidak perlu'; mekanisme di bawah ini arsip usulan, bukan rule):
  jahitan dienumerasi utuh di depan dari graf tepi keluar EP+VM; klaster
  diidentifikasi dari graf, bukan label tangan; daftar jahitan SM-xx
  (produsen, konsumen, bukti, status: dienumerasi → produsen terverifikasi
  → konsumen terverifikasi → TERTUTUP); jejak event live menambah jahitan
  berbasis event + mengonfirmasi tepi statis; lead/PoC yang melewati
  jahitan terbuka wajib melabelinya asumsi terbuka; urutan default diusulkan
  hulu→hilir (topologis), EV pemecah seri — bila diputus, menggantikan
  default EV di B2; primitif bersama = klaster bersama sendiri,
  diinventarisasi sekali, dirujuk semua konsumen. Catatan: graf Across
  BUKAN DAG (2 siklus) — urutan topologis total tidak ada. Dilaksanakan
  di pilot (pass jahitan + siklus hidup di Tahap C: 11/12 TERTUTUP, SM-05
  dienumerasi — konsumennya TokenMessenger eksternal, butuh keputusan
  Kim) — status rule: TIDAK DIPAKAI.

## 5. Tahap B — yang wajib selesai (7 butir)

1. Daftar scope + scope lock (B13).
2. Sensus statis 100% (B1/B6). (Field 'keluar' TIDAK wajib — B16 tidak dipakai.)
3. Inventaris alamat live — daftarnya saja (B3).
4. Sensus live ringan B7 (B15).
5. Identifikasi klaster + daftar handoff (B11). (Daftar jahitan SM-xx lengkap TIDAK wajib — B16 tidak dipakai.)
6. Ledger sisa — seed (B10).
7. Struktur registry invariant — kosong, diisi Tahap D (B3).

Bukan pekerjaan Tahap B: pemetaan mendalam per klaster (C),
invariant/buruan (D), PoC+P&L (E), komposisi (F), verdict (G), live-match
mendalam B8 (di giliran klaster), sensus live penuh (opsional), cek
penutupan B9 (dieksekusi di akhir; bahannya yang wajib ada di Tahap B).

## 6. Tahap B — yang TIDAK dilakukan (10 butir)

1. Inventaris mendalam semua klaster sekaligus (itu C per klaster).
2. Menulis invariant (itu D).
3. Buruan saat memetakan (itu D).
4. Eksekusi/verifikasi saat memetakan (itu E).
5. Sensus live penuh (itu B8 per klaster; Tahap B cukup B7 ≤5 mnt).
6. Baca mendalam semua fungsi saat sensus (hanya EP+VM + Tahap C).
7. Menyelesaikan semua unknown saat sensus (diselesaikan di Tahap C).
8. Menilai klaster saat memetakan (itu G).
9. Rerun sensus penuh sebagai syarat tetap (rerun = validasi pilot;
   enumerasi kedua = sapuan akhir B9).
10. Memoles artefak/laporan akhir.
11. Filter di DALAM klaster saat inventory (hanya data yang dibutuhkan
    alur live) — dinilai BERISIKO oleh Kim (15 Nov 2026 18:22 WIB),
    tidak diadopsi; filter yang aman hanya utuh per klaster/alur (K9
    dilewati utuh; K7 bila EV 0). Larangan ini ditulis ke berkas kanonik
    22:33 WIB 16 Nov 2026 atas perintah Kim.

## 7. Output tiap tahap

- A: A01-INTEL (brief 9 butir), A02-INTEL-discovery (catatan penemuan).
- B (depan, seluruh protokol): B01-SCOPE, B02-CENSUS-STATIC (+ staging
  JSON, + tepi keluar EP+VM), B03-CENSUS-LIVE (inventaris alamat + B7),
  B04-CLUSTERS (klaster + jahitan SM-xx), B05-LEDGER-SISA (seed),
  B06-REGISTRY-INVARIANT (struktur kosong). B07-AST-COUNTER = arsip
  validasi saja, bukan metode.
- C (per klaster just-in-time): C<NN>-<KLASTER>-FLOW-INVENTORY
  (inventaris alur + segmentasi + kontrak handoff deskriptif klaster);
  K8 primitif: C08-K8-PRIMITIVES (inventaris primitif, bukan alur);
  K9 (mock) dilewati.
- D: kartu lead + invariant (registry INV-Sx/Hx/Jx/Fx).
- E: PoC fork-live + P&L per kandidat.
- F (usulan): PoC gabungan (lemma komposisi).
- G: verdict 06/07/08/09 per kandidat + distribusi akhir seluruh protokol.

## 8. Isi tiap tahap C–G (dan batasnya)

- C — Pemetaan mendalam per klaster: inventaris alur + segmentasi +
  kontrak handoff deskriptif. Deskriptif: apa yang ADA di kode. Tanpa
  invariant, tanpa buruan, tanpa PoC, tanpa verdict. Per klaster; jahitan
  adalah batas (sisi yang klasternya belum dipetakan dicatat terbuka,
  dilengkapi di round klaster itu). Tarikan just-in-time lintas-klaster =
  kerja Tahap D, bukan C. Pilot Across: K1–K7 inventaris alur (C01–C07),
  K8 inventaris primitif (C08), K9 dilewati (mock) — Tahap C selesai
  18:48 WIB. C tetap per-klaster utuh (Kim: 'ya seperti ini dulu saja');
  inventory per-alur dievaluasi, tidak diadopsi untuk sekarang;
  inventory ramping per-klaster dinilai berisiko, tidak diadopsi.
- D — Eksplorasi: buruan per segmen/handoff/simpul + merumuskan invariant
  (wajib sumber) + kartu lead (wajib klaim finansial). Tanpa PoC. Unit
  round D DIPUTUS 22:33 WIB 16 Nov 2026: per-alur, dua gelombang (buta →
  berpengetahuan + katalog 52 plugin); daftar alur (enumerasi + EV, dari
  B02+B04) adalah prasyarat sebelum D. DIPUTUS 17 Nov 2026: gate finansial
  — plafon live 0 TIDAK lagi membuat lead DORMAN; yang menentukan kerugian
  yang bisa dibuktikan (C10). DIPUTUS 17 Nov 2026: standar TERBUKTI —
  kartu E yang buktinya cuma satu fungsi pendek harus dinilai ulang;
  Gelombang 1/2 tidak memberi verdict TERBUKTI (C11).
- E — Uji test: PoC fork-live + P&L terukur per kandidat. Bentuk rinci
  DIPUTUS 22:33 WIB 16 Nov 2026: yang sudah dipraktikkan (batch maks 3,
  fork blok pin + P&L). DIPUTUS 17 Nov 2026: Tahap E membuktikan rantai
  (a)+(b)+(c) sebagai satu rantai, bukan hanya satu fungsi (C11).
- F — Komposisi (DITERIMA BERSYARAT 22:33 WIB 16 Nov 2026: hanya bila
  penting dan urgent): hanya lead multi-segmen; uji F pakai data terbaru,
  tanpa fork/PoC ulang; lead satu segmen lewati F. Bukti = lemma.
- G — Verdict: verdict per kandidat (06/07/08/09; tiga verdict terpisah;
  severity dari plafon teramati); distribusi akhir di seluruh protokol.
  Bentuk rinci DIPUTUS 22:33 WIB 16 Nov 2026: yang sudah dipraktikkan.
  DIPUTUS 17 Nov 2026: verdict 09 TIDAK lagi mensyaratkan P&L live > 0;
  syaratnya kerugian yang bisa dibuktikan (C10). DIPUTUS 17 Nov 2026:
  verdict memakai standar TERBUKTI (C11) bersama C10.

## 9. Preferensi mengikat (C1–C11)

- C1 Fork-live: fork ke kondisi live (blok pin), kecuali user bilang
  testnet/devnet di awal; mekanisme penentu diuji ulang di fork live bila
  fork-testable, bila tidak catat eksplisit.
- C2 Kejujuran bukti: label DIEKSEKUSI vs DIANALISIS vs CODE-REVIEW-ONLY;
  klaim gugur dicabut sampai ke dokumennya; koreksi eksplisit.
- C3 Verify_zero_and_scan: nol = mencurigakan; simpan output lengkap +
  error; kontrol positif bukan-nol lewat jalur sama; pindai besar
  dibuktikan sensus atau dilabeli SAMPEL persis; langkah belum dijalankan =
  terbuka, bukan rejection/finding; koreksi label di dokumen yang sama.
- C4 Ambang $1 bukan ambang (direvisi 17 Nov 2026, perintah Kim: dari
  $500 turun ke $1): apapun yang merugikan protokol/sistem yang
  salah DISIMPAN sebagai kandidat berapapun kecilnya; floor $1 hanya
  konteks eligibility; tersimpan ≠ tersubmit.
- C5 HOLD: nol submisi/kontak program tanpa persetujuan eksplisit per
  kandidat; jangan buat draf submisi tanpa instruksi eksplisit.
- C6 Tiga verdict terpisah (mekanisme/eligibility/dampak); severity dari
  plafon TERAMATI; insidensi wajib analisis same-tx; plafon per-eksekusi
  tidak dikalikan tanpa bukti pengulangan.
- C7 Bahasa: Indonesia ringan dan sederhana.
- C8 Keep-hunting: tolak "no vulnerability" prematur; hasil kecil berharga;
  tetapkan target lanjutan konkret.
- C9 /stop mencabut otorisasi round seketika: hentikan subagent, tulis nol
  berkas, lanjutkan hanya atas perintah eksplisit baru.
- C10 Definisi kerugian — yang bisa dibuktikan (diputus 17 Nov 2026,
  perintah Kim): kerugian = sesuatu yang BISA DIBUKTIKAN merugikan —
  mekanisme jalan di fork/test atas kode+state nyata, plafon terukur,
  pemicu terjangkau lewat jalur operasi normal, TANPA peran istimewa dan
  TANPA state eksotis; TIDAK HARUS LIVE (kejadian nyata / P&L live /
  korban nyata bukan syarat). Konsekuensi: gate D — plafon live 0 tidak
  lagi membuat lead DORMAN; verdict 09 — tidak lagi mensyaratkan P&L live
  > 0. Sama dengan standar tiga tingkat Kim 23 Okt 2026.
- C11 Standar bukti TERBUKTI — jalur pendek yang saling bergantung (diputus
  17 Nov 2026, perintah Kim 12:14 WIB): TERBUKTI = ada CUKUP jalur pendek
  yang saling bergantung (1 atau beberapa) dari pelanggaran invariant
  sampai ke kerugian yang terukur; kerugian diukur lewat verifikasi
  alur-alur TERKAIT, bukan simpulan di dalam satu fungsi pendek — tes yang
  berhenti di satu fungsi pendek lalu menyimpulkan = masih HIPOTESIS.
  Konsekuensi: kartu E yang buktinya cuma satu fungsi pendek harus dinilai
  ulang; Gelombang 1/2 tidak memberi verdict TERBUKTI (tiap observasi/
  jawaban dilabeli DIANALISIS + dicatat alur terkait yang masih kurang);
  kartu lead merumuskan rantai (a)+(b)+(c) sebagai satu rantai yang harus
  diuji Tahap E; verdict 09/G memakai standar ini bersama C10. Berlaku ke
  G02 juga (buka ulang P7) dan mulai P8.

## 10. Keputusan yang sudah diputus (log singkat)

- 15 Nov 2026: Tahap A+B = lingkup luas satu proyek; pemetaan mendalam
  jadi tahap baru (C). Penomoran: huruf A–G (perintah "nomor baru saja"
  lalu "ubah tahap jadi huruf saja"). Output `<huruf><NN>-<NAMA>`.
- 15 Nov 2026: B6 baca manual penuh, tanpa AST; baca bertingkat ditolak;
  AST sebagai cek kedua ditolak (penghitung tetap arsip).
- 15 Nov 2026: B12 direvisi — invariant keluar dari Tahap B/C, dirumuskan
  di Tahap D.
- 15 Nov 2026: B15 direvisi dua kali — B7 ringan WAJIB di Tahap B
  (membalik keputusan B7 opsional 15:11).
- 15 Nov 2026: B14 7 konvensi sensus diputus (atas delegasi "kamu putuskan
  baiknya untuk universal").
- 15 Nov 2026: just-in-time lintas-klaster PINDAH dari Tahap C ke Tahap D
  (koreksi atas pertanyaan Kim 'bukankah ini harusnya pas eksplorasi?').
  Dipenuhi: round 1 Tahap C = K1 SAJA; K8 = round sendiri di Tahap C.
  Tahap C round 1 (K1) dimulai 17:02 WIB atas perintah Kim; brief
  dikoreksi (tanpa tarikan K2/WETH9).
- Syarat mengikat Kim: "jahitan benar interaksi hulu-hilir itu lengkap"
  (mekanisme B16 masih usulan).
- 15 Nov 2026: Tahap C tetap per-klaster utuh (Kim: 'ya seperti ini dulu
  saja'); inventory per-alur dievaluasi (jaminan + 7 konsekuensi) tapi
  tidak diadopsi untuk sekarang; inventory ramping per-klaster dinilai
  BERISIKO oleh Kim — tidak diadopsi.
- 15 Nov 2026 (18:32 WIB, MENGIKAT): setelah K6, lanjutkan Tahap C terus
  sampai semua klaster lengkap, tanpa tanya per round; yang tidak perlu
  tidak dilakukan. Tafsir: K7 round penuh; K8 inventaris primitif; K9
  dilewati (mock).
- 15 Nov 2026 (18:48 WIB): Tahap C SELESAI — K1–K7 inventaris alur +
  K8 primitif; masuk alur 145/169; SM 11/12 TERTUTUP (SM-05 dienumerasi,
  butuh keputusan Kim tentang konsumennya); H 0, E 0.
- 16 Nov 2026 (22:33 WIB): 10 butir diputus Kim — (1) B16 TIDAK DIPAKAI; (2) Tahap F DITERIMA BERSYARAT (hanya bila penting dan urgent); (3) Tahap D per-alur + dua gelombang; (4) Tahap E/G bentuk rinci = yang dipraktikkan, uji F tanpa fork/PoC ulang (data terbaru); (5) B9 rerun sensus manual DITERIMA; (6) A5 dua celah DITERIMA; (7) arah metode: pakai paling mutakhir (flow money first = metode kerja aktif); (8) deep-run per jalur DITERIMA (mulai P4); (9) larangan filter di dalam klaster DITULIS ke berkas kanonik; (10) SM-05 tetap dieksplorasi ketika terkait.
- 17 Nov 2026 (perintah Kim "tulis begitu"): C10 — definisi kerugian = yang bisa dibuktikan (bukan syarat istimewa, tidak harus live); gate D dan syarat 09 direvisi mengikuti definisi ini.
- 17 Nov 2026 (perintah Kim 12:14 WIB): C11 — standar bukti TERBUKTI = jalur pendek yang saling bergantung sampai ke kerugian terukur lewat alur terkait; tes satu fungsi pendek lalu menyimpulkan = masih HIPOTESIS; kartu E yang buktinya cuma satu fungsi pendek harus dinilai ulang; berlaku ke G02 juga (buka ulang P7) dan mulai P8.

## 11. Belum diputus

- Inventory per-alur (Tahap C) — dievaluasi, tidak diadopsi untuk
  sekarang; bisa dibuka lagi.
- Cara parser independen sapuan akhir (B9) — terbuka sebagai riset
  (rerun sensus manual sudah diterima sebagai enumerasi kedua).
- Skill per tahap A–G + `verify_flow.py` — item pembangunan repo
  (bukan rule); bentuk rinci Tahap D–G sudah diputus sehingga skill
  kini tidak lagi terblokir bentuk.

## 12. Angka terukur pilot Across (acuan biaya)

- Sensus statis: ~14–23 mnt/putaran (rerun 13 mnt 36 dtk, 196 berkas,
  1.245 baris, EP+VM 169).
- Sensus live penuh: ~34 mnt (bukan patokan lean); B7 ringan ≤5 mnt
  (target, belum terukur).
- Pass enumerasi jahitan: ~8 mnt (9 klaster, 12 jahitan).
- Scope lock: ~2 mnt. Tahap C per klaster (TERUKUR): K1 ~6, K2 ~6,
  K3 ~7, K4 ~7, K5 ~7, K6 ~8, K7 ~5, K8 ~3 mnt; total ~49 mnt untuk
  8 round (145/169 EP+VM masuk alur).
- Pilot: 196 berkas, 1.245 baris sensus, 169 EP+VM, 9 klaster, 12 jahitan
  (live 3, parsial 3, belum diukur 6), ledger seed 751 entrypoint. Graf
  BUKAN DAG (2 siklus: K1⇄K2, K2⇄K3). EV 10k blok: K1 18.556 > K2 1.170 >
  K3 268 > K4–K8 belum diukur. Tahap C selesai 18:48 WIB: K1–K7
  inventaris alur + K8 primitif; masuk alur 145/169; SM 11/12 TERTUTUP
  (SM-05 dienumerasi); H 0, E 0; sisa 24 = K9 mock dilewati.

## 13. Berkas kanonik & cara pakai

- Aturan kanonik: `aturan-alur-first.md` (A1–A5, B1–B16, C1–C11, seksi D,
  wajib/TIDAK, Belum diputus).
- Acuan workflow Tahap B: `acuan-workflow-tahap-B.md` (konsolidasi +
  definisi + artefak; pekerja Tahap B jalan dari berkas ini + INTEL
  proyeknya).
- Resume ini: ringkasan untuk acuan cepat — bukan pengganti berkas aturan.
- Draf usulan awal: `usulan-alur-first.md` (bukan rule).
- Pilot: `~/workspace/theadvers/projects/across-flow-first/` (B01–B07 +
  C01–C08 selesai; Tahap C lengkap 18:48 WIB). Berkas kanonik audit
  Across di `~/workspace/theadvers/projects/across/` read-only.
