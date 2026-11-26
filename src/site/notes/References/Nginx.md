---
{"dg-publish":true,"dg-path":"Nginx.md","permalink":"/nginx/","title":"Nginx","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Nginx","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/nginx-user-summary-2026-09-11.md","_raw/articles/swissmade-nginx-ubuntu-setup-2026-11-26.md"],"created":"2026-09-11","updated":"2026-11-26","confidence":"medium"}}
---

Nginx adalah perangkat lunak web server yang juga dapat berperan sebagai reverse proxy. Nginx bersifat open source dengan lisensi BSD dua klausa, sedangkan NGINX Plus adalah produk komersial dengan fitur tambahan.

## Peran umum

- **Web server**: menyajikan file statis dan meneruskan request dinamis melalui antarmuka seperti FastCGI atau proxy.
- **Reverse proxy**: menerima request dari client dan meneruskannya ke layanan backend sesuai aturan konfigurasi; konsep dan batasnya dibahas di [[References/Proxy Server\|Proxy Server]].
- **Load balancer**: membagi request ke beberapa backend menurut metode seperti round-robin, least connections, atau hash IP. Algoritma dan batasnya dibahas di [[References/Load Balancing\|Load Balancing]].
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

## Setup Ubuntu

Panduan SwissMade.Host (Oktober 2023) memasang Nginx dari repository Ubuntu, membuka `Nginx Full` di UFW, lalu membuat server block di `sites-available` dan symlink ke `sites-enabled`. Alur intinya: `apt install nginx`, `ufw allow 'Nginx Full'`, server block `listen 80` dengan `root /var/www/<situs>` dan `try_files $uri $uri/ =404`, `nginx -t`, lalu `systemctl reload nginx`.

TLS memakai Certbot: `certbot --nginx -d domain -d www.domain` mengubah konfigurasi dan menambah listener 443. Pengerasan yang disarankan: `server_tokens off`, `ssl_protocols TLSv1.2 TLSv1.3`, HSTS, basic auth, rate limit per IP, Fail2Ban, gzip, dan cache statis. Panduan ini ditulis untuk Ubuntu 2023 dan tidak menyebut versi Nginx.

## Koreksi terhadap panduan SwissMade.Host

1. **PPA Certbot tidak perlu.** Ubuntu sudah menyediakan `certbot` dan `python3-certbot-nginx` di universe; di mesin ini kandidatnya 2.9.0. Certbot upstream menyarankan snap dan meminta paket apt lama dihapus agar tidak bentrok, bukan menambah PPA.
2. **Cron renew duplikat.** Paket Certbot sudah memasang systemd timer atau cron untuk renew; menambah `0 2 * * * certbot renew` sendiri tidak diperlukan. Uji dengan `certbot renew --dry-run`, bukan menjadwalkan ulang.
3. **HSTS `preload` bukan sekadar header.** Browser hanya menghormati header yang dikirim lewat HTTPS; `preload` baru berlaku setelah didaftarkan ke hstspreload.org, butuh `max-age` minimal 1 tahun dan `includeSubDomains`, dan `includeSubDomains` bisa memutus subdomain yang belum HTTPS. Tambahkan `always` agar header tetap terkirim pada response error.
4. **Basic auth `apr1` lemah.** `openssl passwd -apr1` memakai MD5 cepat. Nginx menerima crypt(), apr1, dan SHA; gunakan `openssl passwd -6` (SHA-512 crypt) dan batasi percobaan login.
5. **Cache statis menulis dua header.** `expires 30d` sudah menghasilkan `Cache-Control: max-age=2592000`; menambah `add_header Cache-Control "public, no-transform"` membuat header kedua. Pilih satu mekanisme, dan aset 30 hari sebaiknya memakai nama ber-hash agar tidak basi.
6. **Rate limit hanya per IP.** `$binary_remote_addr` di belakang proxy adalah IP proxy, jadi butuh `real_ip`; `nodelay` membiarkan burst 20 request langsung; botnet terdistribusi tetap lolos.
7. **Situs default bisa menutupi server block.** Selama symlink `sites-enabled/default` dengan `default_server` masih ada, request ke IP atau Host tak dikenal masuk ke sana. Hapus symlink itu atau tandai server block baru sebagai `default_server`; tambahkan juga `listen [::]:80`.
8. **Fail2Ban tidak punya jail Nginx bawaan.** Menyalin `jail.conf` ke `jail.local` benar, tetapi filter dan jail Nginx harus dibuat atau dipilih (`nginx-http-auth` untuk basic auth); panduan hanya menyuruh mencari konfigurasi di forum.

## Sumber

- [NGINX Documentation](https://nginx.org/en/docs/): modul, konfigurasi, dan panduan pengoperasian.
- [Basic NGINX Setup: Ubuntu Guide to a Functional and Secure Website Serving](https://swissmade.host/en/blog/basic-nginx-setup-ubuntu-guide-to-a-functional-and-secure-website-serving), SwissMade.Host, 9 Oktober 2023.
- Verifikasi lokal: `nginx` tidak terpasang; `apt-cache policy nginx` kandidat 1.24.0-2ubuntu7.21 dan `certbot` 2.9.0 pada Pop!_OS 24.04. Rujukan koreksi: dokumentasi `ngx_http_auth_basic_module`, `ngx_http_headers_module`, `ngx_http_limit_req_module`, MDN HSTS, dan panduan instalasi Certbot.
