# Spec — PAK RT!

## Gambaran

Game simulasi keputusan berbasis kartu. Pemain adalah Ketua RT 05 yang baru terpilih. Permainan berjalan
30 hari (Hari 1–30). Setiap hari muncul 1 kartu kejadian dari warga; pemain memilih 1 dari 2–3 pilihan.
Setiap pilihan mengubah Kas RT (Rp), Kerukunan (0–100), dan/atau Reputasi (0–100). Permainan berakhir
menang di Hari 30, atau kalah lebih cepat bila ada sumber daya yang ambruk.

## Requirements

### R1 — Alur permainan

- **SHALL** permainan punya 4 layar: (a) layar mulai dengan judul, penjelasan singkat, tombol "Mulai Menjabat";
  (b) layar main (kalender hari, 3 meter sumber daya, kartu kejadian, pilihan); (c) layar akhir menang;
  (d) layar akhir kalah.
- **SHALL** setiap hari menampilkan tepat 1 kartu kejadian, kecuali hari dengan event mingguan/malam yang
  menempel sebagai kartu kedua pada hari yang sama.
- **SHALL** permainan berakhir menang saat pemain menyelesaikan Hari 30 (memilih keputusan hari ke-30).
- **SHALL** tombol "Main Lagi" di layar akhir memulai run baru dari Hari 1 dengan sumber daya awal.
- **SHALL** tombol "Lanjut Besok" / pilihan keputusan menjadi satu-satunya jalan maju — tidak ada jalan buntu.

### R2 — Sumber daya

- **SHALL** Kas RT dimulai Rp 500.000, ditampilkan sebagai rupiah (format Indonesia).
- **SHALL** Kerukunan dimulai 70, Reputasi dimulai 70 (skala 0–100).
- **SHALL** Kerukunan dan Reputasi di-clamp 0–100; Kas boleh turun sampai negatif (defisit).
- **SHALL** setiap pilihan menampilkan efeknya SETELAH dipilih (delta +/− pada meter + kalimat reaksi warga),
  bukan sebelumnya — pemain menebak konsekuensi seperti RT sungguhan.
- **SHALL** meter berubah warna dan berdenyut saat masuk zona kritis (Kerukunan/Reputasi < 25, Kas < Rp 100.000).
- **SHALL** kalah terjadi saat: Kas < Rp 0 (bangkrut), Kerukunan = 0 (warga demo), atau Reputasi = 0 (dicopot RW).
  **WHEN** kondisi kalah tercapai **THEN** layar akhir kalah muncul dengan alasan spesifik yang sesuai penyebabnya.
- **SHALL** tidak ada cara kalah di Hari 1 dari satu keputusan (efek satu kartu tidak cukup menjatuhkan meter
  dari nilai awal ke 0) — kecuali Kas bila pemain memilih pilihan paling boros berulang kali, itu disengaja.

### R3 — Kartu kejadian

- **SHALL** ada minimal 28 kartu kejadian harian yang berbeda, semuanya berbahasa Indonesia, masing-masing
  punya: judul, nama warga pelapor, narasi 1–2 kalimat, 2–3 pilihan, dan tiap pilihan punya efek + kalimat
  reaksi warga.
- **SHALL** kartu tidak berulang dalam satu run sampai seluruh pool habis (diacak, tanpa pengulangan).
- **SHALL** ada minimal 3 kartu "event malam" (maling, banjir, listrik mati) yang bisa muncul acak di malam hari
  sebagai kartu tambahan dengan pilihan cepat.
- **SHALL** ada event mingguan deterministik: tiap Hari 7/14/21/28 iuran masuk (+Rp 150.000, ditampilkan sebagai
  kartu laporan, bukan pilihan), dan Hari 15 kerja bakti (pilihan: ikut terjun / kirim konsumsi / pura-pura sibuk).
- **SHALL** semua efek kartu masuk akal dan konsisten dengan narasinya (tidak ada efek acak tanpa kaitan).

### R4 — Penilaian akhir & pencapaian

- **SHALL** layar akhir menang menampilkan peringkat berdasarkan skor akhir:
  skor = Kerukunan + Reputasi + min(Kas, 1.000.000)/10.000.
  Peringkat: ≥190 "Legenda RT 05", ≥160 "RT Teladan", ≥120 "RT Lumayan", <120 "RT Pas-pasan".
- **SHALL** ada minimal 6 pencapaian (achievement) yang bisa dibuka lintas run, mis. "Tangan Besi"
  (menolak 3 hajatan berisik dalam satu run), "Bendahara Jujur" (Kas ≥ Rp 1.000.000 di akhir),
  "Damai Selalu" (Kerukunan 100 di akhir), "30 Hari Penuh" (tamat), "Siaga Malam" (selesaikan 2 event malam),
  "Lurah Pun Sungkan" (Reputasi 100 di akhir).
- **SHALL** pencapaian yang terbuka ditampilkan di layar akhir dan di layar mulai (galeri kecil).

### R5 — Persistensi (localStorage)

- **SHALL** skor terbaik (skor tertinggi + peringkat + tanggal) tersimpan di localStorage dan tampil di layar mulai.
- **SHALL** riwayat jabatan (maks 10 run terakhir: tanggal, hari tercapai, skor, peringkat, alasan kalah/menang)
  tersimpan di localStorage dan bisa dilihat di layar mulai.
- **SHALL** pencapaian yang terbuka tersimpan lintas run.
- **SHALL** tombol "Lanjutkan" muncul di layar mulai bila ada run yang belum selesai (auto-save tiap selesai hari);
  **WHEN** pemain menekan "Lanjutkan" **THEN** run dilanjutkan dari hari terakhir yang tersimpan.

### R6 — Momen delight & rasa

- **SHALL** setiap keputusan memunculkan stempel animasi di kartu ("DISETUJUI" / "DITOLAK" / "DITUNDA" sesuai
  nada pilihan) sebelum kartu berganti.
- **SHALL** reaksi warga tampil sebagai toast/baris percakapan 1–2 detik setelah memilih.
- **SHALL** layar menang menampilkan confetti (CSS/JS, tanpa library eksternal).
- **SHALL** ada toggle suara (WebAudio blip sederhana, default nyala, tersimpan di localStorage).
- **SHALL** transisi kartu memakai animasi geser/fade (bukan pergantian instan 0ms).

### R7 — Mobile-first & aksesibilitas

- **SHALL** layout mobile-first, lebar konten maks 480px di tengah, tanpa scroll horizontal di 360px.
- **SHALL** semua tombol/pilihan minimal 44×44px, bisa dipakai dengan sentuhan.
- **SHALL** kontras teks ≥ 4.5:1; fokus keyboard terlihat; `prefers-reduced-motion` dihormati (animasi dimatikan).
- **SHALL** seluruh copy berbahasa Indonesia natural; tidak ada teks placeholder/lorem ipsum.

### R8 — Tanpa backend

- **SHALL** seluruh logika berjalan client-side; tidak ada panggilan jaringan kecuali font Google Fonts.
- **SHALL** tidak ada login, tidak ada data yang dikirim ke server mana pun.
