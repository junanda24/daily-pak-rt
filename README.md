# PAK RT! — Simulasi Jadi Ketua RT

Game simulasi keputusan berbahasa Indonesia: kamu baru terpilih jadi **Ketua RT 05** dan harus bertahan
**30 hari** menghadapi laporan warga yang datang silih berganti — ayam tetangga berak di halaman orang, pohon
mangga yang buahnya jatuh ke rumah sebelah, hajatan sampai jam 2 pagi, iuran nunggak, jadwal ronda yang
selalu kosong, sampai maling yang kepergok warga jam 2 pagi.

Setiap keputusan menggeser tiga sumber daya: **Kas RT** (rupiah), **Kerukunan** (0–100), dan **Reputasi**
(0–100). Satu saja ambruk, jabatan tamat. Selesai 30 hari, kamu dinilai — dari "RT Pas-pasan" sampai
"Legenda RT 05".

## Kenapa ide ini dipilih

Semua orang Indonesia kenal sosok Pak RT — tukang stempel surat, penengah ribut tetangga, korban pertama
kalau ada masalah di lingkungan — tapi belum ada yang mengubahnya jadi game. Riset sebelum build memverifikasi
bahwa dunia ini memang sekaya itu: tugas RT asli (Permendagri 18/2018, UU 3/2024) meliputi mediasi,
siskamling, iuran, kerja bakti, dan surat pengantar; kultur pop-nya sudah ada (acara komedi "RT 5" dengan
Cak Lontong sebagai Pak RT, sketsa viral soal pohon mangga dan jadwal ronda). Game ini mengambil dunia yang
100% familier lalu membaliknya: kamulah yang harus memutuskan, dan wargalah yang menilai. Efek kejutnya:
"kok kepikiran sih, jadi Pak RT dibikin game".

## Fitur

- **30 kartu kejadian harian** — semuanya ditulis tangan dari situasi RT nyata (ayam, mangga, hajatan, iuran,
  surat pengantar, ronda, sampah, karaoke, posyandu, banjir, lahiran tengah malam, duka warga, dan lain-lain),
  masing-masing dengan 2–3 pilihan dan konsekuensi yang masuk akal dengan narasinya.
- **3 kartu kejadian malam** (maling kepergok, banjir dadakan, listrik mati se-RT) — muncul acak 25% tiap
  malam mulai Hari 3.
- **Event mingguan deterministik** — iuran masuk +Rp 150.000 tiap Hari 7/14/21/28; kerja bakti besar Hari 15.
- **3 sumber daya yang saling tarik-menarik** — Kas RT (Rp, mulai 500.000), Kerukunan (70), Reputasi (70),
  dengan denyut merah saat masuk zona kritis.
- **Efek tersembunyi sebelum memilih** — seperti RT sungguhan, kamu menebak konsekuensi; delta dan reaksi
  warga baru terlihat setelah memilih.
- **3 alasan kalah yang spesifik** — kas bangkrut, warga demo (kerukunan 0), dicopot Pak RW (reputasi 0).
- **Skor + peringkat akhir** — skor = Kerukunan + Reputasi + min(Kas, 1.000.000)/10.000; peringkat Legenda RT 05
  / RT Teladan / RT Lumayan / RT Pas-pasan.
- **6 pencapaian** — Tangan Besi, Bendahara Jujur, Damai Selalu, 30 Hari Penuh, Siaga Malam, Lurah Pun Sungkan.
- **Persistensi localStorage** — skor terbaik, riwayat 10 jabatan terakhir, pencapaian lintas run, auto-save
  tiap hari + tombol "Lanjutkan Jabatan".
- **Momen delight** — stempel animasi DISETUJUI/DITOLAK/DITUNDA di kartu, toast reaksi warga, confetti saat
  menang, efek suara WebAudio (bisa dimatikan).
- **Mobile-first** — lebar maks 480px, tanpa scroll horizontal di 360px, tombol pilihan min 48px,
  `prefers-reduced-motion` dihormati, fokus keyboard terlihat.

## Cara main

1. Buka gamenya, tekan **Mulai Menjabat**.
2. Setiap hari, baca kartu kejadian dari warga, lalu pilih satu keputusan.
3. Perhatikan perubahan Kas, Kerukunan, dan Reputasi setelah memilih — lalu tekan **Lanjut Besok**.
4. Bertahan sampai Hari 30 tanpa satu pun sumber daya ambruk.
5. Lihat skor dan peringkatmu; ulangi untuk membuka semua pencapaian.

Tidak ada jawaban yang selalu benar: pilihan yang murah hati menguras kas, pilihan yang tegas merusak
kerukunan. Kuncinya menyeimbangkan ketiganya.

## Tech stack

- HTML + CSS + JavaScript murni (single-file, tanpa build step, tanpa framework, tanpa backend).
- Font Outfit + Work Sans (Google Fonts; game tetap jalan bila font gagal dimuat).
- Suara: WebAudio API (oscillator, tanpa file audio).
- Penyimpanan: localStorage (kunci `pakrt_save_v1`, `pakrt_best_v1`, `pakrt_history_v1`,
  `pakrt_ach_v1`, `pakrt_sound_v1`).
- Verifikasi: Playwright headless (Chromium) — simulasi run penuh.

## Struktur folder

```
2026-11-06-pak-rt/
├── README.md                      # dokumen ini
├── src/
│   └── index.html                 # game lengkap (single-file, hasil export artifact)
└── openspec/changes/2026-11-06-pak-rt/
    ├── proposal.md
    ├── specs/pak-rt/spec.md
    ├── design.md
    └── tasks.md
```

## Cara menjalankan dari source

Tidak butuh install apa pun. Buka `src/index.html` langsung di browser (atau sajikan lewat server statis
apa pun, mis. `python3 -m http.server`). Semua logika berjalan client-side; tidak ada panggilan jaringan
kecuali Google Fonts.

## Link hasil

- Game (artifact): slug `pak-rt` — kartu artifact `web-artifact-build-completed-8b30b27b-b151-46ef-81fd-9a402a368ecb`
- Repo: https://github.com/junanda24/daily-pak-rt
- Tanggal rilis: 2026-11-06

## Catatan/temuan saat build

- **Keseimbangan ekonomi ketat tapi bisa menang.** Simulasi headless: pemain yang selalu memilih opsi
  pertama (biasanya paling royal) bangkrut di Hari 20 (kas −Rp 60.000); pemain yang selalu memilih opsi
  terburuk digulingkan warga di Hari 6 (kerukunan 0). Jalur menang terverifikasi: run acak campuran menang
  dengan skor 213 ("Legenda RT 05"). Pesan desainnya: tidak ada satu tombol yang aman ditekan terus.
- **Verifikasi mandiri (Playwright, Chromium headless, viewport 390px):** 0 error konsol, 0 overflow
  horizontal (scrollWidth = 390), kartu iuran muncul Hari 7/14, kerja bakti Hari 15, event malam muncul
  1–4 kali per run, layar kalah bangkrut dan demo terlihat langsung dengan teks spesifiknya, layar menang
  terlihat langsung (skor 213, Legenda RT 05, pencapaian "Siaga Malam" + "30 Hari Penuh" terbuka),
  skor terbaik + riwayat jabatan tampil di menu setelah run selesai, tombol "Lanjutkan" memulihkan save
  (Hari 2, meter persis). Layar kalah ketiga (dicopot RW) terverifikasi lewat trace kode (`cekKalah()`
  memeriksa kas → kerukunan → reputasi, dengan teks "Dicopot oleh Pak RW!" di bundle) — tidak sempat
  dipicu live karena kerukunan hampir selalu jatuh lebih dulu.
- **Satu perbaikan selama build:** toast reaksi sempat menutupi tombol "Lanjut Besok" — diperbaiki dengan
  ruang gulir ekstra + tombol ter-scroll ke tengah + toast bisa ditutup dengan ketukan.
- **Yang disengaja tidak dibuat:** tanpa multiplayer/papan skor online (butuh backend + login); tanpa suara
  rekaman (WebAudio synth saja); tanpa peta/animasi jalan (fokus ke kartu keputusan).
- **IP:** semua aset original (ikon SVG buatan sendiri, ilustrasi CSS). Tidak ada gambar/karakter berlisensi.
  Nama warga fiktif.
