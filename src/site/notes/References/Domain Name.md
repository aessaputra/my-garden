---
{"dg-publish":true,"dg-path":"Domain Name.md","permalink":"/domain-name/","title":"Domain Name","hideInFiletree":true,"tags":["network","dns","security","guide"],"dg-note-properties":{"title":"Domain Name","category":"references","tags":["network","dns","security","guide"],"sources":["_raw/articles/domain-name-mdn.md","_raw/articles/domain-name-cloudflare.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

Domain name adalah alamat website yang bisa dibaca manusia, dipetakan ke IP address melalui sistem DNS. Halaman ini menyintesis 2 sumber: struktur dan cara kerja dari MDN, plus aspek registrasi dan keamanan dari Cloudflare.

## Definisi

Domain name adalah string teks unik dan mudah diingat untuk mengakses website, contoh `google.com`. Alamat sebenarnya website adalah IP address numerik (`192.0.2.2`), tapi berkat DNS, pengguna cukup mengetik nama yang ramah manusia. Proses penerjemahan ini disebut DNS lookup.

## Struktur domain name

Dibaca dari kanan ke kiri, dari paling umum ke paling spesifik. Dipisahkan titik.

- TLD (Top-Level Domain): bagian paling kanan. Generik (`.com`, `.net`, `.org`) tanpa syarat; `.gov` untuk pemerintah; `.edu` untuk institusi pendidikan; TLD negara (`.us`, `.jp`) bisa mensyaratkan bahasa/lokasi. Maksimum 63 karakter, daftar lengkap dikelola ICANN.
- Label: bagian di kiri TLD, 1-63 karakter, huruf/angka/`-` (tidak boleh di awal/akhir). Label tepat sebelum TLD disebut SLD (Second-Level Domain).
- Subdomain: di bawah domain yang kamu kontrol, misal `developer.mozilla.org`, `support.mozilla.org`.

Contoh `google.co.uk`: `.uk` TLD, `.co` 2LD (indikasi perusahaan), `google` 3LD (paling spesifik).

## Domain name vs URL

URL berisi domain name plus protokol dan path: `https://cloudflare.com/learning/` = protokol `https` + domain `cloudflare.com` + path `/learning/`.

## Registrasi domain

- Kamu tidak membeli domain: kamu menyewa hak pakai 1+ tahun dan bisa memperbarui dengan prioritas. Tidak pernah memiliki domain.
- Registrars (perusahaan) berkoordinasi dengan domain registries untuk melacak info pendaftar.
- Cek ketersediaan via layanan whois di situs registrar, atau perintah `whois domain.example` di shell. Output `NOT FOUND` berarti domain tersedia.
- Setelah daftar, dalam beberapa jam semua DNS server menerima informasi DNS. Refresh tidak instan karena tiap DNS server menyimpan info sementara sebelum invalidate dan query ulang ke authoritative name server.
- Ada 300+ juta domain terdaftar di dunia.

## Keamanan dan praktik domain

- Domain squatting: mendaftarkan domain dengan itikad buruk untuk dijual kembali, kadang phishing. Registrar predator bisa membeli domain yang kedaluwarsa lalu menjualnya mahal ke pemilik asli.
- Domain privacy (WHOIS privacy): menyembunyikan kontak registran dari publik untuk mengurangi spam/pelecehan.
- Domain hijacking: pihak jahat mengambil alih domain.
- Pilih registrar tepercaya; bandingkan biaya dan biaya perpanjangan (markup bisa besar).
- Premium domain: lebih mahal karena nilai persepsi (mudah diingat, cocok marketing/bisnis).
- Transfer domain: bisa setelah 60 hari terdaftar; butuh authorization code dan lepas domain locks.

## Cara kerja DNS lookup

1. Ketik `mozilla.org` di browser.
2. Cek local DNS cache komputer. Jika ada, langsung dapat IP.
3. Jika tidak, komputer bertanya ke DNS server.
4. DNS server mengembalikan IP; browser bernegosiasi konten dengan web server.

## Lihat juga

- [[What is Internet\|What is Internet]]: IP address dan DNS sebagai dasar cara kerja internet
- [[References/How Does Internet Work\|How Does Internet Work]]: IP, DNS, TCP/IP, dan perjalanan dari dua komputer ke jaringan global
- [[References/HTTP\|HTTP]]: HTTP bekerja di atas IP/DNS untuk mengakses website
- [[InterPlanetary Name System\|InterPlanetary Name System]]: alternatif sistem penamaan terdesentralisasi (IPFS/DNSLink)