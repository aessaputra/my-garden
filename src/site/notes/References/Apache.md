---
{"dg-publish":true,"dg-path":"Apache.md","permalink":"/apache/","title":"Apache","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Apache","aliases":["Apache HTTP Server","httpd"],"categories":["Software Systems"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/apache-research-packet-2026-09-12.md"],"created":"2026-09-12","updated":"2026-09-12","confidence":"high"}}
---

Apache HTTP Server, sering disebut Apache atau httpd, adalah web server open source dari Apache Software Foundation. Server ini berjalan pada Linux, Windows, macOS, dan sistem Unix-like. Apache di sini bukan nama untuk seluruh proyek yayasan tersebut.

Dalam keluarga [[References/Web Servers\|Web Servers]], Apache dapat menyajikan konten statis, meneruskan request ke aplikasi, menjadi reverse proxy, dan mengakhiri koneksi TLS. Fitur yang tersedia bergantung pada modul dan konfigurasi yang diaktifkan.

## Modul dan model concurrency

Modul menambahkan kemampuan seperti autentikasi, URL rewriting, TLS, dan proxy. Modul dapat disertakan saat kompilasi atau dimuat secara dinamis. Multi-Processing Module atau MPM menentukan cara server menerima koneksi dan membagi pekerjaan.

Tepat satu MPM harus aktif pada satu instance server. Pilihannya mencakup:

- `prefork`: model berbasis proses tanpa thread, berguna ketika kompatibilitas perangkat lunak memerlukannya.
- `worker`: model gabungan beberapa proses dan beberapa thread.
- `event`: turunan worker yang memindahkan penanganan koneksi idle keep-alive ke listener sehingga thread worker dapat menerima pekerjaan lain.
- `mpm_winnt`: MPM yang memakai fasilitas native Windows.

Apache tidak selalu memakai satu proses untuk setiap koneksi. Pada sistem Unix modern yang mendukung threads dan polling thread-safe, pilihan build default umumnya `event`; paket distribusi dan instalasi aktual tetap perlu diperiksa.

MPM event juga tidak membuat seluruh pemrosesan aplikasi menjadi non-blocking. Filter tertentu dan response dinamis dapat tetap menahan worker. Kapasitas ditentukan pula oleh `MaxRequestWorkers`, memori, koneksi backend, serta durasi request.

## Virtual hosting dan konfigurasi

Virtual hosting memungkinkan beberapa situs dilayani oleh satu instance Apache. Untuk name-based hosting, server terlebih dahulu mencocokkan IP dan port, kemudian nama host dengan `ServerName` atau `ServerAlias` pada kandidat virtual host.

Bila tidak ada nama yang cocok, virtual host pertama pada kelompok IP dan port tersebut menjadi fallback. Tentukan konfigurasi fallback secara sengaja agar request dengan nama yang tidak dikenal tidak memperoleh situs yang keliru. Konfigurasi virtual host tidak otomatis membuat record DNS.

File `.htaccess` memungkinkan perubahan per direktori tanpa mengedit konfigurasi utama. Kemampuan ini berguna ketika pengelola konten tidak memiliki akses administratif, tetapi bukan syarat wajib menjalankan Apache.

Jika konfigurasi utama dapat diakses, dokumentasi menyarankan menaruh aturan di sana. Pencarian dan pembacaan `.htaccess` menambah kerja per request, sedangkan delegasi konfigurasi menambah risiko. Batasi directive lewat `AllowOverride` dan `AllowOverrideList` bila delegasi diperlukan.

## TLS, proxy, dan LAMP

`mod_ssl` menyediakan integrasi TLS melalui OpenSSL. Mengaktifkan modul saja belum menyediakan [[References/HTTPS\|HTTPS]] yang benar: sertifikat, private key, nama host, dan versi protokol yang diterima harus dikonfigurasi. Istilah SSL pada nama modul bukan anjuran memakai protokol SSL lama.

Untuk reverse proxy HTTP, `mod_proxy` bekerja bersama modul protokol seperti `mod_proxy_http`. Load balancing memerlukan modul balancer dan algoritmenya. Reverse proxy tidak membutuhkan `ProxyRequests On`; mengaktifkan forward proxy tanpa pembatasan dapat menciptakan open proxy.

Apache merupakan komponen A dalam LAMP: Linux, Apache, MySQL, dan PHP/Perl/Python. Itu susunan teknologi yang lazim, bukan ketergantungan wajib. Apache tidak harus berjalan di Linux atau memakai database dan bahasa tersebut.

PHP juga tidak harus dijalankan di dalam proses Apache. `mod_proxy_fcgi` dapat meneruskan request ke PHP-FPM, yang prosesnya dikelola terpisah. Modul ini tidak memulai proses aplikasi sendiri. Ukuran pool Apache dan kapasitas PHP-FPM perlu diselaraskan agar koneksi persisten tidak menghabiskan worker backend.

## Operasi dan pemilihan

Dalam [[References/Deployment\|Deployment]], periksa sintaks dengan `apachectl configtest` sebelum perubahan layanan. `apachectl graceful` membaca ulang konfigurasi melalui graceful restart tanpa langsung memutus koneksi yang terbuka. Nama command dan lokasi konfigurasi dapat berbeda menurut paket OS.

Lulus pemeriksaan sintaks bukan bukti bahwa routing, sertifikat, permissions, atau backend sudah benar. Uji request untuk setiap virtual host, periksa access log dan error log, lalu amati resource dan timeout setelah perubahan.

Perbarui server, modul, runtime aplikasi, dan OS. Batasi hak tulis konfigurasi serta log, berikan akses filesystem hanya pada direktori yang diperlukan, dan atur batas ukuran maupun waktu request sesuai kebutuhan. Kontrol ini tidak menjamin bebas DoS atau kerentanan aplikasi.

Dibanding [[References/Nginx\|Nginx]], alasan memilih Apache dapat berupa kebutuhan modul, delegasi `.htaccess`, integrasi aplikasi, atau pengalaman operasional tim. Tidak ada kesimpulan bahwa salah satunya selalu lebih cepat atau lebih stabil tanpa workload dan pengukuran yang sebanding.

Popularitas dan dukungan komunitas bukan bukti kausal bahwa Apache bertahan karena stabilitas. Catatan ini tidak menetapkan pangsa pasar atau peringkat performa. Pada pemeriksaan 12 September 2026, situs proyek menampilkan 2.4.68 sebagai rilis cabang stabil 2.4; status itu perlu diperiksa ulang saat instalasi.

## Sumber

- [Apache HTTP Server Project](https://httpd.apache.org/)
- [Multi-Processing Modules](https://httpd.apache.org/docs/2.4/mpm.html)
- [Name-based Virtual Host Support](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)
- [.htaccess files](https://httpd.apache.org/docs/2.4/howto/htaccess.html)
- [mod_ssl](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html)
- [mod_proxy_fcgi](https://httpd.apache.org/docs/2.4/mod/mod_proxy_fcgi.html)
- [Security Tips](https://httpd.apache.org/docs/2.4/misc/security_tips.html)
- [apachectl](https://httpd.apache.org/docs/2.4/programs/apachectl.html)
- [event MPM](https://httpd.apache.org/docs/2.4/mod/event.html)
- [mod_proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)
- [Ubuntu: How to install Apache2](https://ubuntu.com/server/docs/how-to/web-services/install-apache2/)
