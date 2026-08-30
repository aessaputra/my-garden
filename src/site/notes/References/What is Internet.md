---
{"dg-publish":true,"permalink":"/references/what-is-internet/","title":"What is Internet","hideInFiletree":true,"tags":["network","http","ssl","security","guide"],"dg-note-properties":{"title":"What is Internet","category":"references","tags":["network","http","ssl","security","guide"],"sources":["_raw/articles/what-is-internet.md"],"created":"2026-08-21","updated":"2026-08-21"}}
---

Internet adalah jaringan global komputer yang saling terhubung dan berkomunikasi lewat protokol standar. Tidak ada satu orang atau organisasi yang mengendalikannya. Penjelasan ini berasal dari Vint Cerf, salah satu perintis internet.

## Cara informasi bergerak

Data bergerak dari satu komputer ke komputer lain sebagai bits melalui medium fisik: kabel Ethernet, fiber optic, atau sinyal radio (Wi-Fi). Tiap medium punya trade-off sendiri antara kecepatan, jangkauan, dan biaya.

## IP address dan DNS

IP address mengidentifikasi tiap komputer di jaringan. DNS menerjemahkan nama seperti `roadmap.sh` menjadi alamat IP. Keduanya adalah protokol dasar yang membuat internet bekerja.

## Packet, routing, dan reliabilitas

Informasi dikirim dalam bentuk packet dan tidak harus lewat jalur tetap; rute bisa berubah di tengah transfer. Router menentukan jalur yang diambil. Karena banyak jalur tersedia, jaringan tetap andal meski satu jalur gagal.

## HTTP dan HTML

HTTP adalah protokol standar untuk mentransfer halaman web. HTML adalah format dokumen yang dirender browser.

## Enkripsi dan public key

Cryptography menjaga komunikasi tetap aman di internet. SSL/TLS mengenkripsi koneksi, dan public key mendasari pertukaran kunci yang aman.

## Cybersecurity

Cybersecurity adalah langkah protektif terhadap aktivitas kriminal lewat jaringan dan perangkat. Ini mencakup pemahaman cybercrime umum seperti phishing dan malware.

## Lanjutan

- DNS guide roadmap.sh: "How a website is found on the Internet"
- howdns.works
- Pengantar DNS over HTTPS dalam bentuk kartun

## Lihat juga

- [[References/Jaringan (Network)\|Jaringan (Network)]]: networking di React Native (Fetch API, WebSocket), implementasi praktis dari konsep ini
- [[InterPlanetary Name System\|InterPlanetary Name System]]: alternatif sistem penamaan terdesentralisasi (IPFS/DNSLink)