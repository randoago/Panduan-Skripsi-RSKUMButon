# Panduan Skripsi — Rekayasa Sistem Komputer, FT UM Buton

Template dan panduan penulisan skripsi untuk mahasiswa **Program Studi Rekayasa Sistem Komputer (RSK), Fakultas Teknik, Universitas Muhammadiyah Buton**. Semua dokumen ditulis dalam **LaTeX** dan dari satu sumber yang sama dapat dibuat menjadi **PDF** maupun **Word (.docx)**.

> **Mulai cepat:** baca [Pedoman](00-pedoman/pedoman.pdf), lihat [contoh skripsi terisi](00-contoh/output/skripsi.pdf), lalu edit template tahap Anda. Cara memasang dan memakai ada di [Panduan LaTeX](00-panduan/panduan-latex.pdf).

## Alur tahapan skripsi

| Tahap | Folder | Dokumen | Contoh PDF |
| :-: | --- | --- | --- |
| 1 | [`01-formulir/`](01-formulir) | Formulir pengajuan judul (3 usulan, mengikuti SP-01A) | [formulir.pdf](01-formulir/formulir.pdf) |
| 2 | [`02-proposal/`](02-proposal) | Template proposal: sampul, persetujuan, abstrak, Bab I–III, daftar pustaka | [proposal.pdf](02-proposal/proposal.pdf) |
| 3 | [`03-seminar-hasil/`](03-seminar-hasil) | Format seminar hasil: Bab I–V, daftar pustaka, lampiran | [seminar-hasil.pdf](03-seminar-hasil/seminar-hasil.pdf) |
| 4 | [`04-skripsi/`](04-skripsi) | Format skripsi: pengesahan, keaslian, abstrak, kata pengantar, Bab I–V, lampiran | [skripsi.pdf](04-skripsi/skripsi.pdf) |

Folder berawalan `00-` bukan tahapan, melainkan pendukung (pedoman, panduan, contoh, hasil, gaya bersama).

## Apa yang sudah otomatis

| Hal | Pengaturan yang sudah tertanam |
| --- | --- |
| Kertas dan margin | A4; kiri 4, atas 4, kanan 3, bawah 3 cm (formulir: Arial 12) |
| Huruf dan spasi | Times New Roman 12 pt; isi 1,5 spasi; abstrak, judul tabel/gambar, daftar, daftar pustaka, dan lampiran 1 spasi; alinea menjorok 1 cm, rata kanan-kiri |
| Judul bab | `BAB I` + judul (14 pt tebal, tengah), tiap bab di halaman baru; subbab sampai tiga tingkat |
| Nomor halaman | Sampul, persetujuan, pengesahan, keaslian: tanpa nomor; abstrak sampai daftar lampiran: romawi; isi: arab |
| Tabel dan gambar | Nomor per bab (Tabel 2.1, Gambar 3.2; lampiran L1.1); judul tabel di atas, judul gambar di bawah |
| Daftar isi, tabel, gambar, lampiran, singkatan | Dibuat otomatis |
| Sitasi dan daftar pustaka | **APA edisi ke-7**. Catat sumber sekali di `00-common/pustaka.bib`, panggil dengan `\parencite{kunci}` atau `\textcite{kunci}`; daftar pustaka tersusun sendiri |
| Nama dosen | Pembimbing, penguji, Ketua Prodi, dan Dekan diatur sekali, lalu dipakai di semua dokumen |

## Cara pakai

**1. Pasang program** (macOS; periksa dengan `./menu.sh cek`):

| Program | Fungsi | Pasang |
| --- | --- | --- |
| `xelatex`, `latexmk`, `biber` | membuat PDF dan daftar pustaka | `brew install --cask mactex-no-gui` |
| `pandoc`, `python3` | membuat Word | `brew install pandoc python3` |

**2. Jalankan menu:**

```bash
./menu.sh                      # menu interaktif
./menu.sh pdf proposal         # buat PDF
./menu.sh word skripsi         # buat Word
./menu.sh semua semua          # PDF + Word semua template
./menu.sh semua contoh         # PDF + Word tiga dokumen contoh
./menu.sh peran                # atur pembimbing, penguji, ketua prodi, dekan
./menu.sh buka proposal        # buka hasilnya
./menu.sh bersih               # hapus berkas sementara
```

Pilihan dokumen: `proposal`, `formulir`, `seminar-hasil`, `skripsi`, `semua`, `contoh` (atau `contoh-proposal`, `contoh-hasil`, `contoh-skripsi`). Hasil ada di `00-output/`; hasil dokumen contoh di `00-contoh/output/`.

**3. Edit isi** hanya pada berkas `.tex` di folder `bab/` tiap tahap, lalu buat ulang PDF atau Word. Ingin menambah bab? Buat `bab/NN-nama.tex`, lalu `\input{bab/NN-nama}` di berkas utama.

Bisa juga memakai editor LaTeX (misalnya VS Code dengan LaTeX Workshop): buka berkas utama (`02-proposal/proposal.tex`, dan seterusnya) lalu tekan *Build*. Setiap folder sudah punya `latexmkrc` yang memilih XeLaTeX otomatis.

## Isi folder

```text
00-pedoman/        pedoman.pdf (aturan penulisan skripsi, hanya PDF)
00-panduan/        panduan-latex (.md, .pdf, .docx): cara memakai dan mengedit
00-contoh/         proposal, seminar hasil, dan skripsi contoh terisi (data fiktif) + output/
00-output/         hasil PDF dan Word dari template
00-common/         gaya bersama, pustaka.bib, daftar dosen (dosen.tex), peran (peran.tex)
00-gambar/         logo dan gambar contoh (diagram alat, rangkaian, prototipe)
00-word/           pembuat Word (buat_word.py) dan filter pandoc (filter.lua)
01-formulir/       tahap 1
02-proposal/       tahap 2
03-seminar-hasil/  tahap 3
04-skripsi/        tahap 4 (isi per bab di bab/)
menu.sh            menu pembuat PDF dan Word
```

## Pertanyaan yang sering muncul

- **Word kosong pada daftar isi?** Daftar isi, tabel, gambar, dan lampiran di Word berupa *field*. Pilih **Ya/Yes** saat Word menawarkan pembaruan, atau tekan Ctrl+A lalu F9.
- **Mengganti nama dosen?** Jalankan `./menu.sh peran`, atau edit `00-common/peran.tex`. Daftar dosen ada di `00-common/dosen.tex`.
- **Apakah data pada contoh asli?** Tidak. Nama, NPM, angka hasil, dan peran dosen di `00-contoh/` fiktif dan hanya memperagakan format.
- **Isi tabel hilang saat jadi Word?** Hindari `>{\centering}p{..}` pada tabel; pakai kolom `c` atau `p{..}`.
- **Menjalankan pandoc sendiri?** Jangan isi `TEXINPUTS`; `menu.sh` dan `buat_word.py` sudah mengaturnya.
- **Bab IV Pedoman** (tata cara pengajuan, bimbingan, seminar, ujian) bersifat umum; ikuti ketentuan terbaru Program Studi.
