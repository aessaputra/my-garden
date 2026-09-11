---
{"dg-publish":true,"dg-path":"Nginx.md","permalink":"/nginx/","title":"Nginx","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Nginx","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/nginx-user-summary-2026-09-11.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Nginx (dibaca engine-x) adalah perangkat lunak web server yang juga dapat berperan sebagai reverse proxy. Nginx bersifat open source dengan lisensi BSD dua klausa, sedangkan NGINX Plus adalah produk komersial dengan fitur tambahan. Pemakaian dan lisensi perlu diperiksa per versi dan produk, bukan diasumsikan seragam.

## Peran umum

- **Web server**: menyajikan file statis dan meneruskan request dinamis melalui antarmuka seperti FastCGI atau proxy.
- **Reverse proxy**: menerima request dari client dan meneruskannya ke layanan backend sesuai aturan konfigurasi.
- **Load balancer**: membagi request ke beberapa backend menurut metode seperti round-robin, least connections, atau hash IP.
- **HTTP cache**: menyimpan response dari backend untuk digunakan kembali melalui mekanisme `proxy_cache`, dengan kebijakan mengikuti aturan HTTP yang dipatuhi. [[References/Cache-Control\|Cache-Control]] menjelaskan header yang berperan di sini.
- **Mail proxy**: proxy untuk SMTP, IMAP, dan POP3 dengan autentikasi melalui backend.
- **Stream proxy**: meneruskan koneksi TCP dan UDP.

[[References/Web Servers\|Web Servers]] menjelaskan peran-peran tersebut secara umum; halaman ini berfokus pada implementasi Nginx.

## Arsitektur dan konfigurasi

Nginx menjalankan proses master yang membaca konfigurasi dan mengawasi worker. Worker bersifat event-driven dan non-blocking sehingga satu worker dapat menangani banyak koneksi tanpa satu thread per koneksi.

Konfigurasi ditulis dalam file deklaratif, dikelompokkan dalam blok seperti `http`, `server`, dan `location`. Perubahan diterapkan dengan mekanisme reload yang memungkinkan proses lama menyelesaikan koneksi, sehingga restart penuh sering tidak diperlukan.

## Batas performa dan operasional

Karakteristik event-driven menjadikan Nginx efisien untuk banyak pola beban kerja, terutama koneksi bersamaan yang tidak selalu aktif mentransfer data. Bukan berarti Nginx selalu lebih cepat daripada server lain pada setiap skenario; hasil bergantung pada pola request, modul yang aktif, perangkat keras, dan konfigurasi. Ukur workload sendiri sebelum menyimpulkan.

Kesalahan konfigurasi dapat menimbulkan risiko keamanan, misalnya path traversal pada alias atau direktori, log yang membocorkan data, dan TLS yang lemah. Ikuti panduan pengamanan resmi, perbarui versi secara teratur, dan batasi modul yang diaktifkan.

[[References/Deployment\|Deployment]] dan [[References/Caching\|Caching]] menghubungkan peran Nginx dengan proses rilis dan strategi cache yang lebih luas.

## Sumber

Paste pengguna sebagai konteks awal. Dokumentasi berikut menjadi rujukan pemeriksaan implementasi, bukan snapshot yang diverifikasi pada ingest ini:

- [NGINX Documentation](https://nginx.org/en/docs/): modul, konfigurasi, dan panduan pengoperasian.
