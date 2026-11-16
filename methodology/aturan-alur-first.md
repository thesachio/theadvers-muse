# Aturan Alur-First — Tahap A (INTEL) dan Tahap B (Scope Mapping)

Status: aturan mengikat untuk metode alur-first. Metode kerja aktif sejak 22:33 WIB 16 Nov 2026 (diputus Kim: 'pakai paling mutakhir'; Segitiga menjadi rujukan/legacy). Disusun 15 Nov 2026
dari keputusan Kim + preferensi yang sudah mengikat. Sumber: draf
`usulan-alur-first.md` + `MEMORY.md` + berkas pilot
`~/workspace/theadvers/projects/across-flow-first/B01-SCOPE.md`.
Penerapan pertama: pilot Across.
Acuan workflow Tahap B (konsolidasi B1–B14 + definisi, untuk proyek
berikut): `acuan-workflow-tahap-B.md`.

Yang ada di sini hanya yang sudah diputus. Yang belum diputus tercatat di
bagian akhir sebagai usulan/belum diputus — bukan rule.

## A. INTEL

A1. INTEL = brief, bukan dokumen teknis. Isinya 9 butir: nama project
(singkat); deskripsi lingkup project (bridge/swap/LP dll, singkat);
deployment stage; Repo (alamat github atau setara); website (tempat user
akses dan interaksi UI aktif); docs (link); audit (singkat: siapa + link);
bugbounty (URL); brief severity (singkat bila ada).

A2. Pre-0 = daftar docs pendukung saja, BUKAN verifikasi teknis.

A3. Stage (MAINNET/TESTNET/DEVNET) = filter pertama yang menentukan Mode
Uji di D (FORK LIVE_MATCH). HEAD bisa lebih lama atau lebih baru dari
live — bukan patokan kebenaran. D fokus ke yang cocok LIVE_MATCH.

A4. Batas pencarian Tahap A (15 Nov 2026, perintah Kim): peran agen
Tahap A untuk proyek baru — mulai hanya dari nama proyek (+ tautan
yang user beri, bila ada). Tiap butir INTEL dicari lewat tangga
sumber berurutan, maks 3 cek per butir. Bila tangga habis tanpa hasil
→ TIDAK KETEMU (bukan bukti tidak ada). Tiga status hasil: DITEMUKAN
(dengan tautan), TIDAK ADA (sumber resmi menyatakan tidak ada),
TIDAK KETEMU (tangga habis, tidak terbukti ada/tidak). Tiap TIDAK
KETEMU wajib dicatat apa yang sudah dicoba (sumber + hasil). Dua
butir penahan beban: stage (butir 3) dan repo (butir 4) — bila salah
satunya TIDAK KETEMU, Tahap A berhenti dan eskalasi ke Kim (tidak
ditebak). Anggaran total penemuan maks 30 menit kerja agen; bila
habis, berhenti dengan hasil yang ada + TIDAK KETEMU untuk sisanya.
INTEL COMPLETE hanya bila semua butir DITEMUKAN (atau TIDAK ADA
dengan sumber yang menyatakan); bila ada TIDAK KETEMU → PARTIAL +
tabel Catatan Ketiadaan.

Tangga per butir (urutan cek):
- (1) Nama: dari user, tanpa pencarian. Bila ambigu, satu putaran
  klarifikasi; bila tetap ambigu, catat apa adanya.
- (2) Deskripsi lingkup: docs intro → README repo → deskripsi repo
  GitHub. Berhenti bila satu sumber menjelaskan lingkup dalam satu
  paragraf.
- (3) Stage: docs (apakah mainnet live?) → app UI (apakah hidup?) →
  repo (deployments/README). Stage di Tahap A = klaim docs/app/repo
  saja (A2: bukan verifikasi teknis); konfirmasi teknis di Tahap B
  (sensus live / deploy_parity). Bila tidak ada klaim → TIDAK KETEMU
  → eskalasi.
- (4) Repo: tautan di docs → GitHub search nama proyek → tautan di
  website. Hanya pilih bila jelas resmi (org cocok, ditaut dari
  docs); bila ada beberapa kandidat dan tidak jelas → TIDAK KETEMU
  (ambigu) + eskalasi. JANGAN menebak URL dari nama.
- (5) Website: tautan di docs → tautan di README repo → web search
  '<proyek> app'. Cek hidup murah: URL merespons (200/30x). Bila URL
  ketemu tapi tidak merespons → catat kodenya, bukan TIDAK KETEMU.
- (6) Docs: tautan di README → tautan di website → web search
  '<proyek> docs'. Sama: tidak merespons ≠ tidak ketemu.
- (7) Audit: halaman audit/security di docs → folder audits/ di repo
  → web search '<proyek> audit report' (1 pencarian, 3 hasil
  teratas). Bila tidak ketemu → TIDAK KETEMU + PARTIAL (tidak adanya
  di docs+repo bukan bukti tidak ada audit).
- (8) Bugbounty: halaman bug bounty di docs → SECURITY.md/README repo
  → web search '<proyek> bug bounty' (1 pencarian). Bila tidak
  ketemu → TIDAK KETEMU + PARTIAL.
- (9) Brief severity: diturunkan dari halaman bounty (butir 8). Bila
  bounty TIDAK KETEMU, severity ikut TIDAK KETEMU; tidak ada
  pencarian terpisah.

A5. INTEL dikunci dengan pin: commit + blok referensi (+ stage). INTEL
kanonik read-only selama audit — tidak disentuh. Bila D menemukan INTEL
salah, koreksi dicatat eksplisit dan terbuka, tidak diam-diam. Diputus 22:33 WIB 16 Nov 2026: (a) commit yang dipin di run dari-0 = HEAD saat penemuan; (b) blok referensi ditentukan di Tahap B (sensus live).

## B. Scope Mapping

B1. Sensus = 100% peristiwa pemindah nilai (bukan fungsi saja):
entrypoint + transfer langsung/donasi/force-feed + pergerakan
level-sistem (selfdestruct/coinbase) + multicall/dispatch dinamis
transitif + proxy→implementation + callback/hook + transfer SOL/SPL ke
PDA + langkah off-chain (dilabeli asumsi). Fungsi = unit sensus
(kelengkapan); segmen = unit buruan (partisi).

B2. Scope menyeluruh diketahui di awal; pemetaan mendalam +
eksplorasi round per round per klaster alur, diurutkan EV live;
rolling Tahap C → Tahap D → Tahap E per klaster. Urutan
EV live = default, bukan gerbang. Bila EV tidak dipakai (data live
tidak ada — lihat B15 — atau Kim memerintah urutan lain), pakai
urutan lain yang dicatat alasannya (mis. hulu→hilir dari peta alur,
atau pilihan Kim). Cakupan tetap dijamin B9/B10; yang hilang hanya
prioritas uang-dulu.

B3. Yang TETAP UTUH: sensus (statis + live), daftar scope, inventaris
alamat live, cek penutupan graf akhir, registry invariant (satu penomoran —
diisi mulai Tahap D, lihat B12), aturan/gerbang.

B4. Yang BISA DIPECAH per klaster: inventaris alur + segmentasi, kontrak
handoff, penulisan invariant, uji invariant, komposisi, verdict.

B5. Peta utuh, bukti just-in-time: mapping awal jalur dan relasi harus
menyeluruh (cakupan); kerja yang bisa dikerjakan di fase/klaster
spesifik TIDAK dijalankan di scope besar.

B6. Sensus statis: baca manual penuh — semua fungsi di semua berkas
dibaca dan dapat verdict (yes/no/unknown), seperti pilot Across
(1.230 baris, 196 berkas, ~23 menit — masih toleransi). Verifikasi
parent + kontrol positif tetap jalan. TIDAK pakai AST (diputus Kim
15 Nov 2026): penghitung AST sempat dibangun dan divalidasi terhadap
sensus pilot (AST 1.245 = manual 1.230 + 15 deklarasi interface, 0
manual terlewat, kontrol positif 9/9, 6/6 baris hantu bersih,
deterministik) tapi TIDAK dipakai — berkasnya tetap ada di folder
pilot sebagai referensi (`B07-AST-COUNTER.md`), bukan bagian dari
metode. B9 (usaha yang cukup) TIDAK berlaku untuk kedalaman sensus
statis — di sini tidak diambil risiko.

B7. Cek rantai utama proyek, singkat dan ringan (diputus Kim 15 Nov
2026 — menggantikan 'cek ETH L1'): cek SATU rantai utama proyek
(rantai tempat kontrak inti ter-deploy, menurut INTEL + deployments/;
bila INTEL tidak menyebut eksplisit: rantai dengan alamat deployment
inti terbanyak). Bila rantai utama tidak bisa dicek (RPC tidak
terjangkau / tidak ada kode di alamat inti / tidak ada blok
referensi), cek rantai utama KEDUA (deployment inti terbanyak
berikutnya). Bila satu rantai tidak merespons (timeout/403/429),
jangan tunggu — langsung pindah ke rantai kedua. Bila keduanya
gagal: B7 selesai dengan catatan (apa yang dicoba + hasil),
verifikasi pindah ke B8. Tes ringan = (1) getCode alamat kontrak
inti (1–3 alamat) pada blok referensi; (2) bila ada kode, satu
panggilan view murah (mis. liveness()/owner()). TIDAK termasuk:
jejak event, hitung log, verifikasi seluruh inventaris alamat, cek
rantai lain — itu kerja B8 per klaster. Anggaran maks 5 menit untuk
seluruh B7 (target, belum terukur); bila habis, berhenti dengan
hasil yang ada + catatan. B7 tetap OPSIONAL (lihat B15): bila
dilewati, sensus live di Tahap B tinggal inventaris alamat
klaim-repo (statis) + catatan verifikasi ditunda ke B8. Yang hilang
bila dilewati: peringatan dini bila INTEL salah / belum ada
deployment — baru ketahuan di cek B8 klaster pertama; kerja statis
tetap valid sebagai peta kode (menunggu di TUNDA-LIVE).

B8. Cek live-match diulang di awal fase uji per klaster, sebelum PoC
ditulis. Yang tidak live-match masuk wadah TUNDA-LIVE: tercatat di
ledger sisa (tetap terhitung, bukan hilang); nol waktu eksekusi
(tanpa PoC, tanpa fork); dibuka lagi hanya bila deployment live muncul
atau Kim memerintah cek ulang; wadah ditinjau ulang di sapuan akhir.

B9. Scope mapping cukup usaha yang cukup — miss bisa terdeteksi di
eksplorasi lewat: (1) baca ulang mendalam per klaster; (2) cek handoff
+ konservasi (delta tak terjawab diselidiki); (3) jejak event live
sebagai enumerasi independen; (4) cek penutupan graf. Penopang wajib:
sapuan akhir dengan enumerasi independen kedua dibanding ledger
akhir. Caranya masih terbuka — bukan AST (diputus 15 Nov 2026) dan
bukan regex yang sama (terbukti cacat di pilot). Sensus di
depan adalah rencana + daftar periksa, bukan bukti kelengkapan;
buktinya datang dari eksplorasi + sapuan akhir. Diputus 22:33 WIB 16 Nov 2026: rerun sensus manual DITERIMA sebagai enumerasi independen kedua (pilot: rerun menemukan 15 baris yang sensus pertama lewatkan). Cara parser independen tetap terbuka sebagai riset, bukan rule.

B10. Ledger "sisa" sesudah tiap round (entrypoint belum masuk alur,
handoff terbuka, klaster belum dikerjakan). Round selesai hanya bila
sisa menyusut. Target buruan harus ada di sensus — kalau tidak, di
luar scope menurut konstruksi.

B11. Inventaris handoff dienumerasi dari batas unit (rantai linear N
unit → N−1 handoff), bukan dilabeli tangan. Simpul fork/join jadi unit
sendiri.

B12. Invariant TIDAK dinyatakan di Tahap B atau Tahap C (direvisi
15 Nov 2026 atas perintah Kim — menggantikan 'invariant DINYATAKAN
dulu'; pemetaan mendalam pindah ke Tahap C, 15 Nov 2026, perintah Kim
'nomor baru saja'): Tahap B hanya membangun yang riil/teramati untuk
seluruh protokol — sensus, klaster, daftar jahitan (B16). Tahap C
membangun yang riil/teramati per klaster — inventaris alur,
segmentasi, kontrak handoff deskriptif (jaminan produsen + asumsi
konsumen sebagaimana teramati di kode: nama+tipe+satuan field).
Invariant adalah klaim asumsi agen tentang apa yang SEHARUSNYA
berlaku — bukan fakta teramati — jadi dirumuskan di Tahap D
(eksplorasi), bersama kartu leadnya, dan diuji di Tahap E. Setiap
invariant di Tahap D WAJIB mencantumkan sumbernya: field kontrak
handoff yang bersangkutan, atau baris docs/NatSpec — bukan perasaan
agen. Tiga lapis tetap: L1 dalam segmen (INV-Sx), L2 handoff + simpul
(INV-Hx, INV-Jx), L3 ujung-ke-ujung/komposisi (INV-Fx). Tiap invariant
wajib akibat finansial bila pecah. Cek testability dicatat saat
perumusan (fork apa, data live apa; dilabeli bila tidak bisa diuji).
Registry invariant tetap satu penomoran global (B3), diisi mulai Tahap D.

B13. Scope lock sebelum sensus jalan: daftar berkas scope harus cocok
persis dengan inventaris kanonik (diff kosong), commit terverifikasi,
contracts/ bersih.

B14. Konvensi sensus statis (15 Nov 2026, diputus asisten atas
delegasi Kim — berlaku universal, bukan cuma Across):
- Getter publik = entrypoint (ya). Entrypoint = bisa dipanggil dari
  luar (literal, mekanis); filter nilai lewat value_moves; yang
  mengikat alur tetap EP+VM.
- Receive badan kosong = value_moves no. Menerima saja bukan
  memindahkan; value_moves dinilai dari badan — ada perpindahan
  lanjutan (transfer keluar / perubahan saldo pihak lain) → yes;
  hanya menerima → no; tidak bisa dipastikan → unknown.
- Fungsi payable yang memanggil sistem eksternal yang diketahui
  (mis. relayMessage adapter → messenger bridge) = value_moves yes;
  tahap alur WAJIB verifikasi penerusan value per adapter dari badan
  — bila tidak meneruskan, turunkan ke no.
- Dispatcher dengan target/calldata arbitrer (Multicall3 aggregate*,
  deployAndExecute, directWithdraw, dsb.) = value_moves unknown;
  diselesaikan transitif di inventaris alur (ikuti ke
  implementasi/target).
- Getter auto-generated DIBARISKAN (konsisten dengan butir pertama):
  semua getter publik dibariskan (entrypoint yes, value_moves no).
- Entrypoint warisan TIDAK dibariskan (satu baris per definisi, di
  berkas pendefinisi). Tahap alur WAJIB punya langkah perluasan
  permukaan lewat implementasi/warisan sebelum segmentasi.
- Atribut berkas direct/proxy dicatat dua lapis: permukaan
  milik-sendiri + permukaan warisan. Inventaris alur memakai lapis
  warisan untuk permukaan deployed, lapis milik-sendiri untuk lokasi
  kode.

B15. Cek onchain: sensus live ringan B7 WAJIB di Tahap B; cek
onchain mendalam WAJIB di B8 (15 Nov 2026, direvisi atas perintah Kim
'perlu sensus live ringan' — menggantikan revisi 15:11 yang membuat B7
opsional): Tahap A TIDAK cek onchain (A2 — brief klaim saja). Tahap B
wajib B7 ringan — getCode 1–3 alamat inti di blok referensi + satu view
murah, rantai utama (fallback rantai utama kedua), anggaran 5 menit —
sebagai konfirmasi live bahwa klaim INTEL/deployment benar, sumber data
EV, dan sinyal live untuk identifikasi klaster. Sensus live PENUH
(semua rantai, jejak event lengkap) tetap OPSIONAL di Tahap B; cek
onchain mendalam WAJIB di B8: live-match per klaster sebelum PoC
ditulis; yang tidak live-match masuk TUNDA-LIVE; PoC klaster itu
TERBLOKIR sampai cek live-match ada. Pengecualian: proyek
TESTNET/DEVNET — "live" = lingkungan itu sendiri (A3).

B16. TIDAK DIPAKAI (diputus Kim 22:33 WIB 16 Nov 2026 — 'B16 tidak perlu'). Mekanisme di bawah ini TIDAK menjadi rule; dibiarkan sebagai arsip usulan: jahitan (handoff antar-klaster) dienumerasi UTUH di depan
(Tahap B mapping), dari graf alur — bukan ditemukan belakangan per
klaster. (1) Sensus mencatat, untuk tiap fungsi EP+VM, tepi
keluarnya: fungsi/kontrak apa yang dipanggil, token apa yang
dipindah, event apa yang diemisikan. (2) Klaster diidentifikasi DARI
graf itu, bukan dilabeli tangan dari nama kontrak: satu klaster =
satu tahap pipa alur; draf klaster dari nama kontrak hanya hipotesis
awal yang dikoreksi graf. (3) Daftar jahitan = semua tepi
antar-klaster, masing-masing dengan ID (SM-xx), klaster produsen,
klaster konsumen, bukti (baris sensus + baris kode), dan status.
Status di ledger sisa: dienumerasi → produsen terverifikasi →
konsumen terverifikasi → TERTUTUP; jahitan tertutup hanya bila kedua
sisi terverifikasi. Jejak event live (B7/B8) menambah jahitan
berbasis event (aktor off-chain yang menyambung klaster) dan
mengonfirmasi tepi statis mana yang benar-benar menyala live; tepi
statis yang tidak pernah live dicatat statis-saja, bukan dihapus.
Lead/PoC yang jalurnya melewati jahitan terbuka WAJIB melabeli
jahitan itu sebagai asumsi terbuka. Urutan klaster default =
hulu→hilir (topologis): klaster berjalan sesudah produsennya,
supaya jahitan masuknya sudah terverifikasi sisi produsen saat
klaster itu jalan; EV live memecah seri di antara klaster yang
sejajar. (Bila diputus, ini menggantikan default EV di B2: EV jadi
pemecah seri, bukan urutan utama.) Primitif bersama
(kontrak/library/token yang dipakai banyak klaster) = klaster
bersama sendiri, diinventarisasi sekali dan dirujuk semua konsumen;
bila belum diinventarisasi, konsumen mencatat jahitannya terbuka
dan mengutip interface primitif dari sensus sebagai asumsi. Cek
penutupan akhir (B9) memverifikasi semua jahitan TERTUTUP. Biaya:
sensus sedikit lebih dalam untuk fungsi EP+VM (waktu sensus perlu
diukur ulang — angka 13m36s rerun tidak memuat field 'keluar').

## Tahap B — yang wajib selesai (15 Nov 2026, perintah Kim)
Uji pemisahnya: Tahap B wajib menyelesaikan semua yang dipakai tahap
berikut sebagai INPUT dan tidak bisa direkonstruksi tahap berikut
tanpa baca ulang seluruh protokol. Yang tidak memenuhi uji itu bukan
pekerjaan Tahap B.
Wajib selesai di Tahap B:
1. Daftar scope + scope lock (B13) — gerbang masuk sensus.
2. Sensus statis 100% peristiwa pemindah nilai (B1/B6) — fondasi cakupan. (Field 'keluar' tiap fungsi EP+VM TIDAK wajib — B16 tidak dipakai, diputus 22:33 WIB 16 Nov 2026.)
3. Inventaris alamat live — daftarnya saja (B3); verifikasi onchain
daftar itu = B7/B8.
4. Sensus live ringan B7 (B15) — konfirmasi live klaim INTEL, sumber
EV, sinyal live identifikasi klaster.
5. Identifikasi klaster + daftar handoff (B11) — partisi kerja. (Daftar jahitan SM-xx lengkap TIDAK wajib — B16 tidak dipakai, diputus 22:33 WIB 16 Nov 2026.)
6. Ledger sisa — seed (B10): semua entrypoint, klaster, jahitan,
handoff terbuka; diperbarui sesudah tiap round.
7. Struktur registry invariant (B3) — penomoran + registry kosong;
diisi mulai Tahap D.
Bukan pekerjaan Tahap B (dikerjakan di tahap berikut): pemetaan
mendalam per klaster (Tahap C: inventaris alur + segmentasi + kontrak
handoff deskriptif), invariant (Tahap D), buruan (Tahap D), PoC + P&L
(Tahap E), komposisi (Tahap F), verdict (Tahap G), live-match mendalam
per klaster (B8, di giliran klaster), sensus live penuh (opsional), cek
penutupan graf akhir (B9 — dieksekusi di akhir audit; yang wajib ada di
Tahap B adalah bahannya: ledger sisa + daftar jahitan + sensus).

## Tahap B — yang TIDAK dilakukan (15 Nov 2026, perintah Kim: waktu
Tahap B harus efektif-efisien, jangan buang waktu ke yang seharusnya
dikerjakan spesifik di tahap berikut)
Uji pemisahnya: Tahap B hanya mengerjakan yang butuh pandangan
seluruh protokol; semua yang butuh konteks satu klaster saja
dikerjakan di giliran klaster itu (Tahap C/D/E).
Jangan dilakukan di Tahap B:
1. Inventaris mendalam semua klaster sekaligus (inventaris alur +
segmentasi + kontrak handoff penuh untuk semua klaster di depan) —
itu Tahap C per klaster, just-in-time. Alasan: 2–4 jam sebelum eksplorasi
apa pun; batas bisa bergeser sesudah klaster tetangga dibaca.
2. Menulis invariant — itu Tahap D (B12). Alasan: asumsi yang ditulis
sebelum eksplorasi akan basi dan ditulis ulang.
3. Buruan (mencari celah) saat memetakan — itu Tahap D. Alasan:
memperlambat pemetaan; lead tanpa sumber (handoff belum ada); bias
peta ke yang 'menarik'.
4. Menguji/mengeksekusi saat memetakan, termasuk memverifikasi
kontrak handoff dengan eksekusi — itu Tahap E. Kontrak handoff di
Tahap B bersifat deskriptif (apa kata kode).
5. Sensus live penuh (semua rantai, jejak event lengkap, verifikasi
semua alamat) — itu B8 per klaster. Di Tahap B cukup B7 ringan
(≤5 menit). Alasan: sebagian besar hanya dibutuhkan saat giliran
klasternya; state live berubah; biaya transport besar (pilot: 53/101
pemeriksaan gagal transport).
6. Baca mendalam semua fungsi saat sensus — sensus mengklasifikasi
(value_moves/entrypoint), bukan memahami logika tiap fungsi. Baca
mendalam hanya untuk fungsi EP+VM (tepi keluar, B16) + inventaris
per klaster (Tahap C).
7. Menyelesaikan semua 'unknown' saat sensus — unknown dicatat
sebagai unknown, diselesaikan di Tahap C klaster yang bersangkutan saat
konteks alurnya ada. Alasan: sebagian besar unknown (deklarasi
interface, dispatcher arbitrer) baru bisa diputus dengan konteks alur.
8. Menilai/menghakimi klaster saat memetakan ('klaster ini
kelihatannya bersih/berbahaya') — itu Tahap G (verdict). Peta harus
netral; penilaian dini membiasakan eksplorasi.
9. Rerun sensus penuh sebagai syarat tetap — rerun pilot adalah
validasi/pengukuran, bukan syarat metode. Kendali mutu sensus =
verifikasi parent atas hitungan + kontrol positif (B6); enumerasi
kedua yang independen = sapuan akhir B9, bukan sensus ulang di
Tahap B.
10. Menulis laporan akhir / memoles artefak — artefak Tahap B adalah
berkas kerja (baris JSON, daftar, ledger), bukan laporan poles.
11. Filter di DALAM klaster saat inventory (hanya data yang dibutuhkan
alur live) — dinilai BERISIKO oleh Kim (15 Nov 2026 18:22 WIB), tidak
diadopsi; larangan ini ditulis ke berkas ini 22:33 WIB 16 Nov 2026
atas perintah Kim ('larangan ditulis'). Filter yang aman hanya utuh
per klaster/alur.
Untuk proyek baru, tepi keluar EP+VM dicatat DALAM pass sensus
(field 'keluar' di baris sensus) — bukan pass terpisah; pass
enumerasi jahitan yang terpisah hanya catch-up untuk pilot Across
yang sensusnya sudah jalan tanpa field itu. (Koreksi 22:33 WIB
16 Nov 2026: field 'keluar' TIDAK wajib — B16 tidak dipakai. Pass
enumerasi jahitan terpisah tetap hanya catch-up pilot Across.)
Yang dikerjakan di
Tahap B: lihat seksi 'Tahap B — yang wajib selesai'.

## C. Preferensi yang mengikat (berlaku selama metode dijalankan)

C1. Fork-live: pengujian mengikuti kondisi live nyata — fork dibangun ke
kondisi live (kontrak ter-deploy + state live pada blok yang di-pin),
kecuali user sudah menjelaskan di awal proyek masih testnet/devnet.
Mekanisme penentu wajib diuji ulang di fork live bila fork-testable;
bila tidak fork-testable, catat eksplisit.

C2. Kejujuran bukti: label DIEKSEKUSI vs DIANALISIS vs
CODE-REVIEW-ONLY; klaim yang gugur dicabut sampai ke dokumennya;
koreksi disampaikan eksplisit.

C3. Verify_zero_and_scan: setiap nol = mencurigakan; simpan output
lengkap termasuk error; wajib kontrol positif bukan-nol lewat jalur
yang sama; pindai besar dibuktikan sensus (rentang + chunk + cakupan)
atau dilabeli SAMPEL persis; langkah belum dijalankan = langkah
terbuka, bukan rejection/finding; koreksi label di dokumen yang sama.

C4. Ambang $500 bukan ambang batas: apapun yang merugikan protokol /
sistem yang salah DISIMPAN sebagai kandidat berapapun kecilnya; floor
$500 hanya konteks eligibility. Tersimpan ≠ tersubmit.

C5. HOLD: nol submisi / kontak program tanpa persetujuan eksplisit per
kandidat; jangan buat draf laporan submisi tanpa instruksi eksplisit.

C6. Tiga verdict terpisah (mekanisme / eligibility / dampak); severity
dari plafon TERAMATI; insidensi wajib analisis same-tx; plafon
per-eksekusi tidak dikalikan tanpa bukti pengulangan.

C7. Bahasa: Indonesia ringan dan sederhana.

C8. Keep-hunting: tolak kesimpulan prematur "no vulnerability"; hasil
kecil berharga; tetapkan target lanjutan konkret, bukan menyatakan
permukaan bersih.

C9. /stop atau perintah berhenti eksplisit mencabut otorisasi round
seketika: hentikan subagent, tulis nol berkas, lanjutkan hanya atas
perintah eksplisit baru.

## D. Tahap C–G — modular (15 Nov 2026, perintah Kim: pisah modular seperti versi triangle; pemetaan mendalam jadi Tahap C, 15 Nov 2026, perintah Kim 'nomor baru saja')

Pemetaan huruf (15 Nov 2026, perintah Kim 'ubah tahap jadi huruf saja'): Tahap A = INTEL (dulu Tahap 0), B = Mapping (dulu Tahap 1), C = Pemetaan mendalam per klaster (dulu Tahap 2), D = Eksplorasi (dulu Tahap 3), E = Uji test (dulu Tahap 4), F = Komposisi (dulu Tahap 5, USULAN), G = Verdict (dulu Tahap 6). Output tiap tahap: `<huruf><NN>-<NAMA>`, mis. B01-SCOPE, C01-FLOW-INVENTORY. Penomoran aturan (A1–A5, B1–B16, C1–C9) dan SM/INV tidak berubah — namespace terpisah.

- Tahap B — Mapping: sensus + klaster + daftar jahitan + ledger sisa + registry (B1–B16). Tanpa invariant (B12), tanpa pemetaan mendalam, tanpa buruan/PoC/verdict.
- Tahap C — Pemetaan mendalam per klaster: inventaris alur + segmentasi + kontrak handoff deskriptif per klaster (B12, B16). Tanpa invariant, tanpa buruan/PoC/verdict.
- Tahap D — Eksplorasi: buruan per segmen/handoff/simpul; merumuskan invariant tiap kandidat (wajib cantumkan sumber: field handoff atau docs/NatSpec) + kartu lead (tiap kartu lead wajib klaim finansial sendiri). Tanpa PoC. Diputus 22:33 WIB 16 Nov 2026: unit round = per-alur, dua gelombang (buta → berpengetahuan + katalog 52 plugin); daftar alur (enumerasi + EV, dari B02+B04) adalah prasyarat sebelum D.
- Tahap E — Uji test: PoC fork-live + P&L terukur per kandidat; membuktikan/membantah mekanisme. Diputus 22:33 WIB 16 Nov 2026: bentuk rinci = yang sudah dipraktikkan (batch maks 3, fork blok pin + P&L terukur); verdict mengikuti bentuk yang sekarang (06/07/08/09).
- Tahap F — Komposisi (DITERIMA BERSYARAT, diputus Kim 22:33 WIB 16 Nov 2026: 'terima jika memang penting dan urgent'): HANYA untuk lead multi-segmen; F hanya dijalankan bila penting dan urgent. Uji F TIDAK menjalankan fork ulang dan TIDAK menjalankan PoC ulang — pakai data terbaru (hasil Tahap E); bukti komposisi = lemma (segmen terbukti + handoff cocok + simpul terbukti + jalur gabungan dirangkai dari data terbaru). Lead satu segmen LEWATI Tahap F (PoC Tahap E sudah final).
- Tahap G — Verdict: verdict per kandidat (06/07/08/09; tiga verdict terpisah; severity dari plafon teramati); distribusi akhir dihitung di seluruh protokol. Diputus 22:33 WIB 16 Nov 2026: bentuk rinci = yang sudah dipraktikkan (verdict mengikuti bentuk yang sekarang).

Rolling per klaster: Tahap C klaster → Tahap D klaster → Tahap E klaster → …; Tahap F lintas-klaster + Tahap G di akhir. Bentuk rinci Tahap D/E/G sudah diputus 22:33 WIB 16 Nov 2026 (lihat butir di atas); bentuk rinci Tahap C sudah terbukti di pilot (8 round). Deep-run per jalur (diputus Kim 22:33 WIB 16 Nov 2026): satu perintah berdiri hulu→hilir sampai verdict per lead, tanpa tap antar langkah; berlaku mulai jalur berikut (P4 — P3 sudah jalan step-by-step).

Just-in-time lintas-klaster (15 Nov 2026, perintah Kim; KOREKSI 15 Nov 2026 — dipindah dari Tahap C ke Tahap D atas pertanyaan Kim 'bukankah ini harusnya pas eksplorasi?'): materi terkait dan urgent dari klaster lain ditarik just-in-time di Tahap D (eksplorasi) — saat memburu satu alur, pemburu mengikuti alur itu melewati klaster, hanya yang dilewati alur itu. Di Tahap C, kerja per klaster; jahitan adalah batas (sisi produsen/konsumen yang klasternya belum dipetakan dicatat terbuka, dilengkapi di round klaster itu lewat siklus hidup B16). Alasan: kebutuhan tarik lintas-klaster ada di Tahap D, karena alur (unit buruan) tidak mengenal batas klaster; di Tahap C, tarikan dini berisiko menyeret penilaian (apakah handoff cocok?) ke dalam kerja deskriptif. Keputusan klaster yang dipenuhi: (2) round 1 Tahap C = K1 SAJA (loop K1+K2 diburu bersama di Tahap D, dirangkai di Tahap F); (3) K8 = round sendiri di Tahap C (diinventarisasi sekali, dirujuk konsumen; ditarik just-in-time di Tahap D bila alur yang diburu memakainya). Urutan EV (K1 dulu) tetap sebagai prioritas, bukan pagar.

## Diputus 16 Nov 2026 (22:33 WIB)

Diputus Kim atas daftar 10 butir yang belum diputus:
1. B16 TIDAK DIPAKAI ('B16 tidak perlu') — mekanisme jahitan lengkap + urutan hulu→hilir + field 'keluar' tidak menjadi rule; urutan klaster default tetap EV (B2); daftar jahitan pilot tetap artefak eksekusi.
2. Tahap F DITERIMA BERSYARAT ('terima jika memang penting dan urgent') — isi F diterima; F hanya dijalankan bila penting dan urgent.
3. Tahap D: unit round = per-alur, dua gelombang (buta → berpengetahuan + katalog 52).
4. Tahap E/G: bentuk rinci = yang sudah dipraktikkan; verdict mengikuti bentuk yang sekarang; uji F TIDAK menjalankan fork ulang dan TIDAK menjalankan PoC ulang — pakai data terbaru.
5. B9: rerun sensus manual DITERIMA sebagai enumerasi independen kedua (YA).
6. A5: dua celah DITERIMA (YA) — pin HEAD saat penemuan; blok referensi ditentukan di Tahap B.
7. Arah metode: 'pakai paling mutakhir' — flow money first menjadi metode kerja aktif; Segitiga menjadi rujukan/legacy.
8. Deep-run per jalur DITERIMA — mode eksekusi per jalur; berlaku mulai jalur berikut (P4).
9. Larangan filter di dalam klaster DITULIS ke berkas ini (butir 11 seksi 'Tahap B — yang TIDAK dilakukan').
10. SM-05 (pilot): tetap dieksplorasi ketika terkait — konsumennya tidak dipatok sekarang.

## Belum diputus (bukan rule)

- Inventory per-alur (Tahap C) — dievaluasi, tidak diadopsi untuk sekarang; bisa dibuka lagi.
- Cara parser independen untuk sapuan akhir (B9) — terbuka sebagai riset (rerun sensus manual sudah diterima sebagai enumerasi kedua, 22:33 WIB 16 Nov 2026).
- Skill per tahap A–G + `verify_flow.py` — item pembangunan repo (bukan rule); bentuk rinci Tahap D–G sudah diputus sehingga skill kini tidak lagi terblokir bentuk.

## Diputus tidak dipakai

- Baca bertingkat untuk sensus statis (DITOLAK 15 Nov 2026, keputusan
  Kim): mengganti baca manual dengan classifier AST + baca sebagian.
  Alasan: Kim tidak mau ambil risiko pada scope mapping statis;
  classifier belum ada dan belum tervalidasi. Yang dipakai: B6 (baca
  manual penuh, tanpa AST).
- Penghitung AST sebagai cek kedua sensus statis (DITOLAK 15 Nov 2026,
  keputusan Kim: "tidak usah pakai AST kalau begitu"): sempat dibangun
  dan divalidasi terhadap sensus pilot (1.245 vs 1.230 + 15, 9/9
  kontrol, 6/6 hantu bersih, deterministik) tapi tidak dipakai.
  Berkasnya tetap di folder pilot sebagai referensi, bukan bagian
  dari metode. Konsekuensi: B9 (sapuan akhir) butuh cara enumerasi
  independen lain — masih terbuka.
