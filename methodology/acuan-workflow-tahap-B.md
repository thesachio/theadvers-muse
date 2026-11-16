# Acuan Workflow Tahap B — Alur-First (Sensus + Scope Mapping)

Status: acuan untuk proyek berikut. Disusun 15 Nov 2026 dari aturan
yang sudah diputus (`aturan-alur-first.md`) + bukti pilot Across.
Yang ada di sini hanya yang sudah diputus; yang belum diputus
tercatat di bagian akhir — bukan rule.

Aturan kanonik tinggal di `aturan-alur-first.md` (A1–A5, B1–B16,
C1–C9). Dokumen ini = konsolidasi khusus Tahap B: semua rule yang
dipakai di Tahap B + definisi tiap istilah, supaya pekerja Tahap B
proyek berikut bisa jalan hanya dari dokumen ini + INTEL proyeknya.

## 1. Tujuan Tahap B

Tahap B = tahu scope menyeluruh di awal (sensus utuh + peta alur),
tanpa buruan. Keluarannya — dua lapis:
A. Sekali, untuk seluruh protokol (bagian depan):
1. B01-SCOPE.md — daftar scope + scope lock (B13).
2. B02-CENSUS-STATIC.md + staging JSON — sensus statis 100%
   peristiwa pemindah nilai (B1/B6). (Field 'keluar' tiap fungsi EP+VM
   TIDAK wajib — B16 tidak dipakai, diputus 22:33 WIB 16 Nov 2026.)
3. B03-CENSUS-LIVE.md — inventaris alamat live (daftar) + sensus live
   ringan B7 (WAJIB, B15); sensus live penuh opsional, sisanya
   just-in-time per klaster (B8).
4. B04-CLUSTERS.md — identifikasi klaster + daftar handoff (B11).
   (Daftar jahitan SM-xx lengkap TIDAK wajib — B16 tidak dipakai,
   diputus 22:33 WIB 16 Nov 2026.)
5. Ledger sisa (B10) — seed: entrypoint belum masuk alur, handoff
   terbuka, klaster belum dikerjakan.
6. Registry invariant (B3) — struktur + penomoran (kosong; diisi
   Tahap D).
Per klaster (Tahap C — pemetaan mendalam, bukan Tahap B): inventaris
alur klaster + segmentasi + kontrak handoff klaster (deskriptif) —
jaminan produsen + asumsi konsumen sebagaimana teramati di kode
(nama+tipe+satuan field). Invariant TIDAK dibuat di Tahap B atau
Tahap C (B12) — dirumuskan di Tahap D.

TIDAK di Tahap B: pemetaan mendalam per klaster (Tahap C), buruan
kandidat, PoC, verdict (06/07/08/09), komposisi jalur serangan,
submisi, kontak program, transaksi mainnet/testnet.

## 2. Masuk (sebelum Tahap B jalan)

- INTEL (Tahap A, A1–A5): brief 9 butir; stage; pin commit + blok
  referensi. INTEL kanonik read-only selama audit.
- Scope lock (B13): daftar berkas scope cocok persis inventaris
  kanonik (diff kosong), commit terverifikasi, contracts/ bersih.
- Mode Uji diturunkan dari stage (A3): MAINNET → FORK LIVE_MATCH;
  HEAD bisa lebih lama atau lebih baru dari live — bukan patokan
  kebenaran; D fokus ke yang cocok LIVE_MATCH.

## 3. Rule Tahap B (B1–B16)

### B1. Sensus = 100% peristiwa pemindah nilai
Definisi: sensus menghitung SEMUA cara nilai bisa bergerak, bukan
cuma fungsi: entrypoint + transfer langsung/donasi/force-feed +
pergerakan level-sistem (selfdestruct/coinbase) + multicall/dispatch
dinamis transitif + proxy→implementation + callback/hook + transfer
SOL/SPL ke PDA + langkah off-chain (dilabeli asumsi).
Fungsi = unit sensus (kelengkapan); segmen = unit buruan (partisi).
Cara pakai: tiap fungsi di tiap berkas scope dapat satu baris JSON
dengan verdict value_moves (yes/no/unknown) + entrypoint; peristiwa
bukan-fungsi (transfer langsung, force-feed, dsb.) dicatat sebagai
permukaan berkas/sistem, bukan baris fungsi.

### B2. Scope menyeluruh di awal, eksplorasi per klaster
Definisi: luasan diketahui utuh di awal (sensus + daftar scope);
kerja mendalam jalan round per round per klaster alur, diurutkan EV
live; rolling Tahap C → Tahap D → Tahap E per klaster (Tahap B
sensus utuh → Tahap C klaster 1 → Tahap D klaster 1 → Tahap E klaster 1
→ Tahap C klaster 2 → …).
Cara pakai: jangan mulai buruan sebelum sensus utuh selesai; jangan
kerjakan semua klaster sekaligus. Urutan EV live = default, bukan
gerbang: bila EV tidak dipakai (data live tidak ada — B15 — atau Kim
memerintah urutan lain), pakai urutan lain yang dicatat alasannya
(mis. hulu→hilir dari peta alur, atau pilihan Kim); cakupan tetap
dijamin B9/B10, yang hilang hanya prioritas uang-dulu.

### B3. Yang TETAP UTUH
Definisi: hal yang tidak boleh dipecah per klaster — sensus (statis
+ live), daftar scope, inventaris alamat live, cek penutupan graf
akhir, registry invariant (satu penomoran), aturan/gerbang.
Cara pakai: klaster boleh dipecah, tapi enam hal ini selalu utuh
untuk seluruh protokol.

### B4. Yang BISA DIPECAH per klaster
Definisi: inventaris alur + segmentasi, kontrak handoff (Tahap C); penulisan invariant (Tahap D) + uji invariant (Tahap E); komposisi (Tahap F); verdict (Tahap G).
Cara pakai: pecah kerja mendalam per klaster (diurutkan EV live);
sensus dan aturan tetap utuh (B3).

### B5. Peta utuh, bukti just-in-time
Definisi: mapping jalur dan relasi di depan harus menyeluruh
(daftar entrypoint, daftar domain/klaster, hubungan antar kontrak,
ledger sisa). Kerja yang terikat satu klaster/lead — terutama
sensus live — TIDAK dijalankan di scope besar; dikerjakan di
fase/klaster yang membutuhkannya.
Cara pakai: di depan cukup peta + sensus live singkat (B7);
verifikasi live per klaster jalan just-in-time (B8).

### B6. Sensus statis: baca manual penuh, tanpa AST
Definisi: semua fungsi di semua berkas dibaca manual dan dapat
verdict (yes/no/unknown); verifikasi parent + kontrol positif tetap
jalan. TIDAK pakai AST (diputus Kim 15 Nov 2026).
Cara pakai: kelompokkan berkas (pilot: G01–G10), satu worker per
kelompok membaca tiap fungsi dari kode; koordinator verifikasi tiap
staging sebelum sintesis; kontrol positif 9/9 wajib lolos; nol
mekanis wajib dipecahkan. B9 (usaha yang cukup) TIDAK berlaku di
sini — kedalaman sensus statis tidak diambil risiko.
Bukti biaya (pilot Across): putaran 1 ~23 menit; rerun 13 menit 36
detik; biaya jujur satu putaran ~14–23 menit.

### B7. Cek rantai utama proyek, singkat dan ringan
Definisi: cek SATU rantai utama proyek (rantai tempat kontrak inti
ter-deploy, menurut INTEL + deployments/; bila INTEL tidak menyebut
eksplisit: rantai dengan alamat deployment inti terbanyak). Bila
rantai utama tidak bisa dicek (RPC tidak terjangkau / tidak ada kode
di alamat inti / tidak ada blok referensi), cek rantai utama KEDUA
(deployment inti terbanyak berikutnya). Bila satu rantai tidak
merespons (timeout/403/429), jangan tunggu — langsung pindah ke
rantai kedua. Bila keduanya gagal: B7 selesai dengan catatan (apa
yang dicoba + hasil), verifikasi pindah ke B8.
Tes ringan = (1) getCode alamat kontrak inti (1–3 alamat) pada blok
referensi; (2) bila ada kode, satu panggilan view murah (mis.
liveness()/owner()). TIDAK termasuk: jejak event, hitung log,
verifikasi seluruh inventaris alamat, cek rantai lain — itu kerja B8
per klaster. Anggaran maks 5 menit untuk seluruh B7 (target, belum
terukur); bila habis, berhenti dengan hasil yang ada + catatan.
Cara pakai: B7 tetap OPSIONAL (lihat B15): bila dilewati, sensus
live di Tahap B tinggal inventaris alamat klaim-repo (statis) +
catatan verifikasi ditunda ke B8.

### B8. Cek live-match diulang per klaster; TUNDA-LIVE
Definisi: cek live-match diulang di awal fase uji per klaster,
sebelum PoC ditulis. Yang tidak live-match masuk wadah TUNDA-LIVE.
Cara pakai: TUNDA-LIVE = tercatat di ledger sisa (tetap terhitung,
bukan hilang); nol waktu eksekusi (tanpa PoC, tanpa fork); dibuka
lagi hanya bila deployment live muncul atau Kim memerintah cek ulang;
wadah ditinjau ulang di sapuan akhir.

### B9. Scope mapping usaha yang cukup + sapuan akhir
Definisi: sensus di depan hanya perlu usaha yang cukup — miss bisa
terdeteksi di eksplorasi lewat (1) baca ulang mendalam per klaster;
(2) cek handoff + konservasi (delta tak terjawab diselidiki);
(3) jejak event live sebagai enumerasi independen; (4) cek
penutupan graf akhir. Batasan jujur: cek penutupan tidak bisa
menemukan item yang tidak ada di daftarnya; miss di klaster belum
dijelajahi tetap tersembunyi sampai round-nya.
Penopang wajib: sapuan akhir dengan enumerasi independen kedua
dibanding ledger akhir. Caranya MASIH TERBUKA — bukan AST (diputus
15 Nov 2026) dan bukan regex yang sama (terbukti cacat di pilot).
Sensus di depan adalah rencana + daftar periksa, bukan bukti
kelengkapan; buktinya datang dari eksplorasi + sapuan akhir.
Catatan: B9 TIDAK berlaku untuk kedalaman sensus statis (B6).

### B10. Ledger sisa
Definisi: ledger "sisa" sesudah tiap round — entrypoint belum masuk
alur, handoff terbuka/jahitan terbuka, klaster belum dikerjakan.
Cara pakai: round selesai hanya bila sisa menyusut; round tanpa
hasil baru = sinyal berhenti dan periksa; target buruan harus ada di
sensus — kalau tidak, di luar scope menurut konstruksi.

### B11. Handoff dienumerasi dari batas unit
Definisi: inventaris handoff dienumerasi dari batas unit (rantai
linear N unit → N−1 handoff), bukan dilabeli tangan. Simpul
fork/join jadi unit sendiri.
Cara pakai: tiap batas antar segmen dapat satu kontrak handoff
(produsen, konsumen, field nama+tipe+satuan/desimal, jaminan
produsen, asumsi konsumen); gerbang memeriksa output N memenuhi
input N+1.

### B12. Invariant TIDAK dinyatakan di Tahap B
Definisi: Tahap B hanya membangun yang riil/teramati untuk
seluruh protokol — sensus, klaster, daftar handoff (B11). (B16 tidak dipakai, diputus 22:33 WIB 16 Nov 2026.) Tahap C
membangun yang riil/teramati per klaster — inventaris alur,
segmentasi, kontrak handoff deskriptif (jaminan produsen + asumsi
konsumen sebagaimana teramati di kode). Invariant adalah klaim asumsi
agen tentang apa yang SEHARUSNYA berlaku — bukan fakta teramati —
jadi dirumuskan di Tahap D (eksplorasi), bersama kartu leadnya, dan
diuji di Tahap E. (Direvisi 15 Nov 2026 atas perintah Kim —
menggantikan 'invariant DINYATAKAN dulu'; pemetaan mendalam pindah
ke Tahap C, 15 Nov 2026, perintah Kim 'nomor baru saja'.)
Cara pakai: setiap invariant di Tahap D WAJIB mencantumkan
sumbernya: field kontrak handoff yang bersangkutan, atau baris
docs/NatSpec — bukan perasaan agen. Tiga lapis tetap: L1 dalam
segmen (INV-Sx), L2 handoff + simpul (INV-Hx, INV-Jx), L3
ujung-ke-ujung/komposisi (INV-Fx). Tiap invariant wajib akibat
finansial bila pecah. Cek testability dicatat saat perumusan (fork
apa, data live apa; dilabeli bila tidak bisa diuji). Registry
invariant tetap satu penomoran global (B3), diisi mulai Tahap D.

### B13. Scope lock
Definisi: sebelum sensus jalan — daftar berkas scope harus cocok
persis dengan inventaris kanonik (diff kosong), commit
terverifikasi, contracts/ bersih.
Cara pakai: kunci dulu, sensus kemudian; drift di luar scope
dicatat, tidak dihitung.

### B14. Konvensi sensus statis (7 butir, universal)
Diputus 15 Nov 2026 atas delegasi Kim.
1. Getter publik = entrypoint (ya). Entrypoint = bisa dipanggil
   dari luar (literal, mekanis); filter nilai lewat value_moves;
   yang mengikat alur tetap EP+VM.
2. Receive badan kosong = value_moves no. Menerima saja bukan
   memindahkan; value_moves dinilai dari badan — ada perpindahan
   lanjutan (transfer keluar / perubahan saldo pihak lain) → yes;
   hanya menerima → no; tidak bisa dipastikan → unknown.
3. Fungsi payable yang memanggil sistem eksternal yang diketahui
   (mis. relayMessage adapter → messenger bridge) = value_moves
   yes; tahap alur WAJIB verifikasi penerusan value per adapter
   dari badan — bila tidak meneruskan, turunkan ke no.
4. Dispatcher dengan target/calldata arbitrer (Multicall3
   aggregate*, deployAndExecute, directWithdraw, dsb.) =
   value_moves unknown; diselesaikan transitif di inventaris alur
   (ikuti ke implementasi/target).
5. Getter auto-generated DIBARISKAN (konsisten dengan butir 1):
   semua getter publik dibariskan (entrypoint yes, value_moves no).
6. Entrypoint warisan TIDAK dibariskan (satu baris per definisi, di
   berkas pendefinisi). Tahap alur WAJIB punya langkah perluasan
   permukaan lewat implementasi/warisan sebelum segmentasi.
7. Atribut berkas direct/proxy dicatat dua lapis: permukaan
   milik-sendiri + permukaan warisan. Inventaris alur memakai lapis
   warisan untuk permukaan deployed, lapis milik-sendiri untuk
   lokasi kode.

### B15. Cek onchain: sensus live ringan B7 WAJIB di Tahap B; cek
onchain mendalam WAJIB di B8
Definisi: Tahap A TIDAK cek onchain (A2 — brief klaim saja). Tahap
1 wajib B7 ringan — getCode 1–3 alamat inti di blok referensi + satu
view murah, rantai utama (fallback rantai utama kedua), anggaran
5 menit — sebagai konfirmasi live bahwa klaim INTEL/deployment
benar, sumber data EV, dan sinyal live untuk identifikasi klaster.
Sensus live PENUH (semua rantai, jejak event lengkap) tetap OPSIONAL
di Tahap B. Cek onchain mendalam WAJIB di B8: live-match per klaster
sebelum PoC ditulis; yang tidak live-match masuk TUNDA-LIVE; PoC
klaster itu TERBLOKIR sampai cek live-match ada. (Direvisi 15 Nov
2026 atas perintah Kim 'perlu sensus live ringan' — menggantikan
revisi 15:11 yang membuat B7 opsional.)
B8 tetap menjadi gerbang: tanpa cek live-match, tidak ada PoC; tidak
boleh PoC HEAD lalu diklaim sebagai bukti live. Pengecualian: proyek
TESTNET/DEVNET — "live" = lingkungan itu sendiri (A3).
Daftar lengkap yang wajib selesai di Tahap B: lihat seksi 'Tahap B —
yang wajib selesai' di aturan-alur-first.md.

## 4. Definisi istilah

- **Sensus statis**: baca manual penuh semua fungsi di semua berkas
  scope; tiap fungsi satu baris JSON (file/contract/function/line/
  end_line/visibility/mutability/payable/entrypoint/value_moves/
  mechanisms/events_emitted/external_calls/notes).
- **Sensus live**: inventaris alamat live + keberadaan kode live +
  jejak pemakaian entrypoint (event) di live; di depan singkat
  (rantai utama proyek, B7).
- **Peristiwa pemindah nilai**: segala cara nilai bergerak (B1) —
  bukan cuma fungsi.
- **Entrypoint**: fungsi yang bisa dipanggil dari luar
  (public/external, termasuk getter dan receive/fallback) — B14.1.
- **value_moves**: yes/no/unknown (B14.2–B14.4). Unknown =
  diselesaikan transitif di inventaris alur, bukan temuan.
- **EP+VM**: entrypoint DAN value_moves yes — angka yang mengikat
  untuk inventaris alur (bukan EP total).
- **Klaster alur**: kelompok alur ujung-ke-ujung, diurutkan EV live
  (pilot Across: K1 deposit→fill, K2 bundle→relay, K3 refund, K4
  periphery, K5 mintburn/HyperCore, K6 counterfactual, K7 klaim
  Merkle, K8 primitif eksternal, K9 mock bukan-live).
- **Unit / segmen**: unit = satu aliran uang ujung-ke-ujung;
  segmen = potongan alur di handoff yang bisa diamati (event,
  perubahan state, pesan lintas-chain, batas waktu, langkah
  off-chain); segmen = unit buruan.
- **Handoff**: batas antar segmen + kontraknya (B11).
- **Simpul (fork/join)**: titik percabangan/penggabungan aliran;
  fork → cek konservasi (Σkeluaran = masukan − fee); join → cek
  konsumsi tepat sekali; simpul = unit sendiri, prioritas tinggi.
- **Invariant tiga lapis**: INV-Sx / INV-Hx / INV-Jx / INV-Fx (B12; dirumuskan di Tahap D, bukan Tahap B atau 2).
- **Ledger sisa**: daftar sisa sesudah tiap round (B10).
- **TUNDA-LIVE**: wadah yang tidak live-match (B8).
- **Cek penutupan graf**: cek akhir bahwa semua entrypoint/alur
  tertutup; batasan: tidak bisa menemukan item yang tidak ada di
  daftarnya (B9).
- **Sapuan akhir**: enumerasi independen kedua dibanding ledger
  akhir (cara terbuka, B9).
- **Kontrol positif**: fungsi yang diketahui pasti pemindah nilai,
  dicek di baris yang dipin untuk membuktikan sensus tidak buta
  (pilot: 9/9 — depositV3:611, fillRelay:986,
  executeSlowRelayLeaf:1242, requestSlowFill:1157,
  proposeRootBundle:560, executeRootBundle:612,
  disputeRootBundle:715, addLiquidity:449, removeLiquidity:476).
- **Nol mekanis**: hitung mekanis yang memberi 0 — wajib dipecahkan
  (cari nama sebenarnya), bukan diterima.
- **Unresolved**: butir belum terjawab di sensus (unknown + catatan
  lain) — daftar kerja inventaris alur, bukan temuan.
- **EV live**: nilai harapan per alur dari data live (volume/saldo
  per entrypoint) — penentu urutan klaster.
- **Mode Uji**: diturunkan dari stage (A3); MAINNET → FORK
  LIVE_MATCH.

## 5. Artefak yang dihasilkan Tahap B

- `A01-INTEL.md` — brief Tahap A (9 butir, A1) + pin + waktu.
- `B01-SCOPE.md` — scope lock (B13): target, pin commit, daftar
  berkas, yang di luar scope + alasan.
- `B02-CENSUS-STATIC.md` — hasil sensus statis (hitungan, kontrol,
  nol terpecahkan, batasan).
- `B03-CENSUS-LIVE.md` — hasil sensus live (inventaris alamat,
  kehadiran kode + jejak event (bila B7 dijalankan; bila B7 dilewati,
  verifikasi ditunda ke B8 per klaster), batasan RPC, koreksi).
- `notes/staging-G01..G10-*.md` — staging per kelompok (baris JSON
  per fungsi) + `notes/staging-synthesis.md`.
- `notes/clusters-draft.md` — draf klaster + ledger sisa benih
  (entrypoint → klaster, per entrypoint saat round-nya jalan).
- `scripts/` + `scripts/out/` — hitung mekanis (regex, BUKAN AST)
  + output hitungan.
- Bila sensus diulang (pengukuran biaya): folder
  `notes/rerun-<tanggal>/` terpisah + `comparison-parent.md`
  (perbandingan oleh parent); berkas sensus pertama TIDAK ditimpa.

## 6. Rule pendukung yang dipakai di Tahap B

Rule ini tinggal di `aturan-alur-first.md`; yang dipakai di Tahap B:
- A3 — stage = filter Mode Uji (D fokus LIVE_MATCH).
- A5 — pin commit + blok referensi; INTEL kanonik read-only.
- C1 — fork-live (sensus live mengikuti kondisi live nyata).
- C2 — kejujuran bukti (DIEKSEKUSI vs DIANALISIS vs
  CODE-REVIEW-ONLY; koreksi eksplisit).
- C3 — verify_zero_and_scan (nol mencurigakan; kontrol positif;
  pindai dibuktikan sensus atau dilabeli SAMPEL).
- C7 — bahasa Indonesia ringan dan sederhana.
- C9 — /stop mencabut otorisasi round seketika.

## 7. Biaya terukur (pilot Across, 15 Nov 2026)

- Tahap A: 32 detik (merangkum INTEL kanonik COMPLETE); 62 detik
  (penemuan dari across.to, 9/9 DITEMUKAN, tangga A4).
- Sensus statis: putaran 1 ~23 menit (11:46–12:09 WIB); rerun 13
  menit 36 detik (14:33:35–14:47:11 WIB). Biaya jujur satu putaran:
  ~14–23 menit. Rerun lebih cepat kemungkinan karena scope dan
  format sudah dikenal, bukan karena metode membaik.
- Sensus live (pilot penuh, sebelum B7): ~34 menit (12:09–12:43
  WIB); dengan B7 (singkat, rantai utama saja) akan lebih pendek — belum diukur; anggaran B7 yang baru maks 5 menit (target).
- Total Tahap B pilot: ~57 menit (11:44–12:43 WIB).
- Sensus statis pilot: 196 berkas, 1.230 baris (putaran 1) / 1.245
  baris (rerun, +15 deklarasi interface); sesudah B14: daftar kerja
  1.267 baris (VM 256/780/231, EP 751, EP+VM 169).

## 8. Yang masih terbuka (bukan rule)

- Cara enumerasi independen untuk sapuan akhir (B9) — bukan AST,
  bukan regex yang sama.
- Aturan pin commit di run dari-0 + blok referensi (A5) — Kim
  menunda untuk fase ini (15 Nov 2026).
- Urutan klaster + bentuk rinci Tahap C–G — keputusan per proyek (struktur modular A–G di aturan-alur-first.md seksi D)
  (pilot Across: tiga keputusan klaster masih menunggu Kim).
- Arah metode (terima/tolak sebagai pengganti Segitiga) — pilot
  adalah buktinya, bukan keputusannya.

---

Disusun dari: `aturan-alur-first.md` (A1–A5, B1–B15, C1–C9),
`usulan-alur-first.md`, dan berkas pilot
`~/workspace/theadvers/projects/across-flow-first/` (A01-INTEL.md,
B01-SCOPE.md, B02-CENSUS-STATIC.md, B03-CENSUS-LIVE.md,
notes/clusters-draft.md, notes/rerun-2026-11-15/).
