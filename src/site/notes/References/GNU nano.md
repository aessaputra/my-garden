---
{"dg-publish":true,"dg-path":"GNU nano.md","permalink":"/gnu-nano/","title":"GNU nano","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"GNU nano","aliases":["nano","nano editor","GNU nano editor"],"categories":["Software Systems"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/nano-home-2026-11-26.md","_raw/articles/nano-news-2026-11-26.md","_raw/articles/nano-docs-2026-11-26.md","_raw/articles/nano-history-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

GNU nano adalah editor teks kecil dan ramah yang menjadi bagian proyek GNU. Ia menyajikan perintah lewat kombinasi tombol yang tertera di bagian bawah layar, bukan lewat mode seperti [[References/Vim\|Vim]]. Karena itu nano sering dipakai untuk mengedit file konfigurasi di server, tempat pengguna butuh perubahan cepat tanpa mempelajari modal editing. Riwayat proyek mencatat bahwa nano sengaja dibuat kompatibel dengan Pico dan pada versi 1.0 berukuran sekitar setengah biner Pico.

## Versi terkini

Versi terbaru adalah 9.3 "Damiana", dirilis 21 Oktober 2026. Dua perubahan yang dicatat: nama file kepanjangan di mini bar kini terpotong dengan benar, dan tombol operator pada keypad menghasilkan karakter yang tertera di tombolnya. Rilis sebelumnya:

- **9.2 "Alquézar"**, 31 Juli 2026: nano menolak mulai bila standard output bukan terminal; opsi `--newbuffer` dan `set newbuffer` ditambahkan sebagai sinonim yang lebih baik untuk `--multibuffer`; pasangan keystroke lama `^W^T` dan `^/^T` dikenali lagi.
- **9.1**, 23 Juni 2026: viewport pencarian ditempatkan rapat ke kiri bila memungkinkan; kemampuan baca/tulis format Mac lama (baris berakhir carriage return tunggal) dihapus; toggle `^T` antara WhereIs dan GotoLine dihapus; perbaikan backup dan `.save` yang tidak di-chmod saat nano dibunuh atau crash.
- **9.0**, 8 April 2026; **8.7.1**, 4 Februari 2026; **8.7**, 12 November 2025; **8.6**, 21 Agustus 2025; **8.5**, 12 Juni 2025; **8.4**, 2 April 2025; **8.3**, 21 Desember 2024; **8.2**, 5 September 2024; **8.1**, 12 Juli 2024; **8.0**, 1 Mei 2024; **7.2**, 18 Januari 2023.

## Riwayat fitur

Halaman History merangkum perkembangan dari 1.0 sampai 2.6. Versi 1.0 sudah memuat lisensi GNU GPL, dukungan slang dan ncurses, autoindent, lebar tab, pencarian dan penggantian regex, toggle flag lewat Meta, serta tab completion saat membaca/menulis file. Versi 1.2 menambah kompatibilitas Pico, smooth scrolling (`-S`), bracket matching, `.nanorc`, syntax highlighting berwarna, dan multiple buffer (`-F`). Versi 2.0 membawa UTF-8 dan mode input verbatim; 2.2 menambah key binding yang dapat dikustomisasi, pencarian yang dapat dibatalkan, dan dukungan pager; 2.4 membuat undo/redo default serta menambah dukungan linter dan formatter; 2.6 menambah komentar baris dengan `M-3` dan perbaikan spasi di akhir paragraf.

Halaman itu berhenti di 2.6, jadi rentang 2.7 sampai 9.3 hanya tercakup halaman News, bukan halaman History.

## Dokumentasi dan konfigurasi

Halaman Documentation adalah indeks menuju FAQ, cheatsheet keystroke, manual HTML dan PDF, man page `nano.1`, man page `nanorc.5` untuk file konfigurasi, README, ChangeLog, dan NEWS. Berkas `nanorc` adalah tempat mengatur key binding, warna, dan perilaku lain; detailnya ada di man page terpisah. Untuk pengeditan file di host jarak jauh, nano biasanya dipakai lewat sesi SSH yang dijelaskan di [[References/Command-Line Interface\|Command-Line Interface]], dan berbeda dari [[References/GNU Emacs\|GNU Emacs]] yang menawarkan lingkungan terintegrasi jauh lebih besar.

## Catatan sumber

Beranda nano hanya berisi ASCII art, nomor versi 9.3, dan menu navigasi; isi substantif diambil dari News, Documentation, dan History. Versi 7.2 terpasang di mesin ini (dicek dengan `nano --version`, GNU nano 7.2, 2023), jadi versi lokal tertinggal beberapa rilis dari 9.3 dan perilaku 8.x/9.x tidak diuji langsung. Halaman History terakhir diperbarui untuk 2.6 dan tidak boleh dibaca sebagai status fitur terkini. Semua tanggal rilis berasal dari halaman News yang diakses 26 November 2026.

## Sumber

- [nano – Text editor](https://www.nano-editor.org/), beranda, diakses 26 November 2026.
- [nano – News](https://www.nano-editor.org/news.php), diakses 26 November 2026.
- [nano – Documentation](https://www.nano-editor.org/docs.php), diakses 26 November 2026.
- [nano – History](https://www.nano-editor.org/history.php), diakses 26 November 2026.
