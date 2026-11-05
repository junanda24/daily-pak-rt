# Proposal — PAK RT! Simulasi Jadi Ketua RT

**Tanggal:** 2026-11-06 · **Slug:** `pak-rt` · **Jenis:** game (web app statis, frontend-only, tanpa backend) · **Repo:** `junanda24/daily-pak-rt`

## Apa ini

Game simulasi keputusan: kamu baru terpilih jadi Ketua RT 05. Bertahan **30 hari** masa bakti simulasi dengan
menghadapi laporan warga yang datang silih berganti — ayam tetangga berak di halaman orang, pohon mangga yang
buahnya jatuh ke rumah sebelah, hajatan sampai jam 2 pagi, iuran nunggak, jadwal ronda yang selalu kosong,
sampai maling yang kepergok warga. Setiap keputusan menggeser tiga sumber daya: **Kas RT** (rupiah),
**Kerukunan** (0–100), dan **Reputasi** (0–100). Satu saja ambruk = jabatan tamat. Selesai 30 hari = kamu
dinilai, dari "RT Gagal Total" sampai "Legenda RT 05".

## Kenapa ide ini mengejutkan

- **Nggak ada yang bikin game jadi Ketua RT.** Semua orang Indonesia kenal sosok Pak RT — tukang stempel
  surat, penengah ribut tetangga, korban pertama kalau ada masalah — tapi belum ada yang mengubahnya jadi game.
  Riset pagi ini: tugas RT asli (Permendagri 18/2018, UU 3/2024) memang seluas itu — mediasi, siskamling,
  iuran, kerja bakti, surat pengantar — dan kultur pop-nya sudah ada (acara komedi "RT 5" dengan Cak Lontong
  sebagai Pak RT, sketsa viral pohon mangga, drama jadwal ronda). Game ini mengambil dunia yang 100% familier
  lalu membaliknya: kamulah yang harus memutuskan, dan wargalah yang menilai.
- **Komedi yang lahir dari kenyataan, bukan dari lelucon tempelan.** Semua event ditulis dari situasi RT nyata
  yang terverifikasi lewat riset (iuran, siskamling, kerja bakti, hajatan, surat pengantar, mediasi), bukan
  karangan bebas.
- **Reigns-nya kampung halaman.** Mekanik pilih-kartu-keputusan ala Reigns, tapi dengan rasa Indonesia:
  nama warga, bahasa sehari-hari, dan konsekuensi yang terasa (kas defisit, warga demo, dicopot RW).

## Kenapa bentuk output ini

Game web statis (HTML5 + JS, artifact) — langsung bisa dimainkan di HP tanpa install, tanpa login, tanpa
server. Mekaniknya murni client-side (acak kartu, hitung sumber daya, simpan skor), jadi backend tidak
dibutuhkan dan tidak dipakai. High score + riwayat jabatan tersimpan di localStorage.

## Kenapa bukan backend

Tidak ada data yang harus disinkronkan antar-pemain, tidak ada logika berat di server. Semua state hidup di
browser. Menambah backend hanya menambah biaya tanpa menambah rasa.

## Uji bar scope (2026-11-03)

Bukan utilitas satu fungsi: ada mesin event (~30 kartu keputusan + event malam + event mingguan), 3 sumber
daya yang saling tarik-menarik, sistem akhir (menang/kalah + peringkat), pencapaian (achievement), riwayat
jabatan lintas permainan, dan alur end-to-end 30 hari. Sekelas JUKIR!/TAWAR!/TEBAK HARGA!, bukan kata-angka.

## Fitur inti

1. Kartu keputusan harian — 1 event per hari, 2–3 pilihan, reaksi warga setelah memilih.
2. Tiga sumber daya: Kas RT (Rp), Kerukunan, Reputasi — dengan animasi naik/turun dan peringatan kritis.
3. Kalender 30 hari + event mingguan (iuran masuk tiap Minggu, kerja bakti, laporan kas).
4. Event malam dadakan (maling, banjir, listrik mati) dengan pilihan cepat.
5. Sistem akhir: menang di hari 30 (peringkat berdasarkan statistik) atau kalah (kas bangkrut / kerukunan 0 / reputasi 0).
6. Pencapaian (6 achievement) + papan riwayat jabatan (semua run tersimpan, skor terbaik).
7. Momen delight: stempel "DISETUJUI / DITOLAK" di kartu, toast reaksi warga, confetti saat menang, efek suara
   mungil (WebAudio, bisa dimatikan).

## Anti-AI-slop

Copy Bahasa Indonesia natural, nama warga dan situasi spesifik (bukan "warga mengeluh tentang masalah"),
tidak ada lorem ipsum, tidak ada placeholder. Semua aset visual digambar via CSS/SVG sendiri — tanpa gambar
pihak ketiga, tanpa karakter berlisensi.

## IP check

Semua aset original (ikon SVG buatan sendiri, ilustrasi CSS). Tidak ada gambar/karakter berlisensi. Nama warga
fiktif. Tidak perlu disclaimer fan-made.

## Keputusan sadar (tidak dibuat)

- Tanpa mode multiplayer / papan skor online (butuh backend + login — di luar scope satu sesi).
- Tanpa suara rekaman (WebAudio synth saja).
- Tanpa peta/animasi jalan — fokus ke kartu keputusan, bukan simulasi spasial.
