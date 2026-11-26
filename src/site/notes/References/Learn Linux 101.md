---
{"dg-publish":true,"dg-path":"Learn Linux 101.md","permalink":"/learn-linux-101/","title":"Learn Linux 101","hideInFiletree":true,"tags":["references","devops"],"noteIcon":"","dg-note-properties":{"title":"Learn Linux 101","aliases":["Learn Linux 101","LPIC-1 Exam 101 roadmap"],"categories":["Software Systems"],"type":"reference","tags":["references","devops"],"sources":["_raw/articles/ibm-learn-linux-101-series-2026-11-26.md","_raw/articles/ibm-learn-linux-101-map-2026-11-26.md","_raw/articles/lpic1-101-1-hardware-settings-2026-11-26.md","_raw/articles/lpic1-103-1-command-line-2026-11-26.md","_raw/articles/lpic1-104-5-file-permissions-2026-11-26.md","_raw/articles/lpi-lpic1-objectives-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

**Learn Linux 101** adalah seri tutorial IBM Developer (penulis Ian Shields) yang memetakan materi LPIC-1 Exam 101. Tujuannya bukan mengajar dari nol, tetapi menyediakan satu tutorial per objective ujian, sehingga bisa dipakai sebagai jalur belajar terstruktur atau sebagai indeks topik administrasi Linux. Halaman ini merangkum struktur ujian dan isi tutorial yang sudah diperiksa, bukan menggantikan latihan di sistem nyata.

## Struktur ujian

LPIC-1 memerlukan dua ujian, 101 dan 102; keduanya harus lulus. Exam 101 terdiri dari empat topik dengan bobot total 60:

- **101 System architecture** (8): 101.1 hardware settings (2), 101.2 boot (3), 101.3 runlevel/boot target (3).
- **102 Installation and package management** (12): 102.1 layout disk (2), 102.2 boot manager (2), 102.3 shared library (1), 102.4 paket Debian (3), 102.5 RPM/YUM/Zypper (3), 102.6 Linux sebagai guest virtualisasi (1).
- **103 GNU and UNIX commands** (26): 103.1 command line (4), 103.2 filter teks (2), 103.3 file dan direktori (4), 103.4 stream/pipe/redirect (4), 103.5 proses (4), 103.6 prioritas (2), 103.7 regex (3), 103.8 editor vi (3).
- **104 Devices, filesystems, FHS** (14): 104.1 partisi dan filesystem (2), 104.2 integritas filesystem (2), 104.3 mount (3), 104.5 permission dan ownership (3), 104.6 hard/symbolic link (2), 104.7 FHS (2).

Kuota disk (104.4) dihapus dari objectives versi 5; halaman LPI menandainya "Removed". Exam 102 melanjutkan dengan topik 105 shells/scripting, 106 UI dan desktop, 107 tugas administratif, 108 layanan sistem, 109 jaringan, dan 110 keamanan; seri ini fokus pada Exam 101.

## Status tutorial

Roadmap mengikuti objectives versi 5.0 (29 Oktober 2018). Sebagian tutorial masih ditulis untuk objectives versi 4.0 (15 April 2015) dan ditandai **Version 4**, misalnya 102.4, 102.5, 103.5, 103.7, 103.8, 104.3, 105.2, 107.2, dan 108.1. Dua objective berstatus "Coming soon": 102.6 dan 110.2. Objective 105.3 (SQL) juga sudah obsolete di versi 5. Halaman objectives LPI yang diperiksa 26 November 2026 mengonfirmasi versi 5.0 masih versi rilis saat ini, dan mencantumkan rencana perubahan: menghapus `ifup`/`ifdown` dan perintah `net-tools` lama, menghapus TCP wrappers, serta mencatat bahwa `textutils` kini bagian dari `coreutils`.

Contoh di tutorial memakai distro lama: Fedora 22 (kernel 4.0.4), CentOS 6 (2.6.32), Ubuntu 14.04 sampai 16.04, dan Fedora 13. Sintaks perintahnya tetap relevan, tetapi detail versi dan nama perangkat bisa berbeda di sistem sekarang.

## Isi yang diperiksa

Tiga tutorial dibaca penuh, sisanya hanya terdaftar di roadmap:

- **103.1 Command line** — builtin versus perintah eksternal, token dan quoting (single quote tidak mengekspansi variabel), `echo -n`/`-e` dan escape, metakarakter, operator kontrol (`;`, `&&`, `||`, `&`, `;;`, `|`, `()`), exit code 0/non-zero, subshell; variabel environment dan `export` (tidak terlihat oleh parent), `env -i`/`-u`, `unset`, `set -u`, `exec` yang menggantikan shell; opsi `uname`; history (`!`, `!N`, `!-N`, `!string`, `!?string?`, `HISTSIZE`, `~/.bash_history`, sesi paralel saling menimpa, tidak digabung); `which` versus `type` (alias/builtin); `PATH`, path absolut/relatif, `.`, `..`, `~`, `CDPATH`, `OLDPWD`; bagian man 1-8 plus 9/n/o/l, `whatis`, `apropos`, `man -a`, pager `less`, builtin `help`, `info`, dan `--help`. Materi ini bertumpang tindih dengan [[References/Learn Shell\|Learn Shell]].
- **101.1 Hardware settings** — setting BIOS/UEFI (urutan boot, secure boot dan key), jenis media penyimpanan (PATA/SATA/SCSI/ATAPI, USB/Thunderbolt, SSD M.2/NVMe), `sysfs` di `/sys`, `procfs` di `/proc` (`interrupts`, `dma`, `ioports`), udev dan aturan di `/usr/lib/udev/rules.d` serta `/etc/udev/rules.d`, tipe entri `/dev` (b/c/d/l), `lspci` (`-n`, `-q`, `-s`, `-t`, `-d`, `-k`), `lsusb -t`, `usbview`, D-Bus; modul kernel dengan `lsmod`, `modinfo`, `modprobe` (`-a`, `-r`, `-n`, `-v`) termasuk modul built-in dan parameter modul.
- **104.5 File permissions** — `whoami`, `groups`, `su`, `sudo -s`; `chmod` simbolik dan oktal, `umask`, `chown`, `chgrp`, bit suid/sgid/sticky; ACL disebut di luar cakupan objective. Ini berhubungan langsung dengan topik keamanan host di [[References/Server Security\|Server Security]].

Topik 103.2 dan 103.4 (filter teks serta pipe/redirect) beririsan dengan [[References/Linux Text Processing Command\|Linux Text Processing Command]]. Topik 102.4 dan 102.5 (paket Debian serta RPM/YUM) beririsan dengan [[References/Package Managers\|Package Managers]], [[References/Debian\|Debian]], dan [[References/Red Hat Enterprise Linux\|Red Hat Enterprise Linux]].

## Catatan sumber

Seri ini terakhir diperbarui 24 Februari 2022 menurut halaman seri, sementara roadmap-nya masih memakai objectives 2018 dan sebagian tutorialnya dari 2015. Karena itu halaman ini `confidence: medium`: strukturnya cocok dengan objectives resmi LPI, tetapi status kelengkapan tutorial adalah klaim IBM yang bisa berubah. Roadmap IBM adalah indeks; isi yang diringkas di atas hanya dari tiga tutorial yang benar-benar dibaca, sedangkan objective lain hanya terdaftar. Seri ini alat belajar, bukan jaminan lulus ujian.

## Sumber

- [Learn Linux 101: A roadmap for LPIC-1 certification](https://developer.ibm.com/series/learn-linux-101/), IBM Developer, Ian Shields; halaman seri, 24 Februari 2022.
- [Learn Linux, 101: A roadmap for LPIC-1](https://developer.ibm.com/tutorials/l-lpic1-map/), peta tutorial per objective, termasuk penanda Version 4 dan "Coming soon".
- [Prepare for LPIC-1 exam 1 - topic 101.1: Configure hardware settings](https://developer.ibm.com/tutorials/l-lpic1-101-1/), IBM Developer.
- [Prepare for LPIC-1 exam 1 - topic 103.1: The Linux command line](https://developer.ibm.com/tutorials/l-lpic1-103-1/), IBM Developer.
- [Prepare for LPIC-1 exam 1 - topic 104.5: Manage file permissions and ownership](https://developer.ibm.com/tutorials/l-lpic1-104-5/), IBM Developer.
- [LPIC-1 Exam 101 and 102 Objectives](https://www.lpi.org/our-certifications/exam-101-objectives/), Linux Professional Institute, versi 5.0; diperiksa 26 November 2026.
