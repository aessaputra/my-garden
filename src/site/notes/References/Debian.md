---
{"dg-publish":true,"dg-path":"Debian.md","permalink":"/debian/","title":"Debian","hideInFiletree":true,"tags":["references","devops","security"],"noteIcon":"","dg-note-properties":{"title":"Debian","aliases":["Debian GNU/Linux","The Debian Project"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","security"],"sources":["_raw/articles/debian-home-2026-11-26.md","_raw/articles/debian-about-2026-11-26.md","_raw/articles/debian-intro-2026-11-26.md","_raw/articles/debian-releases-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

Debian adalah sistem operasi bebas yang dikembangkan dan dipelihara oleh Debian Project, asosiasi individu yang sepakat membuat satu sistem operasi bersama. Di inti sistem ada kernel: Debian memakai kernel Linux atau kernel FreeBSD, sementara port ke GNU Hurd masih dalam pengerjaan. Sebagian besar alat dasar berasal dari proyek GNU, karena itu namanya GNU/Linux atau GNU/Hurd. Di atas kernel dan alat dasar itu ada lebih dari 70.000 paket aplikasi yang dikelola APT, sehingga satu mesin dan ribuan mesin bisa dipasang perangkat lunak dengan cara yang sama.

## Rilis dan siklus hidup

Debian selalu memelihara sedikitnya tiga rilis sekaligus:

- **stable** — rilis produksi yang direkomendasikan. Versi saat ini Debian 13, kode *trixie*, dirilis sebagai 13.0 pada 9 Agustus 2025 dengan pembaruan terakhir 13.8 pada 21 November 2026.
- **testing** — paket yang belum masuk stable tetapi sedang diantrekan; versi saat ini *forky*, dengan nomor 14 yang belum punya tanggal rilis.
- **unstable** — tempat pengembangan aktif, selalu bernama *sid*, ditujukan untuk pengembang dan pengguna yang siap menghadapi perubahan besar.

Siklus hidup satu rilis lima tahun: tiga tahun dukungan penuh lalu dua tahun Long Term Support (LTS). Sesudah LTS berakhir, dukungan hanya tersedia lewat Extended LTS pihak ketiga yang berbayar.

| Versi | Kode | Rilis | EOL | Akhir LTS | Status |
|---|---|---|---|---|---|
| 13 | Trixie | 2025-08-09 | 2028-08-09 | 2030-06-30 | stable |
| 12 | Bookworm | 2023-06-10 | 2026-07-11 | 2028-06-30 | oldstable, di bawah LTS |
| 11 | Bullseye | 2021-08-14 | 2024-08-14 | 2026-08-31 | arsip, ELTS pihak ketiga |
| 14 | Forky | belum ditetapkan | — | — | testing |

Kode nama rilis diambil dari karakter Toy Story; versi 15 sudah diumumkan sebagai *duke*. Integritas data rilis dijaga file `Release` bertanda tangan digital: checksum semua file `Packages` disalin ke dalamnya, dan tanda tangannya disimpan di `Release.gpg` memakai kunci penandatangan arsip. Untuk stable dan oldstable ada tanda tangan tambahan dari kunci offline yang dibuat khusus oleh Stable Release Team.

## Paket dan APT

Debian mendefinisikan paket sebagai perangkat lunak terkompilasi yang dibungkus dalam format siap pasang. APT mengelola ribuan paket itu beserta dependensinya, dan menjadi contoh khas manajer paket tingkat OS yang dibandingkan dengan manajer tingkat bahasa di [[References/Package Managers\|Package Managers]]. Repositori resmi memuat lebih dari 70.000 paket, dari server web sampai perkakas pengembangan.

## Filosofi bebas

Kata "free" di Debian berarti kebebasan, bukan harga nol. Komitmen itu diformalkan dalam Social Contract. Debian tetap memberi jalan bagi perangkat lunak non-free bila pengguna membutuhkannya, termasuk paket yang tugasnya memasang perangkat lunak non-free ke sistem. Untuk perangkat keras, proyek menyarankan membeli dari produsen yang menyediakan driver bebas, karena driver non-free bisa hilang dukungannya sewaktu-waktu.

## Komunitas dan struktur

Debian dikerjakan oleh sekitar seribu pengembang aktif yang tersebar di seluruh dunia dan bekerja sebagai sukarelawan; sebagian besar belum pernah bertemu langsung. Komunikasi berjalan lewat mailing list dan IRC. Keanggotaan resmi dikelola di `nm.debian.org`, sedangkan daftar kontributor yang lebih luas ada di `contributors.debian.org`. Proyek punya struktur organisasi tertulis dan Developers' Corner untuk informasi internal.

## Perangkat keras dan arsitektur

Debian berjalan di hampir semua PC, termasuk model lama, dan setiap rilis umumnya menambah arsitektur yang didukung. Daftar resmi ada di dokumentasi rilis stable, sedangkan kecocokan perangkat bisa dicek lewat wiki DebianOn dan linux-hardware.org. Untuk mencoba tanpa mengganggu sistem yang ada, Debian bisa dipasang di ruang sekitar 2 GB, atau 600 MB bila tanpa desktop grafis.

## Dokumentasi, dukungan, dan keamanan

Pintu masuknya adalah halaman Documentation (installation guide, FAQ, HOWTO, wiki), Bug Tracking System, dan mailing list. Dukungan bisa lewat dokumentasi, mailing list, IRC, atau konsultan berbayar. Pengumuman keamanan dan arsip Debian Security Advisory tersedia di halaman Security, hal yang relevan saat host Debian masuk lingkup penguatan di [[References/Server Security\|Server Security]]. Halaman Why Debian memuat daftar pro dan kontra secara terbuka, bukan hanya pemasaran.

## Catatan sumber

Beranda Debian adalah halaman portal: judul bagian, tautan, dan lima berita terbaru, dengan snapshot 13.8 tertanggal 21 November 2026. Isi substantif di halaman ini diverifikasi dari About Debian, Introduction to Debian, dan Debian Releases; tautan seperti DebianOn, linux-hardware.org, dan halaman pengguna hanya dicatat sebagai rujukan yang disebut sumber, bukan diuji. Debian juga memakai kernel FreeBSD pada port tertentu, sehingga konsep kernel dan lisensi di [[References/FreeBSD\|FreeBSD]] berguna sebagai pembanding.

## Sumber

- [Debian -- The Universal Operating System](https://www.debian.org/), beranda, diakses 26 November 2026.
- [About Debian](https://www.debian.org/intro/about), diakses 26 November 2026.
- [Introduction to Debian](https://www.debian.org/intro/), diakses 26 November 2026.
- [Debian Releases](https://www.debian.org/releases/), diakses 26 November 2026.
