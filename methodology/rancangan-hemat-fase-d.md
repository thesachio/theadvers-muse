# Rancangan Hemat Fase D — DIPUTUS (16 Nov 2026)

Status: DIPUTUS 16 Nov 2026 (perintah Kim 15:10 WIB) — 8 tuas hemat jadi RULE untuk aksi berikut, mendampingi aturan brief bahasa verifikasi invariant (12:49 WIB 16 Nov). Berlaku mulai aksi berikut, bukan retroaktif ke batch yang sedang jalan. Efisiensi apa pun harus menjaga bukti ganda dan mutu pembuktian, bukan memangkasnya diam-diam. Bagian dari trace kekurangan runut (dimulai dari pre-0).

Prinsip efisiensi waktu (16 Nov 2026, perintah Kim, untuk ke depan): manfaatkan waktu sebaik-baiknya, gunakan seefektif-efisien mungkin. Bila satu tes penentu (mis. replay fork) sudah bisa mengonfirmasi, jangan jalankan pindai sejarah panjang dulu — pindai hanya sebagai konteks pendukung, dibatasi (jendela 10.000 blok seperti B03) atau lewat indexer.

Lantai bawah / floor (16 Nov 2026, perintah Kim): yang tidak boleh dipangkas — baca kode, ukur live, verifikasi parent atas bukti penentu. Segala penghematan hanya boleh memotong di atas lantai ini.

## Kenapa fase D lama (sudah teramati)

- Dua bukti per lead: PoC HEAD + fork live di blok yang di-pin + deploy_parity per lead. RPC publik lambat / 403 (contoh: klaim "0 log dalam 10.000 blok" tidak bisa direproduksi, `cast logs` 403 archive-gated).
- Parent menjalankan ulang semua tes forge + mencocokkan gas (R4-B01 13/13, R7-B01 8/8, R7-B03 9/9). Waktu dobel, dan cuma ada satu parent.
- Batch berurutan, maks 3 lead (R_7: B01/B02/B03).
- Gate D sering lolos di percobaan ke-2 (R7-B01, R7-B02, R_2 B01) karena format atau jangkar.
- Jalan buntu fork-testability ketahuan belakangan (precompile HyperCore tidak tersaji di fork Foundry; butuh leaf live yang tidak ada).
- Tiap lead itu mini-investigasi: 3 titik bukti + label M/AF + muat plugin + delta nilai + invariant.

## Tuas hemat — tanpa ubah metode (parent-side)

1. **Satu fork per wilayah, dipakai ulang.** State di blok yang di-pin tidak berubah. Bangun satu anvil fork per wilayah; semua batch dan rerun parent pakai fork yang sama. Memotong waktu bangun fork dan tarik state live berulang kali.
2. **deploy_parity sekali per wilayah, cache + pakai ulang.** Hasilnya (codesize, selector, VERSION, owner) tidak berubah di blok yang di-pin. Bacaan live lain yang sama antar lead juga begitu.
3. **Satu harness, dua mode jalan.** Tulis PoC sekali dengan saklar fork (mode HEAD + mode fork), bukan dua harness terpisah. Agen menulis sekali, menjalankan dua kali.
4. **Triage fork-testability di awal D.** Tepat setelah deploy_parity, cek cepat: butuh precompile? butuh leaf live yang tidak ada? butuh signer off-chain? Kalau ya, putuskan jalurnya sebelum satu batch penuh habis (fail fast).
5. **Pipeline, bukan paralel buta.** Agen mulai kerjakan B02 (bangun fork + parity + tulis PoC) selagi parent memeriksa B01. Waktu kalender turun tanpa mengubah bukti. Yang di-pipeline itu kerja mekanis, bukan verdict.
6. **Otomatiskan cek mesin.** Perbandingan gas, cek jangkar, bacaan nilai live lewat skrip — lebih cepat dan lebih teliti dari cek tangan. Pelajaran R_12: jangkar meleset +1..+25 baris, skrip menangkap itu seketika.

## Tuas hemat — perlu ubah upstream (verify_segitiga.py)

7. **Pre-check murah sebelum gate D.** Cek cepat lokal tanpa RPC: jangkar dalam batas baris berkas, label M/AF ada, tanpa token kompresi, manifes ≤16 baris. Gagal ketahuan dalam detik, bukan satu putaran penuh.

## Opsi bersyarat

8. **Parent cek penuh hanya untuk bukti penentu** — jangkar kartu yang naik lead, gas PoC, nilai live. Sisanya cek acak dengan aturan tertulis. Mutu tetap untuk yang menentukan verdict; yang dikurangi hanya cek ulang hal yang tidak mengubah verdict. Ini satu-satunya tuas yang menyentuh kedalaman pemeriksaan, jadi ditandai terpisah.

## Yang jangan dipangkas

- Pembuktian fork live.
- Pemeriksaan parent atas bukti penentu.
- Sensus 100%.
- Jangan saring di HEAD dulu (H-c2: live masih build pra-fix, saringan HEAD akan membunuhnya).
