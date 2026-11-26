---
{"dg-publish":true,"dg-path":"vmstat.md","permalink":"/vmstat/","title":"vmstat","hideInFiletree":true,"tags":["references","devops"],"noteIcon":"","dg-note-properties":{"title":"vmstat","aliases":["virtual memory statistics","vmstat(8)"],"categories":["Software Systems"],"type":"reference","tags":["references","devops"],"sources":["_raw/articles/vmstat-man-page-2026-11-26.md","_raw/articles/vmstat-redhat-2026-11-26.md","_raw/articles/vmstat-phoenixnap-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

`vmstat` melaporkan statistik sistem secara periodik: proses, memori virtual, swap, IO blok, interrupt, context switch, dan waktu CPU. Artikel Red Hat menyebutnya sebagai alat untuk "virtual memory", tetapi output bawaannya lebih luas dari itu dan sering dipakai sebagai pemeriksaan cepat kesehatan host.

## Sintaks dan arti sampel pertama

Bentuknya `vmstat [option ...] [delay [count]]`. Tanpa `delay`, `vmstat` mencetak satu laporan berisi rata-rata sejak boot terakhir. Dengan `delay`, laporan pertama tetap rata-rata sejak boot dan baris berikutnya mengukur interval `delay`. Man page menambahkan nuansa yang sering terlewat: laporan proses dan memori bersifat instan pada kedua kasus, sedangkan yang dirata-ratakan adalah field laju seperti swap, IO, interrupt, dan CPU. Tanpa `count`, pembaruan berjalan terus sampai dihentikan.

## Kolom

| Kolom | Arti |
|---|---|
| `r` | proses runnable, sedang berjalan atau menunggu giliran CPU |
| `b` | proses terblokir menunggu IO selesai |
| `swpd` | memori swap yang dipakai |
| `free` | memori idle |
| `buff` / `cache` | memori untuk buffer / cache |
| `inact` / `active` | memori inactive dan active, hanya dengan `-a` |
| `si` / `so` | memori di-swap masuk / keluar per detik |
| `bi` / `bo` | KiB diterima / dikirim dari block device per detik |
| `in` / `cs` | interrupt per detik / context switch per detik |
| `us` / `sy` | waktu CPU untuk kode non-kernel / kernel |
| `id` | waktu idle |
| `wa` | waktu menunggu IO |
| `st` | waktu CPU yang diambil hypervisor dari VM |
| `gu` | waktu menjalankan kode guest KVM, termasuk guest nice |

Man page mencatat perubahan historis: sebelum Linux 2.5.41, waktu menunggu IO dihitung sebagai idle, sehingga `id` dan `wa` pada kernel lama tidak sebanding dengan kernel sekarang. `st` baru tersedia sejak Linux 2.6.11. Satuan memori mengikuti `-S` dan default-nya KiB; `bi`/`bo` didefinisikan sebagai KiB/s, dan `-S` secara eksplisit tidak mengubah kedua field itu.

## Opsi yang berguna

`-a` menggantikan kolom `buff` dan `cache` dengan `inact` dan `active`, sehingga yang terlihat adalah pembagian memori menurut aktivitas, bukan buffer dan cache. `-f` mencetak jumlah fork sejak boot. `-s` mencetak tabel total memori, swap, tick CPU, page in/out, interrupt, context switch, boot time, dan fork. `-d` memberi statistik baca/tulis per disk, dan `-t` menambahkan timestamp pada tiap baris sehingga `vmstat -t 5 10` menghasilkan sepuluh sampel berjarak lima detik.

Man page menambahkan beberapa opsi yang tidak disebut artikel: `-y` membuang laporan pertama sejak boot sehingga output hanya berisi sampel interval, `-n` mencetak header sekali saja, `-w` memakai output lebar untuk mesin ber-memori besar, dan `-S k|K|m|M` memilih satuan 1000, 1024, 1000000, atau 1048576 byte. `-m` menampilkan slab, `-D` ringkasan aktivitas disk, dan `-p device` statistik per partisi. Artikel phoenixNAP menambahkan satu catatan praktis untuk `-D`: di Ubuntu jumlah disk bisa lebih besar dari jumlah disk fisik karena snap membuat SquashFS di `/dev/loop` dan `vmstat` menghitungnya sebagai disk.

## Koreksi terhadap artikel Red Hat

Artikel 2020 menyebut `bi`/`bo` dalam blok, sedangkan man page procps-ng sekarang menuliskannya sebagai KiB/s; untuk mesin modern, pakai definisi man page. Artikel juga menyiratkan seluruh laporan pertama adalah rata-rata, padahal laporan proses dan memori instan dan hanya field laju yang dirata-ratakan. Sebaliknya, artikel tidak membahas `-y`, `-n`, `-w`, `-S`, `-m`, `-D`, `-p`, dan kolom `gu` yang ada di man page.

## Koreksi terhadap artikel phoenixNAP

Artikel phoenixNAP berguna untuk contoh opsi, tetapi beberapa klaimnya tidak akurat. `vmstat` bukan bagian dari paket sysstat seperti ditulis artikel; man page menyebutnya bagian dari procps-ng, sedangkan sysstat menyediakan `sar`, `iostat`, `mpstat`, dan `pidstat`. Opsi `-S` dengan huruf besar `K` dan `M` bukan "hexadecimal kilobytes", melainkan satuan berbasis 1024 dan 1048576 byte. Artikel juga menyebut `b` sebagai "sleeping process count" padahal artinya proses terblokir menunggu IO, `swpd` sebagai "total virtual memory" padahal itu swap yang terpakai, dan `st` sebagai waktu yang "stolen by a virtual machine" padahal itu waktu yang diambil hypervisor dari VM tersebut.

Definisi active/inactive di artikel juga menyesatkan: ia menyebut inactive sebagai memori milik proses yang sudah tidak berjalan, sedangkan dalam kernel keduanya adalah klasifikasi halaman untuk reclaim, bukan penanda proses hidup atau mati. Untuk keputusan kapasitas, definisi man page lebih aman dipakai daripada penjelasan artikel ini.

## Cara membaca yang jujur

`free` yang kecil bukan otomatis tanda masalah, karena Linux memakai memori bebas untuk cache dan akan melepaskannya saat dibutuhkan. Artikel hanya menyebut `free` dan swap activity sebagai informasi penting, tanpa menjelaskan perbedaan free versus available; untuk keputusan kapasitas, angka `free` saja menyesatkan.

Sinyal yang lebih bermakna adalah pola: `si`/`so` yang terus-menerus bukan nol menandakan tekanan memori, `b` yang tinggi bersama `wa` besar mengarah ke IO, dan `st` yang besar berarti CPU VM diambil host lain. Satu sampel tidak cukup untuk menyimpulkan; yang dibaca adalah tren beberapa interval.

Batasnya: `vmstat` memberi angka agregat sistem, bukan per proses atau per container, dan tidak menyimpan riwayat. Untuk pertanyaan per proses atau historis, alat lain lebih tepat. Ia juga tidak menjawab pertanyaan "file apa yang terbuka", yang merupakan wilayah [[References/lsof\|lsof]]. Sebagai perintah baris, `vmstat` dipakai dengan cara yang sama seperti alat [[References/Command-Line Interface\|Command-Line Interface]] lain, dan hasilnya masuk ke pemeriksaan rutin di [[References/Server Security\|Server Security]]. Man page menambahkan batas akses: `vmstat` membaca `/proc`, `-m` memerlukan `/proc/slabinfo` yang mungkin hanya bisa dibaca root, dan mount `/proc` dengan `subset=pid` dapat menyembunyikan sebagian isi.

## Sumber

- [vmstat(8) - Linux manual page](https://man7.org/linux/man-pages/man8/vmstat.8.html), procps-ng, halaman bertanggal 2024-07-19, diakses 26 November 2026.
- [Linux commands: exploring virtual memory with vmstat](https://www.redhat.com/sysadmin/linux-commands-vmstat), Tyler Carrigan, Red Hat Enable Sysadmin, 10 Juni 2020.
- [How to Use the vmstat Command](https://phoenixnap.com/kb/vmstat-command), phoenixNAP Knowledge Base, diakses 26 November 2026.
