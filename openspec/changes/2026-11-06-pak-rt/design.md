# Design — PAK RT!

## Stack

- **Web artifact statis** (single-file HTML + CSS + JS, client-side only). Tanpa build step, tanpa backend.
- Font: Outfit (judul) + Work Sans (isi) via Google Fonts.
- Suara: WebAudio API (oscillator blip, tanpa file audio).
- Penyimpanan: localStorage lewat satu modul `store` (satu-satunya akses localStorage).

## Arah visual (dari ui-ux-pro-max `--design-system`, dipadukan dengan craft-floor impeccable)

- Palet: oranye playful `#F97316` (primer) + biru tepercaya `#2563EB` (aksen) di atas krem hangat
  `#FFF7ED`. Kartu putih, teks cokelat tua `#9A3412` untuk judul kartu, teks gelap untuk isi.
- Dunia visual: "papan pengumuman RT" — kertas krem, stempel merah miring, garis putus-putus seperti
  sobekan kertas, lencana "RT 05" seperti papan nama gang. Tanpa gradient teks, tanpa kicker/eyebrow,
  tanpa kartu-ikon-generik-berderet.
- Tipografi: Outfit 700 untuk judul & angka besar, Work Sans 400/600 untuk isi. Skala jelas.
- Motion: satu momen authored — kartu masuk dengan geser + sedikit rotasi (seperti kartu diletakkan di meja),
  stempel "DISETUJUI" jatuh dengan scale + rotate; meter beranimasi lebar (width transition). Semua
  dimatikan bila `prefers-reduced-motion`.
- Ikon: SVG inline buatan sendiri (dompet, hati/dua tangan, lencana, bulan, palu). Tanpa emoji sebagai ikon.
  (Emoji boleh muncul di dalam teks narasi warga secukupnya? Tidak — patuhi aturan: tanpa emoji di UI.
  Narasi memakai kata-kata.)

## Arsitektur (ikut ARCHITECTURE.md §1 — web artifact statis)

```
index.html   # struktur + mounting point
styles.css   # design token :root → komponen → responsive
app.js       # state → render → events
```

Pola kode JS baku:

```js
// 1. STATE — satu objek state
const state = { screen:'menu', day:1, kas:500000, rukun:70, reputasi:70, ... };
// 2. DATA — EVENTS[], NIGHT_EVENTS[], ACHIEVEMENTS[] terpisah dari logic
// 3. STORE — satu modul localStorage (best, history, achievements, sound, save)
// 4. RENDER — fungsi murni state → DOM per layar (renderMenu/renderGame/renderEnd)
// 5. EVENTS — handler tipis: pilih → applyEffects → render → cek kalah/menang
// 6. INIT — muat save, render()
```

- IIFE/module scope, tidak ada namespace pollution global.
- `querySelector` di-cache di awal; tidak ada logic bisnis di event handler (extract ke fungsi bernama).
- Tidak ada file >300 baris per modul logis — `app.js` dipecah secara konseptual: data event di
  `events.js` terpisah dari `app.js` (dua file JS, keduanya di-load di `index.html`).
- Tidak ada god component: kartu, meter, kalender, toast, stempel masing-masing fungsi render sendiri.

## State — lima keranjang (ARCHITECTURE.md §2)

1. **Lokal** — state permainan berjalan (`day`, `kas`, `rukun`, `reputasi`, kartu aktif, fase layar).
2. **URL** — tidak dipakai (bukan state shareable).
3. **Server state** — tidak ada (tanpa backend).
4. **Global client UI** — toggle suara, toast reaksi (UI-only, bukan data permainan).
5. **Form** — tidak ada form; nama pemain opsional tidak dipakai (identitas = "Pak RT").

## Mesin event

- Pool harian: array `EVENTS` (≥28). Di awal run, pool di-shuffle (Fisher–Yates); tiap hari ambil 1 kartu
  dari depan. Habis → reshuffle sisa (tidak mungkin dalam 30 hari karena pool ≥28 + event mingguan).
- Event malam: peluang 25% tiap malam mulai Hari 3, dari pool `NIGHT_EVENTS` (3 kartu), muncul sebagai kartu
  kedua di hari yang sama (setelah kartu harian selesai).
- Event mingguan: Hari 7/14/21/28 → kartu laporan iuran (+Rp 150.000, tanpa pilihan, tombol "Terima").
  Hari 15 → kartu kerja bakti (pilihan).
- Efek pilihan: `{ kas, rukun, reputasi, reaction, stamp, flags }`. `flags` dipakai untuk achievement
  (mis. `flags.noisyPartyRejected`).
- Cek kalah setelah tiap efek diterapkan; cek menang setelah Hari 30 selesai.

## Skor & peringkat

`skor = rukun + reputasi + min(kas, 1.000.000) / 10.000` (maks 300).
Peringkat: ≥190 Legenda RT 05 · ≥160 RT Teladan · ≥120 RT Lumayan · <120 RT Pas-pasan.

## Animasi/interaksi (per bagian)

- Layar mulai: judul besar + lencana "RT 05" bergoyang pelan, tombol mulai berdenyut halus.
- Kartu kejadian: masuk dari bawah + rotasi -2deg → 0, bayangan lembut ber-offset.
- Stempel: muncul scale 2 → 1 dengan rotate -12deg + opacity, warna merah/biru sesuai nada.
- Meter: transisi width 400ms ease-out; denyut saat kritis.
- Toast reaksi: slide-up dari bawah kartu.
- Menang: confetti 60 partikel jatuh (div absolut, animasi CSS keyframes, dihapus setelah selesai).
- Suara: blip pilih (660Hz, 60ms), blip buruk (180Hz), fanfare menang (3 nada naik).

## Verifikasi

- Tiap SHALL di spec dilacak ke kode (checklist di `tasks.md`).
- Simulasikan alur: mulai → 30 hari (pilih acak) → menang; dan run kalah (pilih boros) → kalah; lanjutkan
  save; cek localStorage.
- Cek 360px tanpa scroll horizontal; cek kontras; cek reduced-motion.
