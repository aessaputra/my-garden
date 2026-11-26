---
{"dg-publish":true,"dg-path":"Linux Text Processing Command.md","permalink":"/linux-text-processing-command/","title":"Linux Text Processing Command","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"Linux Text Processing Command","aliases":["Linux text processing","text processing commands"],"categories":["Software Systems"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/linux-text-processing-earthly-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

Text processing di Linux adalah soal komposisi, bukan satu alat besar: satu perintah menyaring baris, perintah berikutnya memotong kolom, lalu hasilnya diurutkan atau dihitung. Semua berjalan di atas aliran teks yang sama, sehingga perintah kecil bisa dirangkai tanpa saling tahu satu sama lain. Artikel Earthly (Mei 2023, diperbarui Juli 2023) merangkum tiga belas perintah dan tiga alat modern; halaman ini menyarikan bagian yang berguna dan mengoreksi beberapa klaimnya terhadap perilaku perintah yang diperiksa langsung di shell.

## Stream dan redirection

Setiap proses memakai tiga stream: stdin (0), stdout (1), dan stderr (2). Redirection menyambungkan stream itu ke file:

| Operator | Arti |
|---|---|
| `>` | tulis stdout ke file, menimpa isi lama |
| `>>` | tambahkan stdout ke akhir file |
| `<` | baca stdin dari file |
| `2>` / `2>>` | tulis atau tambahkan stderr ke file |
| `&>` | stdout dan stderr ke file yang sama (bash) |

`>` mengosongkan file tujuan sebelum perintah berjalan, jadi `cmd > out.txt` yang gagal tetap meninggalkan file kosong. Urutan penting saat menggabungkan stream: `cmd > out 2>&1` mengirim keduanya ke file, sedangkan `cmd 2>&1 > out` hanya mengirim stderr ke terminal asal karena pengalihan dievaluasi kiri ke kanan.

## Pipe

`cmd1 | cmd2` menyambungkan stdout `cmd1` ke stdin `cmd2`. Exit status pipeline adalah status perintah terakhir, sehingga kegagalan di tengah pipeline mudah hilang; `set -o pipefail` membuat shell memakai status gagal pertama. Contoh populer `curl -L URL | sh` memasang skrip tanpa pernah membacanya; lebih aman unduh dulu, periksa, lalu jalankan.

## Glob bukan regex

Shell tidak memakai regular expression. `*` di shell adalah glob untuk nama file, bukan operator regex. `ls *.go` berarti "semua file berekstensi .go", bukan pola regex, dan ekspansinya dilakukan shell sebelum perintah dijalankan. Konsekuensinya `ls *.go | go fmt` tidak bekerja seperti yang diklaim artikel: `go fmt` membaca argumen, bukan stdin. Yang benar `go fmt *.go`, atau `ls *.go | xargs go fmt` bila daftarnya dari pipe.

Di grep, pola default adalah BRE. `k*s` berarti nol atau lebih `k` lalu `s`, sehingga praktis cocok dengan setiap baris yang mengandung `s`; untuk "k, lalu apa pun, lalu s" tulis `k.*s`. `^example` menandai awal baris, bukan awal kata, jadi ia mencocokkan baris yang dimulai `example`; untuk batas kata pakai `\bexample` (GNU grep) atau `\<example`.

## Perintah dasar

| Perintah | Fungsi dan jebakan |
|---|---|
| `cut` | potong kolom: `-d` delimiter, `-f` field, `-f 2-3` inklusif. Default delimiter TAB, bukan whitespace; untuk spasi berulang pakai `awk '{print $1}'`. |
| `sort` | urutkan baris. Default leksikal, jadi `10` sebelum `9`; pakai `-n` untuk angka, `-r` menurun, `-f` abaikan kapital, `-k` kunci, `-o` tulis ke file. |
| `tac` | balik urutan baris, berguna untuk membaca log terbaru lebih dulu; bukan membalik karakter dalam baris. |
| `uniq` | hanya menciutkan duplikat yang bersebelahan. Untuk duplikat global: `sort file \| uniq`; `-c` menghitung, `-u` hanya baris yang muncul sekali, `-d` hanya yang berulang. |
| `sed` | `s/lama/baru/g` mengganti semua kemunculan per baris; tanpa `g` hanya yang pertama. Default cetak ke stdout, `-i` mengubah file (pakai `-i.bak` untuk menyimpan cadangan). |
| `awk` | bahasa pemrosesan per record: `-F` separator, `$1` field, `NR>1` melewati header, `{sum+=$2} END{print sum}` menjumlahkan kolom. Field non-numerik dihitung nol. |
| `tr` | terjemahkan atau hapus karakter, bukan regex: `tr 'A-Z' 'a-z'`, `-d` hapus, `-s` rapatkan duplikat. Quote rentangnya agar tidak diekspansi shell. |
| `wc` | default baris, kata, byte. `-l`, `-w`, `-c` (byte), `-m` (karakter). Untuk UTF-8 `-c` dan `-m` berbeda: `é` adalah 3 byte tetapi 1 karakter. |
| `tee` | salin stdout ke layar dan file sekaligus; `-a` menambahkan alih-alih menimpa. Berguna menyimpan hasil antar-tahap pipeline tanpa memutusnya. |
| `xargs` | ubah stdin menjadi argumen: `-n 1` satu argumen per eksekusi, `-p` minta konfirmasi, `-0` untuk input berakhiran NUL. Tanpa `-0`, nama file berspasi atau berbaris baru terpecah. |

Dua pola yang sering dipakai: `sort file | uniq -c | sort -rn` untuk frekuensi, dan `find . -type f -print0 | xargs -0 cmd` untuk batch yang aman terhadap spasi. `ls | xargs rm` yang dicontohkan artikel berbahaya bukan hanya karena menghapus tanpa konfirmasi, tetapi juga karena `ls` memecah nama berspasi dan tidak menyertakan dotfile.

## Alat modern

- `rg` (ripgrep): rekursif dengan nomor baris secara default, menghormati `.gitignore`, dan melewati file tersembunyi serta biner. Karena itu hasilnya tidak identik dengan `grep -rn`, meski terlihat mirip pada contoh sederhana.
- `bat`: `cat` dengan syntax highlighting dan pager; saat dipakai dalam pipeline atau skrip, matikan pager (`--no-pager` atau `-p`).
- `fd`: pencarian nama file dengan pola regex (bukan glob), smart case, dan menghormati `.gitignore`; `fd -e go` memfilter ekstensi.

## Koreksi terhadap artikel Earthly

- "Semua di Linux adalah file" adalah penyederhanaan berlebihan; banyak resource diekspos sebagai file di `/proc` dan `/dev`, tetapi proses, socket, dan device punya semantik sendiri.
- "Shell modern mendukung regex" salah. Bash, zsh, dan fish melakukan globbing; `*.go` adalah glob, bukan regex.
- `ls *.go | go fmt` tidak berfungsi karena `go fmt` tidak membaca daftar file dari stdin.
- `grep k*s` bukan pola "karakter apa pun di antara k dan s"; dalam BRE ia cocok dengan hampir setiap baris yang mengandung `s`. Pola yang dimaksud adalah `k.*s`.
- `grep ^example` mencocokkan awal baris, bukan awal kata.
- `cut -f 1` memakai TAB sebagai delimiter, bukan "whitespace".
- Contoh `cut -d ',' -f 2-3` menampilkan tiga kolom (`age,language,hobby`), padahal rentang 2-3 hanya menghasilkan dua kolom.
- `uniq` tidak "hanya mencetak baris unik"; defaultnya menciutkan duplikat bersebelahan.
- `wc -c` menghitung byte, bukan karakter; karakter memakai `-m`.
- Contoh `echo $` untuk nama repo kehilangan nama variabelnya; gunakan `$GITHUB_REPOSITORY` atau `${{ github.repository }}`.
- `curl | sh` dijadikan contoh pemasangan tanpa peringatan verifikasi.

Perintah-perintah ini adalah bahan dasar [[References/Command-Line Interface\|Command-Line Interface]] dan dilatih lebih lanjut di [[References/Learn Shell\|Learn Shell]]; di pekerjaan operasional, `tac`, `grep`, `awk`, dan `sort` adalah alat pertama saat membaca log, misalnya ketika triage di [[References/Server Security\|Server Security]].

## Sumber

- [Linux Text Processing Command](https://earthly.dev/blog/linux-text-processing-commands/), Earthly Blog, Anurag Kumar, 15 Mei 2023 (diperbarui 14 Juli 2023).
- Verifikasi perilaku lokal pada 26 November 2026: `cut` (delimiter TAB default), `grep` BRE `k*s`, `sort` leksikal versus `-n`, `uniq` duplikat bersebelahan, dan `wc -c` versus `-m` dijalankan langsung di shell.
