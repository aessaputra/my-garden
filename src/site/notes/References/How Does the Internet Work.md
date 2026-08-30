---
{"dg-publish":true,"permalink":"/references/how-does-the-internet-work/","title":"How Does the Internet Work","tags":["network","architecture","guide"],"dg-note-properties":{"title":"How Does the Internet Work","category":"references","tags":["network","architecture","guide"],"sources":["_raw/articles/how-does-internet-work-mdn.md"],"created":"2026-08-21","updated":"2026-08-21"}}
---

Artikel MDN tentang infrastruktur dasar Internet. Penjelasan paling visual dan bertahap: dari dua komputer sampai jaringan global. Poin uniknya: perbedaan jelas antara Internet (infrastruktur) dan Web (layanan), plus konsep intranet/extranet.

## Ringkasan

Internet adalah backbone dari Web. Dimulai tahun 1960-an sebagai proyek riset didanai Angkatan Darat AS, berkembang jadi infrastruktur publik di 1980-an. Cara kerjanya tetap: menghubungkan komputer dan memastikan mereka tetap terhubung apa pun yang terjadi.

## Dari dua komputer ke jaringan besar

- Dua komputer: hubungkan dengan kabel Ethernet atau nirkabel (Wi-Fi/Bluetooth).
- Banyak komputer: perlu switch. Switch memastikan pesan hanya sampai ke tujuan yang benar (seperti operator stasiun). 10 komputer cukup 10 kabel dengan switch.
- Jaringan lokal kecil: switch saja tidak bisa untuk miliaran komputer. Hubungkan jaringan lokal dengan router, yang meneruskan pesan antar jaringan seperti kantor pos.
- Ke internet: router dihubungkan lewat infrastruktur telepon menggunakan modem. Lalu jaringan dihubungkan ke ISP (Internet Service Provider), yang mengelola router saling terhubung dan bisa mengakses router ISP lain.

## Konsep penting

- IP address: alamat unik empat angka bertitik, contoh `192.0.2.172`, mengidentifikasi tiap komputer.
- Domain name: alias manusiawi untuk IP address, contoh `google.com` untuk `142.250.190.78`.
- Internet vs Web: Internet adalah infrastruktur teknis; Web adalah layanan di atasnya. Layanan lain di atas Internet: email, IRC.
- Intranet: jaringan privat untuk anggota organisasi (portal, shared drive, wiki, diskusi).
- Extranet: intranet yang dibuka sebagian untuk organisasi mitra/klien.
- Keduanya memakai infrastruktur dan protokol yang sama dengan Internet.

## Lihat juga

- [[References/What is Internet\|What is Internet]]: dasar internet dari roadmap.sh (medium fisik, IP/DNS, packet routing)
- [[References/How Does Internet Work\|How Does Internet Work]]: panduan teknis cs.fyi (TCP/IP ports/sockets, SSL/TLS detail, tren masa depan)
- [[References/Jaringan (Network)\|Jaringan (Network)]]: networking di React Native (Fetch API, WebSocket)