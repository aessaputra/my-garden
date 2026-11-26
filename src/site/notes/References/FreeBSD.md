---
{"dg-publish":true,"dg-path":"FreeBSD.md","permalink":"/free-bsd/","title":"FreeBSD","hideInFiletree":true,"tags":["references","devops","security"],"noteIcon":"","dg-note-properties":{"title":"FreeBSD","aliases":["FreeBSD Project","The FreeBSD Project"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","security"],"sources":["_raw/articles/freebsd-home-2026-11-26.md","_raw/articles/freebsd-about-2026-11-26.md","_raw/articles/freebsd-handbook-introduction-2026-11-26.md","_raw/articles/freebsd-newbies-2026-11-26.md","_raw/articles/freebsd-docs-portal-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

FreeBSD adalah sistem operasi Unix-like open source yang menurunkan diri dari 4.4BSD-Lite, hasil riset Computer Systems Research Group di University of California, Berkeley. Fokusnya fitur, kecepatan, dan stabilitas, dengan dukungan x86 32/64-bit, ARM, AArch64, RISC-V, POWER, dan PowerPC. Sistem dasar dijalankan dari baris perintah, sehingga kebiasaan [[References/Command-Line Interface\|Command-Line Interface]] dan pembacaan manual page adalah modal utama sebelum menyentuh konfigurasi.

## Fitur inti

- **OpenZFS terintegrasi**: ZFS berperan sebagai filesystem sekaligus volume manager, terpasang langsung di sistem, termasuk root-on-ZFS, ZFS Boot Environments, fault management, delegasi administratif, dan dukungan jails.
- **Jails**: isolasi layanan dan aplikasi dengan filesystem, jaringan, dan konfigurasi sendiri. Ini isolasi tingkat OS, bukan mesin virtual.
- **bhyve**: hypervisor bawaan dengan dukungan virtio, PCI passthrough, dan emulasi NVMe cepat.
- **Jaringan**: stack TCP/IP menjadi salah satu kekuatan utama dan alasan FreeBSD dipakai untuk router, firewall, dan layanan internet berbeban berat.
- **Keamanan**: kerangka Mandatory Access Control dan Capsicum (capability dan sandbox) tersedia di sistem dasar; relevan saat memperkuat host seperti yang dibahas di [[References/Server Security\|Server Security]].
- **Kompatibilitas Linux**: banyak binary Linux bisa berjalan tanpa virtualisasi.
- **Ports dan packages**: sekitar 37.000 port, plus paket siap pakai yang dipasang dengan `pkg install`; Ports Collection dipakai bila perlu build kustom.

## Lisensi dan model pengembangan

Lisensi BSD bersifat liberal: source code boleh diubah, diperluas, dan dimasukkan ke produk open source maupun tertutup tanpa kewajiban copyleft. Proyek tetap menerima komponen GPL, tetapi lebih memilih lisensi BSD bila pilihannya setara. Seluruh komponen kernel dan userspace berada dalam satu repositori dengan build system terpadu, warisan filosofi Unix yang mengutamakan komposisi alat kecil alih-alih daemon monolitik.

Pengembangan berjalan lewat *committers* yang punya hak push, *core team* yang dipilih setiap dua tahun, dan FreeBSD Foundation sebagai organisasi nirlaba 501(c)(3) di Amerika Serikat. Riwayat source control: CVS, lalu SVN pada 2008 (dokumentasi dan ports menyusul 2012), lalu Git untuk source dan dokumentasi pada Desember 2020 dan ports pada April 2021.

## Sejarah singkat

Proyek ini lahir awal 1993 dari tiga koordinator terakhir patchkit 386BSD: Nate Williams, Rod Grimes, dan Jordan Hubbard. Nama "FreeBSD" dicetuskan David Greenman. FreeBSD 1.0 terbit Desember 1993 di atas 4.3BSD-Lite ("Net/2"); setelah penyelesaian sengketa hukum Novell dan U.C. Berkeley, proyek bermigrasi ke 4.4BSD-Lite dan merilis FreeBSD 2.0 pada Desember 1994, disusul 2.0.5 pada Juni 1995. Pengembangan jangka panjang kini berada di cabang 16.0-CURRENT.

## Dokumentasi dan memulai

Dokumentasi diperlakukan sebagai bagian inti sistem. Sumber utamanya FreeBSD Handbook dan FAQ; Porter's Handbook untuk framework Ports; Documentation Project Primer untuk kontribusi dokumentasi; serta manual page yang mencakup userspace, driver (section 4), dan API driver kernel (section 9). Dokumentasi bisa dipasang offline dengan `pkg install en-freebsd-doc`, yang menaruh Handbook dan FAQ di `/usr/local/share/doc/freebsd`; versi terjemahan kadang tertinggal dari versi Inggris.

Untuk pemula, halaman *Resources for Newbies* menunjuk Handbook, FAQ, dan manual page sebagai bacaan wajib, lalu mailing list `freebsd-questions`, forum, dan halaman Support sebagai saluran bertanya. Instalasi memakai `bsdinstall`, dengan media CD/DVD atau jaringan (FTP/NFS). Portal dokumentasi menyediakan pintu masuk *Getting started* (Handbook Basics), Handbook, Porter's Handbook, FAQ, Books, Articles, arsip manual page, dan arsip dokumentasi historis.

## Ekosistem turunan

FreeBSD dipakai sebagai fondasi sistem lain, baik komersial maupun open source. Tiga contoh yang disebut halaman Newbies: **GhostBSD** (desktop GTK), **MidnightBSD** (desktop untuk pengguna harian), dan **pfSense** (solusi keamanan jaringan dan firewall berbasis FreeBSD).

## Catatan sumber

Halaman beranda, portal dokumentasi, dan Resources for Newbies sebagian besar berupa ringkasan dan daftar tautan; isi substantif di halaman ini diverifikasi dari *About FreeBSD* dan Handbook Bab 1, sedangkan daftar turunan dan saluran dukungan hanya dicatat sebagai tautan yang disebut sumber, bukan diuji. Status versi di beranda adalah snapshot: pada 21 November 2026 disebut 15.2-BETA3 dan cabang 16.0-CURRENT, jadi angka itu cepat usang dan beta bukan target produksi.

## Sumber

- [The FreeBSD Project](https://www.freebsd.org/), beranda, diakses 26 November 2026.
- [About FreeBSD](https://www.freebsd.org/about/), diakses 26 November 2026.
- [FreeBSD Handbook, Chapter 1: Introduction](https://docs.freebsd.org/en/books/handbook/introduction/), diakses 26 November 2026.
- [Resources for Newbies](https://www.freebsd.org/projects/newbies/), diakses 26 November 2026.
- [FreeBSD Documentation Portal](https://docs.freebsd.org/en/), diakses 26 November 2026.
