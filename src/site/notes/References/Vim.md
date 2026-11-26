---
{"dg-publish":true,"dg-path":"Vim.md","permalink":"/vim/","title":"Vim","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"Vim","aliases":["vi","Vim editor","gVim"],"categories":["Software Systems"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/vim-home-2026-11-26.md","_raw/articles/vim-about-2026-11-26.md","_raw/articles/vim-docs-2026-11-26.md","_raw/articles/vim-download-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

Vim adalah editor teks yang dapat dikonfigurasi tinggi dan dirancang agar pengeditan teks apa pun menjadi efisien. Proyeknya menyebutnya "the ubiquitous text editor" karena Vim disertakan sebagai `vi` di sebagian besar sistem UNIX dan Apple OS X. Berbeda dari [[References/GNU nano\|GNU nano]] yang memakai tombol langsung, Vim bekerja dengan mode dan perintah, sehingga kurva belajarnya lebih curam tetapi alurnya lebih ringkas untuk pengeditan berulang.

## Fitur utama

Halaman resmi menyebut lima kemampuan inti: undo tree persisten bertingkat, sistem plugin yang luas, dukungan ratusan bahasa pemrograman dan format file, pencarian dan penggantian yang kuat, serta integrasi dengan banyak alat lain. Halaman About menambahkan bahwa Vim sering disebut "programmer's editor" dan banyak dipakai seperti IDE, tetapi juga cocok untuk menulis email dan menyunting file konfigurasi. Vim bukan pengolah kata: ia menampilkan highlight dan format, bukan WYSIWYG. Mode `evim` (Easy Vim) membuatnya berperilaku seperti Notepad bagi yang butuh pengeditan sederhana.

## Versi dan rilis

Vim 9.2.1417 adalah versi saat ini. Vim 9.2 dirilis 14 Februari 2026 dengan perubahan besar pada bahasa skrip Vim9: dukungan asli untuk enum, generic function, dan tuple. Rilis itu juga membawa mode diff "linematch" dengan granularitas per karakter, completion fuzzy dan completion register, default baru untuk `history`, `backspace`, dan tampilan High-DPI, serta dukungan Wayland dan XDG Base Directory.

GUI GTK 4 masuk ke master lewat patch 9.2.0501 dan diumumkan 19 Mei 2026. Backend baru ini diaktifkan saat build dengan `--enable-gui=gtk4` dan berjalan di X11 maupun Wayland; GTK 3 tetap default ketika memakai `--enable-gui=auto`, jadi build lama tidak berubah. Backend tersebut masih opt-in sampai stabil.

## Lisensi dan donasi

Vim adalah charityware: lisensinya kompatibel GPL dan distribusinya gratis, tetapi pengguna diminta berdonasi untuk anak-anak di Uganda. Sejak Bram Moolenaar meninggal, donasi diarahkan ke Kuwasha untuk melanjutkan warisannya. Halaman beranda dan About sama-sama menyebut kanal donasi ini, bukan lisensi copyleft biasa tanpa permintaan.

## Unduh dan paket

Untuk MS-Windows, installer bertanda tangan tersedia di proyek `vim-win32-installer` dan juga lewat Winget. Untuk Unix, cara termudah adalah `git clone https://github.com/vim/vim.git` lalu `make` di direktori `src`; tersedia pula arsip sumber `vim-9.2.tar.bz2` dan AppImage harian. Di macOS, MacVim menyediakan GUI dan Homebrew menyediakan versi terminal. Varian lain yang dicatat halaman unduh mencakup Android (Vim Touch), QNX, Cygwin, OpenVMS, dan HP-UX. Halaman itu masih menampilkan paket Windows 9.2.0000, sedangkan versi saat ini 9.2.1417, sehingga tautan unduh Windows adalah snapshot yang bisa tertinggal.

## Dokumentasi

Sistem dokumentasi utama diakses dengan `:help` di dalam Vim dan berisi referensi silang yang mengikuti versi perangkat lunak. Salinan HTML terbaru dipelihara di vimhelp.org oleh Carlo Teubner; salinan di VimDoc disebut sudah usang. Untuk pemula ada Vim Adventure yang mengajarkan `h`, `j`, `k`, `l` secara interaktif dan `:help tutor` yang lebih cepat. Tersedia pula FAQ, Vim Tips wiki, buku sekitar selusin judul termasuk "Use Vim Like A Pro", dan Vimcasts yang dibuat 2010 sampai 2011. Pola pengeditan teks dengan perintah kecil dan pipe dibahas terpisah di [[References/Linux Text Processing Command\|Linux Text Processing Command]], sedangkan posisi editor terminal dalam alur kerja shell ada di [[References/Command-Line Interface\|Command-Line Interface]].

## Catatan sumber

Empat halaman resmi diambil: beranda, About, Documentation, dan Download. Halaman About sudah lama tidak diperbarui; isinya masih menyebut Vim 5/6/7, penghargaan Linux Journal 2005-2009, dan hak cipta 2007, jadi klaim versi di halaman itu tidak dipakai sebagai status terkini. Vim tidak terpasang di mesin ini, sehingga perilaku `:help` dan opsi build tidak diverifikasi lokal; semua klaim versi berasal dari halaman resmi yang diakses 26 November 2026. Untuk perbandingan, [[References/GNU Emacs\|GNU Emacs]] juga dapat diperluas lewat bahasa skrip sendiri, tetapi dengan model ekstensi dan komunitas yang berbeda.

## Sumber

- [Vim - the ubiquitous text editor](https://www.vim.org/), beranda, diakses 26 November 2026.
- [About Vim](https://www.vim.org/about.php), diakses 26 November 2026.
- [Vim documentation](https://www.vim.org/docs.php), diakses 26 November 2026.
- [Downloading Vim](https://www.vim.org/download.php), diakses 26 November 2026.
