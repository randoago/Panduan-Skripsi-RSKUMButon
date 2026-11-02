---
title: "Panduan Penggunaan LaTeX untuk Penulisan Skripsi"
subtitle: "Program Studi Rekayasa Sistem Komputer, Fakultas Teknik, Universitas Muhammadiyah Buton"
---

# 1. Gambaran singkat

Semua dokumen (proposal, seminar hasil, skripsi, formulir judul) ditulis dalam **LaTeX**. Dari satu sumber yang sama Anda bisa menghasilkan:

- **PDF** (tata letak sesuai pedoman), dan
- **Word (.docx)** siap pakai yang disediakan di `00-output/` (untuk diedit atau diserahkan dalam format Word).

Anda **cukup mengedit berkas `.tex`**. PDF dibuat otomatis lewat menu, tidak perlu diatur ulang margin, spasi, atau nomor halamannya.

## Struktur folder

Folder berawalan `00-` bukan tahapan skripsi; `01` sampai `04` adalah tahapan yang berurutan.

```
Panduan-Skripsi-RSKUMButon/
├── menu.sh                menu pembuat PDF
├── 00-panduan/            panduan ini
├── 00-pedoman/            pedoman.pdf (hanya PDF)
├── 00-contoh/             contoh terisi: proposal, seminar-hasil, skripsi (+ output/)
├── 00-output/             hasil: *.pdf dan *.docx
├── 00-common/             gaya dan data bersama (jarang diubah)
│   ├── skripsi.sty, skripsi-bab.sty   tata letak
│   ├── skripsi-pustaka.sty            sitasi dan daftar pustaka APA 7
│   ├── indonesian-apa.lbx             istilah APA bahasa Indonesia
│   ├── pustaka.bib                    daftar sumber (dipakai semua dokumen)
│   ├── dosen.tex                      daftar dosen
│   └── peran.tex                      pembimbing, penguji, dan seterusnya
├── 00-gambar/             logo dan gambar
├── 01-formulir/           tahap 1: formulir pengajuan judul (formulir.tex)
├── 02-proposal/           tahap 2: proposal (proposal.tex, bab/)
├── 03-seminar-hasil/      tahap 3: seminar hasil (struktur sama dengan skripsi)
└── 04-skripsi/            tahap 4: skripsi
    ├── skripsi.tex        berkas utama (sampul, halaman depan, daftar bab)
    └── bab/               satu berkas per bab
```

# 2. Persiapan (sekali saja)

Dibutuhkan lima program (`biber` ikut terpasang bersama MacTeX). Periksa dengan:

```bash
./menu.sh cek
```

| Program | Fungsi | Cara memasang (macOS) |
|---|---|---|
| `xelatex`, `latexmk`, `biber` | membuat PDF (`biber` menyusun daftar pustaka dari `pustaka.bib`) | `brew install --cask mactex-no-gui` |
| `python3` | mengatur nama dosen (`./menu.sh peran`) | bawaan macOS |

Font **Times New Roman** dipakai bila tersedia; jika tidak, otomatis diganti *TeX Gyre Termes* yang bentuknya serupa.

# 3. Cara membuat PDF

## Lewat menu (disarankan)

```bash
cd panduan-skripsi
./menu.sh
```

Pilih: **1** Buat PDF, lalu pilih dokumen. Hasil ada di folder `00-output/`.

## Lewat perintah langsung

```bash
./menu.sh pdf skripsi        # PDF skripsi
./menu.sh pdf semua          # semua dokumen
./menu.sh buka skripsi       # membuka hasilnya
./menu.sh bersih             # menghapus berkas sementara
```

Nama dokumen: `proposal`, `formulir`, `seminar-hasil`, `skripsi`, atau `semua`.

## Membuat PDF tanpa menu

```bash
cd skripsi
TEXINPUTS="../00-common//:" BIBINPUTS="../00-common//:" latexmk -xelatex skripsi.tex
```

`TEXINPUTS` wajib agar LaTeX menemukan gaya di folder `00-common/`; `BIBINPUTS` agar `pustaka.bib` ditemukan. `latexmk` menjalankan `biber` sendiri.

## Membuat PDF dari editor (VS Code, TeXstudio, dll.)

Tiap folder dokumen sudah berisi berkas `latexmkrc` yang mengatur `TEXINPUTS`/`BIBINPUTS` dan memilih **XeLaTeX** otomatis. Jadi cukup **buka berkas utama** (`04-skripsi/skripsi.tex`, `02-proposal/proposal.tex`, dst.) lalu tekan tombol *Build* / *Compile*. Tidak perlu mengatur apa pun.

- Di **VS Code** pasang ekstensi *LaTeX Workshop*, buka berkas utama, lalu **Ctrl+Alt+B** (Cmd+Option+B). Pengaturan proyek sudah ada di `.vscode/settings.json`.
- Di **TeXstudio**, pilih *Tools → Commands → LaTeX* menjadi `latexmk` (bukan `pdflatex`) dan buka berkas utama.
- Selalu kompilasi **berkas utama**, bukan berkas di dalam `bab/`.
- PDF dari editor muncul di folder dokumennya (mis. `04-skripsi/skripsi.pdf`). Salinan rapi di `00-output/` dibuat lewat `./menu.sh`.

# 4. Struktur sebuah dokumen

Contoh `04-skripsi/skripsi.tex` (dipersingkat; proposal dan seminar hasil serupa):

```latex
\documentclass[12pt,a4paper]{report}
\usepackage{skripsi}          % font, bahasa, lingkungan khusus
\usepackage{skripsi-bab}      % margin, spasi, nomor halaman
\usepackage{skripsi-pustaka}  % sitasi & daftar pustaka APA (dari pustaka.bib)
\input{../00-common/dosen}       % data dosen
\input{../00-common/peran}       % peran dosen

\begin{document}
  ... sampul, halaman pengajuan, pengesahan, abstrak, kata pengantar ...
  \begin{daftarisi}\end{daftarisi}
  \begin{daftartabel}\end{daftartabel}
  \begin{daftargambar}\end{daftargambar}
  \begin{daftarlampiran}\end{daftarlampiran}   % skripsi dan seminar hasil
  \begin{akhirdepan}\end{akhirdepan}   % halaman isi mulai dari angka 1
  \pagestyle{isi}
  \input{bab/01-pendahuluan}
  \input{bab/02-tinjauan-pustaka}
  ...
\end{document}
```

Setiap bab ada di berkas sendiri di folder `bab/`:

| Berkas | Isi |
|---|---|
| `01-pendahuluan.tex` | Bab I |
| `02-tinjauan-pustaka.tex` | Bab II |
| `03-metode-penelitian.tex` | Bab III |
| `04-hasil-pembahasan.tex` | Bab IV |
| `05-kesimpulan-saran.tex` | Bab V |
| `06-daftar-pustaka.tex` | Daftar Pustaka |
| `07-lampiran.tex` | Lampiran |

**Menambah bab/lampiran baru:** buat berkas baru di `bab/`, lalu tambahkan `\input{bab/nama-berkas}` pada berkas utama di urutan yang diinginkan.

# 5. Mengedit isi

## 5.1 Judul bab dan subbab

```latex
\chapter{PENDAHULUAN}              % BAB I  (judul ditulis HURUF BESAR)
\section{Latar Belakang}           % 1.1
\subsection{Teori Pertama}         % 1.1.1
\subsubsection{Rincian}            % 1.1.1.1 (tebal, tegak)
\babtp{DAFTAR PUSTAKA}             % judul tanpa nomor, tetap masuk daftar isi
```

Nomor bab, subbab, dan daftar isi dibuat otomatis. Subbab ditulis dengan *Huruf Awal Kapital* setiap kata.

## 5.2 Paragraf dan teks

- Paragraf baru = **baris kosong** di antara dua paragraf. Jangan menekan spasi atau Tab untuk membuat alinea; menjorok 1,27 cm sudah otomatis.
- Cetak miring: `\textit{template}` (kata asing/Inggris/Latin ditulis miring, sesuai pedoman).
- Cetak tebal: `\textbf{penting}`.
- Kutipan: gunakan tanda kutip dua melengkung langsung, misalnya “seperti ini”.

## 5.3 Karakter khusus

Karakter berikut punya arti khusus di LaTeX, tulis dengan garis miring terbalik di depannya:

| Ketik | Hasil | Ketik | Hasil |
|---|---|---|---|
| `\%` | % | `\&` | & |
| `\#` | # | `\_` | _ |
| `\$` | $ | `\{ \}` | { } |

> **Peringatan:** tanda `%` yang tidak diberi `\` akan **menghapus sisa baris** (dianggap komentar). Tulis `5\%`, bukan `5%`.

Tanda `--` menghasilkan tanda hubung panjang (misalnya rentang halaman `264--269`).

## 5.4 Daftar (butir)

```latex
\begin{enumerate}        % bernomor 1. 2. 3.
\item Butir pertama.
\item Butir kedua.
\end{enumerate}

\begin{enumerate}[a.]    % huruf a. b. c.
\item ...
\end{enumerate}

\begin{itemize}          % butir berpoin
\item ...
\end{itemize}
```

## 5.5 Tabel

Judul tabel di **atas**, bentuk tabel **kotak penuh** (garis atas, bawah, kiri, kanan, dan antarbaris). Nomor otomatis mengikuti bab: tabel pertama di Bab IV bernomor **Tabel 4.1**, yang kedua **Tabel 4.2**, dan seterusnya (angka setelah titik adalah urutan di bab itu). Di lampiran nomornya berawalan nomor lampiran, mis. Tabel L2.1. Cara paling aman (dan terbaca baik di Word) memakai `longtable`:

```latex
\begin{singlespace}
\begin{longtable}{|p{0.32\linewidth}|p{0.28\linewidth}|p{0.28\linewidth}|}
\caption{Hasil pengujian sistem}\\
\hline
\textbf{Parameter} & \textbf{Hasil} & \textbf{Keterangan} \\
\hline
Latensi (ms) & 12,5 & Baik \\
\hline
Akurasi (\%) & 95,2 & Baik \\
\hline
\end{longtable}
\end{singlespace}
```

- Lebar kolom ditulis sebagai pecahan `\linewidth` (jumlah sekitar 0,9–1,0).
- Isi tabel diketik satu spasi (`singlespace`).
- Angka desimal memakai **koma** (`12,5`), ribuan dengan titik (`1.000`).
- Sebut tabel di teks: "Tabel 1 menunjukkan ...".

## 5.6 Gambar

Judul gambar di **bawah**, nomor otomatis dengan aturan yang sama (Gambar 1.1 = gambar pertama di Bab I; di lampiran Gambar L2.1). Simpan berkas gambar di folder `00-gambar/` (format `.png` atau `.jpg`).

```latex
\begin{figure}[H]
\centering
\includegraphics[width=0.6\textwidth]{../gambar/grafik-hasil.png}
\caption{Grafik hasil pengujian}
\end{figure}
```

`[H]` menempatkan gambar tepat di posisinya. Atur ukuran dengan `width=0.6\textwidth` (60% lebar teks).

## 5.7 Persamaan

```latex
$ds = \sqrt{r^{2} + \left(\frac{dr}{d\theta}\right)^{2}}\, d\theta$
```

Di Word, persamaan menjadi objek persamaan yang dapat diedit.

## 5.8 Sitasi dan daftar pustaka (gaya APA edisi ke-7)

Sitasi dan daftar pustaka **tidak diketik manual**. Alurnya tiga langkah:

1. **Catat sumber sekali** di berkas `00-common/pustaka.bib` (satu berkas dipakai bersama proposal, seminar hasil, dan skripsi, jadi sumber dari proposal otomatis tersedia lagi di skripsi).
2. **Panggil sumber di teks** dengan kuncinya (*key*).
3. **Kompilasi**. `latexmk` menjalankan `biber`, lalu daftar pustaka tersusun otomatis (urut abjad, baris kedua menjorok, hanya sumber yang dikutip). `./menu.sh` juga sudah mengurusnya.

### Memanggil sumber di teks

| Ketik | Hasil | Dipakai untuk |
|---|---|---|
| `\parencite{andrews2007}` | (Andrews, 2007) | sitasi di akhir kalimat |
| `\textcite{andrews2007}` | Andrews (2007) | nama penulis jadi bagian kalimat |
| `\parencite{xu2012}` | (Xu & Fischione, 2012) | dua penulis, tanda `&` otomatis |
| `\textcite{xu2012}` | Xu dan Fischione (2012) | dua penulis di dalam kalimat |
| `\parencite{capozzi2013}` | (Capozzi et al., 2013) | tiga penulis atau lebih |
| `\parencite{bps1997}` | (Badan Pusat Statistik, 1997) | lembaga sebagai penulis |
| `\parencite[15]{ghosh2010}` | (Ghosh et al., 2010, hlm. 15) | kutipan langsung dengan halaman |
| `\parencite{andrews2007,xu2012}` | (Andrews, 2007; Xu & Fischione, 2012) | beberapa sumber (diurutkan abjad otomatis) |
| `\parencite{panduan2020}` | (*Panduan penulisan karya ilmiah*, 2020) | sumber tanpa penulis |

Contoh kalimat: `Penjadwalan paket berperan penting \parencite{capozzi2013}. \textcite{andrews2007} menyatakan bahwa ...`

Bila kunci salah ketik, di PDF muncul tanda `?` atau `[andrews2007]` dan `biber` memberi peringatan; periksa ejaan kunci di `pustaka.bib`.

### Menulis entri di pustaka.bib

Setiap sumber satu entri: jenis, kunci, lalu field. Contoh buku dan artikel jurnal:

```bibtex
@book{ghosh2010,
  author    = {Ghosh, Arunabha and Zhang, Jun and Andrews, Jeffrey G. and Muhamed, Rias},
  title     = {Fundamentals of {LTE}},
  date      = {2010},
  publisher = {Prentice Hall},
}

@article{capozzi2013,
  author       = {Capozzi, Francesco and Piro, Giuseppe and Grieco, Luigi Alfredo and Boggia, Gennaro and Camarda, Pietro},
  title        = {Downlink packet scheduling in {LTE} cellular networks: Key design issues and a survey},
  journaltitle = {IEEE Communications Surveys \& Tutorials},
  date         = {2013},
  volume       = {15},
  number       = {2},
  pages        = {678--700},
}
```

Aturan penulisan field:

- Beberapa penulis dipisah kata `and`, bentuk `Nama belakang, Nama depan`.
- Singkatan atau huruf besar yang harus dipertahankan dibungkus kurung kurawal, misalnya `{LTE}`, `{DOI}`; tanpa kurung kurawal, judul dijadikan huruf kecil sesuai aturan APA.
- Lembaga sebagai penulis diberi kurung kurawal ganda: `author = {{Badan Pusat Statistik}}`.
- Tanda `&` ditulis `\&`; rentang halaman memakai `--`.
- Untuk sumber berbahasa Indonesia tambahkan `langid = {indonesian}`.
- Judul buku/jurnal dicetak miring otomatis; Anda tidak perlu `\textit`.
- Mendeley atau Zotero dapat mengekspor seluruh koleksi ke format **BibLaTeX** (`.bib`); tempel hasilnya ke `pustaka.bib`. Rapikan kuncinya agar mudah diingat (misalnya `penulis2020`).

**Contoh untuk berbagai jenis sumber** (semuanya ada di `pustaka.bib`; hapus yang tidak dipakai):

| Jenis | Jenis entri | Field penting |
|---|---|---|
| Buku | `@book` | author, title, date, publisher |
| Buku terjemahan | `@book` | author (penulis asli), `translator`, title, date, publisher |
| Bab dalam buku suntingan | `@incollection` | author, editor, title, booktitle, date, pages, publisher |
| Artikel jurnal | `@article` | author, title, journaltitle, date, volume, number, pages, `doi` |
| Prosiding | `@inproceedings` | author, title, booktitle, date, pages |
| Skripsi/tesis | `@thesis` | author, title, `type = {mathesis}`, institution, date, url |
| Laporan lembaga | `@report` | author (lembaga), title, `titleaddon = {Laporan}`, date, url, urldate |
| Lembar data | `@report` | author (produsen), title, `titleaddon = {Lembar data}`, `edition = {Versi 3.3}`, date, url |
| Artikel berita/blog | `@article` | author, title, `journaltitle` (nama situs), `entrysubtype = {nonacademic}`, date (`2026-03-05`), url, urldate |
| Situs web | `@online` | author/lembaga, title, url, urldate |
| Kode di repositori | `@software` | author, title, `titleaddon = {Kode sumber}`, publisher (GitHub), date, url, urldate |
| Tanpa penulis | `@book` | title, date, publisher (tanpa author) |

**Sumber web wajib mencantumkan tanggal akses**, yaitu hari saat sumber dibuka, karena isinya bisa berubah. Isi field `urldate` dengan format `2026-10-21`; daftar pustaka otomatis menulis *Diakses pada 21 Oktober 2026, dari alamat*. Artikel jurnal dengan DOI tidak perlu tanggal akses. Bila tahun tidak diketahui, kosongkan `date`; otomatis tertulis (t.t.).

### Di berkas daftar pustaka

Berkas `bab/04-daftar-pustaka.tex` (proposal) atau `bab/06-daftar-pustaka.tex` (seminar hasil, skripsi) cukup memuat:

```latex
\babtp{DAFTAR PUSTAKA}
\begin{daftarpustaka}
\printbibliography[heading=none]
\end{daftarpustaka}
```

Pada template, berkas ini juga berisi penjelasan dan baris `\nocite{...}` agar semua jenis contoh tampil. **Hapus penjelasan dan baris `\nocite` itu pada naskah Anda**, sehingga daftar pustaka hanya memuat sumber yang benar-benar dikutip. Untuk memaksa satu sumber tampil tanpa dikutip, gunakan `\nocite{kunci}`.

## 5.9 Lampiran

Tiap lampiran adalah judul tanpa nomor bab dengan format "LAMPIRAN n. judul":

```latex
\babtp{LAMPIRAN 2. Daftar pertanyaan wawancara}
Isi lampiran ...
```

# 6. Mengisi data sampul

Ganti teks berikut langsung di berkas `.tex` utama (bagian sampul dan halaman pengajuan):

- `JUDUL SKRIPSI` / `JUDUL PENELITIAN` → judul Anda (huruf besar)
- `NAMA MAHASISWA` dan `NPM`
- `202x` → tahun

Gunakan *Cari & Ganti* di editor agar semua kemunculan terganti sekaligus.

# 7. Dosen dan peran (pembimbing, penguji, dst.)

Daftar dosen ada di `00-common/dosen.tex` (nama, gelar, NIDN). Peran tiap dosen diatur **sekali** dan otomatis dipakai di sampul, lembar persetujuan/pengesahan, kata pengantar, dan abstrak semua dokumen.

## 7.1 Daftar dosen

Satu dosen memiliki dua makro: `\dsn` + kunci (nama dan gelar) dan `\nidn` + kunci (NIDN). *Kunci* adalah singkatan yang dipakai di `peran.tex`. Nomor pada kolom pertama adalah nomor yang diketik di menu pengaturan peran (urutannya mengikuti isi `dosen.tex`).

| No | Nama dan gelar | NIDN | Kunci |
|---|---------------------------------------------|------------------|-----------|
| 1 | Muhammad Awaluddin, S.Kom., M.Kom | 0909038201 | Awaluddin |
| 2 | Darmawan, S.Kom., M.Kom | 0911048204 | Darmawan |
| 3 | Rando, S.Kom., M.Eng. | 0911069501 | Rando |
| 4 | La Ode Muhammad Saidi, S.Si., M.Kom | 0913038905 | Saidi |
| 5 | Didin Adri, S.Si., M.Sc | 0930088802 | Didin |
| 6 | Muhammad Hibrian Wiwi, S.T., M.Kom | 0930129003 | Hibrian |
| 7 | Muhammad Abdur Rasyid Asruddin, S.T., M.Kom | 6641774675130242 | Rasyid |
| 8 | Agusman, S.T., M.M (Dekan Fakultas Teknik) | 0917088206 | Agusman |

Contoh pemakaian kunci: untuk Awaluddin makro namanya `\dsnAwaluddin` dan makro NIDN-nya `\nidnAwaluddin`.

Dekan Fakultas Teknik sudah terisi tetap (Agusman, S.T., M.M) di `peran.tex`. Bila ada pergantian dekan, tambahkan dosen barunya (7.4) lalu pilih lewat menu.

## 7.2 Daftar peran

Ada enam peran. Nama makronya dipakai di berkas `.tex`, dan di menu muncul dengan nama biasa:

| Peran di menu | Makro nama | Makro NIDN | Dipakai di |
|---|---|---|---|
| Pembimbing I | `\PembimbingI` | `\PembimbingInidn` | lembar persetujuan, pengesahan, kata pengantar |
| Pembimbing II | `\PembimbingII` | `\PembimbingIInidn` | lembar persetujuan, pengesahan, kata pengantar |
| Penguji I | `\PengujiI` | `\PengujiInidn` | halaman pengesahan, kata pengantar |
| Penguji II | `\PengujiII` | `\PengujiIInidn` | halaman pengesahan, kata pengantar |
| Ketua Program Studi | `\KetuaProdi` | `\KetuaProdinidn` | halaman pengesahan, kata pengantar |
| Dekan Fakultas Teknik | `\Dekan` | `\Dekannidn` | halaman pengesahan, kata pengantar |

Peran yang belum dipilih tampil sebagai titik-titik isian (……) sehingga dapat ditulis tangan.

## 7.3 Mengatur peran

**Cara termudah:**

```bash
./menu.sh peran
```

(atau menu nomor **7** pada menu interaktif). Program menampilkan daftar dosen bernomor, lalu menanyakan satu per satu: Pembimbing I, Pembimbing II, Penguji I, Penguji II, Ketua Program Studi, dan Dekan. Ketik nomor dosen, tekan Enter untuk membiarkan pilihan yang ada, atau ketik `0` untuk mengosongkan. Berkas `00-common/peran.tex` ditulis ulang otomatis. Setelah itu buat ulang dokumen (menu PDF/Word).

**Cara manual:** edit `00-common/peran.tex`. Isi dengan kunci dosen dari tabel di atas. Contoh, Pembimbing I = Muhammad Awaluddin dan Pembimbing II = Darmawan:

```latex
\newcommand{\PembimbingI}{\dsnAwaluddin}\newcommand{\PembimbingInidn}{\nidnAwaluddin}
\newcommand{\PembimbingII}{\dsnDarmawan}\newcommand{\PembimbingIInidn}{\nidnDarmawan}
```

Nama pada lembar persetujuan dan pengesahan selalu tampil satu baris; bila terlalu panjang, ukuran hurufnya diperkecil otomatis (tidak pindah baris).

## 7.4 Menambah dosen baru

Salin dua baris di `00-common/dosen.tex` dengan kunci baru (tanpa spasi atau tanda baca), lalu isi nama dan NIDN:

```latex
\newcommand{\dsnBudi}{Budi Santoso, S.T., M.T.}
\newcommand{\nidnBudi}{0912345678}
```

Dosen baru otomatis muncul di daftar menu pengaturan peran (di urutan paling bawah), dan kuncinya dapat dipakai di `peran.tex`. Perbarui juga tabel 7.1 pada panduan ini bila perlu.

## 7.5 Peran pada folder contoh

Dokumen pada folder `00-contoh/` mengatur peran dosennya sendiri (hanya ilustrasi) lewat `\renewcommand` di berkas utamanya, sehingga `00-common/peran.tex` tidak berubah. Untuk dokumen sungguhan, cukup atur peran di `00-common/peran.tex` seperti di atas.

# 8. Formulir pengajuan judul

`01-formulir/formulir.tex` berisi formulir pengajuan judul penelitian (Arial 12, 8 halaman). Susunannya:

- **Halaman 1: identitas dan ringkasan.** Logo dan judul formulir; NAMA, NPM/Angkatan, No. HP/Surel, dan Tanggal Pengajuan; tabel tiga judul (huruf kapital, paling banyak 20 kata); tanda tangan pengaju.
- **Halaman 2 sampai 7: tiga usulan, dua halaman per usulan**, masing-masing dibuat oleh satu perintah: `\usulan{1}`, `\usulan{2}`, `\usulan{3}`. Halaman pertama usulan memuat judul, identitas penelitian (bidang, jenis, tipe, asal judul), serta (1) objek/tempat penelitian, (2) latar belakang masalah, (3) rumusan masalah, (4) tujuan penelitian. Halaman kedua memuat (5) metode, (6) tabel penelitian terkait (minimal 10 artikel, rujukan gaya APA 7, beserta persamaan dan perbedaan yang sekaligus menjadi daftar pustaka), status usulan, masukan calon dosen pembimbing, dan tanda tangan mahasiswa serta calon pembimbing.
- **Halaman 8: hasil verifikasi.** Kotak centang untuk Ketua Prodi/dosen verifikator, kotak dosen pembimbing I dan II, serta tanda tangan Ketua Prodi (nama dan NIDN diambil dari `00-common/peran.tex`).

Tiap halaman diawali dengan `halamanbaru` sehingga tidak ada kotak yang terpotong antarhalaman. Untuk mengubah teks petunjuk, edit definisi `\usulan` di bagian atas berkas; ketiga usulan ikut berubah. Membuatnya: `./menu.sh semua formulir`.

# 9. Aturan format yang sudah otomatis

Anda tidak perlu mengatur hal-hal ini secara manual:

| Hal | Pengaturan |
|---|---|
| Kertas | A4 |
| Margin (proposal, seminar hasil, skripsi) | kiri 4, atas 4, kanan 3, bawah 3 cm |
| Huruf | Times New Roman 12 pt |
| Spasi isi | proposal, seminar hasil, skripsi: 1,5 spasi; judul tabel/gambar, lampiran, daftar pustaka 1 spasi |
| Alinea | menjorok 1 cm, rata kanan-kiri |
| Nomor halaman | sampul, lembar persetujuan, pengesahan, dan pernyataan keaslian: tanpa nomor; abstrak sampai daftar lampiran: angka romawi (tengah bawah); isi: angka Arab, awal bab tengah bawah, halaman berikutnya kanan atas |
| Penomoran | bab (I, II, ...), subbab (1.1, 1.1.1), tabel dan gambar per bab (Gambar 1.1, Tabel 2.3) |
| Daftar isi, tabel, gambar | dibuat otomatis |

Proposal, seminar hasil, dan skripsi memakai ukuran, sampul, halaman depan, dan gaya bab yang sama; yang berbeda hanya tulisannya. Judul bab ditulis "BAB I" di tengah lalu judulnya, dan **setiap bab mulai di halaman baru** (halaman pertama bab bernomor tengah bawah, halaman berikutnya kanan atas).

# 10. Berkas Word siap pakai

Berkas `.docx` di `00-output/` (dan `00-contoh/output/`) adalah salinan siap pakai dari dokumen PDF. Perhatikan:

1. Saat membuka di Word, pilih **Yes/Ya** bila ditanya memperbarui field. Ini mengisi **Daftar Isi, Daftar Tabel, dan Daftar Gambar**. Jika belum terisi: klik kanan tulisan di daftar → *Update Field* → *Update entire table*.
2. Pengaturan gaya ada di *Styles* Word (Heading 1–4, Body Text, Table Caption, dst.); mengubah gaya akan mengubah seluruh dokumen.
3. Jangan menghapus *section break* antar bab di skripsi dan seminar hasil; di situlah aturan nomor halaman per bab bekerja.
4. Berkas Word tidak dibuat ulang oleh `menu.sh`. Sumber utama tetap `.tex`; perubahan yang dibuat langsung di Word tidak kembali ke `.tex`.

# 11. Lingkungan khusus yang tersedia

| Lingkungan | Fungsi |
|---|---|
| `tengah` | blok rata tengah satu spasi (sampul, judul halaman) |
| `kanan` | blok digeser ke kanan (penutup kata pengantar, tanda tangan) |
| `daftarpustaka` | pembungkus `\printbibliography`; daftar pustaka otomatis dari `pustaka.bib` (baris kedua menjorok, 1 spasi) |
| `tanpagaris` | tabel tanpa garis (tanda tangan, data) |
| `tabelpusat` | semua kolom tabel di dalamnya rata tengah (khusus Word) |
| `ruang` | jarak kosong vertikal, mis. `\begin{ruang}{2}\end{ruang}` = 2 cm |
| `daftarisi`, `daftartabel`, `daftargambar`, `daftarlampiran` | membuat daftar otomatis (daftar lampiran diisi dari perintah `\lampiran{n}{judul}`) |
| `\satubaris{nama}` | nama (dosen) selalu satu baris; bila kepanjangan, huruf diperkecil otomatis |
| `akhirsampul`, `akhirdepan` | penanda pergantian bagian (nomor halaman) |
| `halamanbaru` | pindah halaman (di Word jadi *page break*) |

# 12. Pemecahan masalah

| Gejala | Penyebab / solusi |
|---|---|
| `File 'skripsi.sty' not found` | LaTeX dijalankan di luar folder dokumen atau tanpa `latexmk`. Kompilasi **berkas utama** dengan `latexmk` (editor) atau pakai `./menu.sh` (bagian 3). |
| `fontspec ... requires XeTeX or LuaTeX` | editor memakai pdfLaTeX. Ganti ke `latexmk` (XeLaTeX); jangan pakai tombol *pdfLaTeX* langsung. |
| Berkas di dalam `bab/` dikompilasi sendiri dan galat | kompilasi berkas utama, bukan berkas bab. |
| `Undefined control sequence` | salah ketik perintah, atau lupa `\` pada karakter khusus (bagian 5.3). Lihat nomor baris pada pesan galat. |
| Kalimat tiba-tiba terpotong | ada `%` tanpa `\` di baris itu. Tulis `\%`. |
| Sitasi tampil `?` atau `[kunci]`; daftar pustaka kosong | kunci salah ketik, atau `biber` belum berjalan. Kompilasi dengan `latexmk`/`./menu.sh` (bukan satu kali `xelatex`). Periksa pesan `biber` (`WARN - I didn't find a database entry for ...`) dan ejaan kunci di `00-common/pustaka.bib`. |
| Daftar isi kosong / nomor halaman salah | jalankan pembuatan PDF sekali lagi (daftar butuh dua putaran; menu sudah mengulang otomatis). |
| Daftar isi di Word hanya tulisan "Klik kanan..." | belum diperbarui; lihat bagian 10. |
| Gambar tidak muncul | periksa nama dan lokasi berkas di `00-gambar/`; nama berkas peka huruf besar-kecil. |
| Menu: "gagal membuat PDF" | buka `build/<dokumen>-latex.log`; galat pertama diawali tanda `!`. |
| Hasil tidak berubah | pastikan berkas yang diedit adalah berkas bab yang dipanggil `\input`, lalu buat ulang dokumen. |

# 13. Ringkasan alur kerja

1. Isi sampul dan data (bagian 6); atur dosen/peran (`./menu.sh peran`).
2. Tulis isi tiap bab di `bab/NN-....tex` (bagian 5).
3. Simpan gambar di `00-gambar/`.
4. `./menu.sh pdf skripsi` → periksa `00-output/skripsi.pdf`.
5. Perbaiki di `.tex`, ulangi langkah 4 sampai siap dikumpulkan.
