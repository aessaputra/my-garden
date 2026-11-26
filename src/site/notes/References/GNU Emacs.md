---
{"dg-publish":true,"dg-path":"GNU Emacs.md","permalink":"/gnu-emacs/","title":"GNU Emacs","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"GNU Emacs","aliases":["Emacs","GNU Emacs editor"],"categories":["Software Systems"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/emacs-home-2026-11-26.md","_raw/articles/emacs-documentation-2026-11-26.md","_raw/articles/emacs-download-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

GNU Emacs adalah editor teks bebas yang dapat diperluas dan dikustomisasi, dengan inti berupa interpreter Emacs Lisp, dialek bahasa Lisp yang ditambah dukungan penyuntingan teks. Proyeknya menyebutnya "text editor — and more" karena di atas editing dasar ia menyediakan perencana proyek (Org), pembaca mail dan news (Gnus), antarmuka debugger, kalender, klien IRC (ERC), dan sistem paket untuk memasang ekstensi dari ELPA. Dibandingkan [[References/Vim\|Vim]] yang berfokus pada efisiensi pengeditan modal, Emacs cenderung menjadi lingkungan kerja yang diperluas dengan Lisp, bukan sekadar editor.

## Fitur

Halaman resmi menyebut editing mode sadar konten dengan pewarnaan sintaks untuk banyak jenis file, dokumentasi bawaan lengkap termasuk tutorial pemula, dukungan Unicode hampir semua aksara manusia, kustomisasi lewat kode Emacs Lisp atau antarmuka grafis, dan fungsionalitas luas di luar teks. Paket ekstensi diunduh dan dipasang lewat sistem paket yang terhubung ke GNU ELPA.

## Versi dan rilis

Versi terbaru adalah Emacs 31.1, dirilis 24 Agustus 2026. Perubahan yang dicatat halaman beranda:

- Kontrol Emacs dengan mouse di terminal aktif secara default.
- Subdirektori `user-lisp/` baru di direktori konfigurasi.
- Child frame didukung pada text frame.
- Perbaikan pada buffer *Completions*.
- Perintah split window memilih arah yang lebih baik.
- Buffer *grep* dan *xref* mendukung mode yang dapat diedit.
- Fitur untuk meninjau perubahan paket sebelum memasang atau memperbaruinya.
- Tema baru yang membuat Emacs lebih ramah bagi pengguna baru.

Sebelumnya, Emacs 30.2 (14 Agustus 2025) adalah rilis pemeliharaan, sedangkan Emacs 30.1 (23 Februari 2025) membawa native compilation default, port ke Android, opsi `trusted-contents`, peningkatan performa, dukungan JSON native, major mode Tree-Sitter untuk Elixir, HEEx, HTML, Lua, dan PHP, dukungan EditorConfig, perbaikan touchscreen, toolbar per window, paket `which-key`, regenerasi TAGS otomatis, `visual-wrap-prefix-mode`, dan warna underline pada TTY. Tanggal rilis lama tersedia di halaman history proyek.

## Platform dan instalasi

Emacs berjalan di GNU, GNU/Linux, FreeBSD, NetBSD, OpenBSD, Haiku, macOS, MS Windows, Android, Solaris, dan MS-DOS/FreeDOS. Untuk GNU/Linux dan BSD, repositori distribusi adalah cara yang disarankan kecuali pengguna selalu butuh rilis terbaru; perintah yang dicontohkan mencakup `guix package -i emacs`, `sudo apt-get install emacs`, `sudo pacman -S emacs`, `sudo dnf install emacs`, dan `sudo zypper install emacs`. Di Haiku tersedia lewat HaikuDepot. Untuk Windows ada installer `emacs-<version>-installer.exe` yang ditandatangani Corwin Brust, atau arsip zip yang dijalankan lewat `bin\runemacs.exe`; pengguna MSYS2 bisa memakai paket `mingw-w64-x86_64-emacs`. Di macOS ada Homebrew (`brew install --cask emacs`), MacPorts (`emacs-app`), dan situs emacsformacosx.com. Android didukung sebagai aplikasi GUI dan bisa dipasang dari mirror GNU atau F-Droid, dengan README khusus untuk memilih unduhan sesuai perangkat. Kode sumber dan pengembangan di-host di Savannah.

Halaman unduh memuat argumen filosofis GNU: dukungan untuk sistem proprietary dianggap jalan agar pengguna mencicipi kebebasan, bukan tujuan untuk memperbaiki sistem tersebut. Itu framing proyek, bukan klaim teknis tentang kualitas port.

## Dokumentasi

Manual GNU Emacs, Introduction to Programming in Emacs Lisp, dan Emacs Lisp Reference Manual dapat dibaca online atau dibeli; manual lain untuk major mode dan fitur opsional juga tersedia. Semuanya didistribusikan bersama Emacs dalam format Info, diakses dengan `C-h i` di dalam Emacs. Distribusi juga menyertakan kode sumber manual dan beberapa reference card dalam berbagai bahasa. Untuk pengeditan file konfigurasi di server, [[References/GNU nano\|GNU nano]] lebih ringan, sedangkan posisi editor dalam alur shell dibahas di [[References/Command-Line Interface\|Command-Line Interface]].

## Catatan sumber

Tiga halaman resmi diambil: beranda, Documentation, dan Download. Halaman beranda memuat fitur dan catatan rilis 31.1/30.2/30.1; versi dan tanggalnya adalah snapshot per 26 November 2026, bukan jaminan rilis berikutnya. Emacs tidak terpasang di mesin ini, jadi klaim perilaku `C-h i`, native compilation, dan paket ELPA tidak diuji lokal. Halaman dokumentasi hanya berisi daftar manual dan tidak menjelaskan isi masing-masing.

## Sumber

- [GNU Emacs](https://www.gnu.org/software/emacs/), beranda, diakses 26 November 2026.
- [GNU Emacs documentation](https://www.gnu.org/software/emacs/documentation.html), diakses 26 November 2026.
- [GNU Emacs download](https://www.gnu.org/software/emacs/download.html), diakses 26 November 2026.
