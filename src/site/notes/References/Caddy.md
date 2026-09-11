---
{"dg-publish":true,"dg-path":"Caddy.md","permalink":"/caddy/","title":"Caddy","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Caddy","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/caddy-research-packet-2026-09-12.md"],"created":"2026-09-12","updated":"2026-09-12","confidence":"high"}}
---

Caddy adalah web server open source yang ditulis dalam Go. Distribusi standarnya berupa executable mandiri dengan kemampuan menyajikan file, reverse proxy, dan otomatisasi sertifikat TLS. Dalam kelompok [[References/Web Servers\|Web Servers]], pembeda utamanya adalah pengelolaan HTTPS yang terintegrasi, bukan sekadar konfigurasi pendek.

## Automatic HTTPS dan batasnya

Ketika Caddy mengetahui nama host yang dilayaninya dan konfigurasi tidak menonaktifkan otomatisasi, Caddy dapat memperoleh serta memperbarui sertifikat dan mengalihkan HTTP ke HTTPS. Let's Encrypt bukan satu-satunya issuer: dokumentasi menjelaskan Let's Encrypt dan ZeroSSL sebagai CA default dengan mekanisme fallback.

Otomatisasi tetap memerlukan infrastruktur yang benar. Untuk deployment publik biasa, DNS harus mengarah ke server, port yang diperlukan harus dapat dijangkau, Caddy harus dapat melakukan bind, dan storage sertifikat harus writable serta persisten.

Validasi ACME bergantung pada challenge yang dipakai. HTTP challenge memerlukan akses port 80; TLS-ALPN memerlukan port 443. DNS challenge menggunakan record TXT sehingga tidak memerlukan port masuk, tetapi membutuhkan konfigurasi provider DNS, kredensial, dan modul yang sesuai. Wildcard melalui Let's Encrypt memerlukan DNS challenge.

Untuk hostname lokal atau internal, Caddy dapat memakai CA lokalnya sendiri. Ini bukan sertifikat publik Let's Encrypt. Perangkat dan browser client harus mempercayai root CA tersebut; instalasi trust otomatis tidak dijamin berhasil, terutama pada container atau service tanpa hak administratif.

[[References/HTTPS\|HTTPS]] melindungi koneksi, bukan memperbaiki authorization atau kerentanan aplikasi. Automatic HTTPS juga tidak menjamin sertifikat langsung tersedia ketika proses berhasil dimulai: pengurusan sertifikat dapat berlangsung di background dan gagal karena DNS, jaringan, storage, atau batas CA.

On-Demand TLS berbeda dari otomatisasi biasa: penerbitan sertifikat dapat dimulai pada handshake pertama untuk nama yang belum diketahui. Fitur ini perlu diaktifkan dan dibatasi, misalnya lewat endpoint persetujuan domain, agar pihak luar tidak bebas memicu penerbitan sertifikat.

## Static file serving dan konfigurasi

`caddy file-server` dapat menyajikan direktori kerja tanpa file konfigurasi. Namun, default command tersebut mendengarkan HTTP pada port 80; bukan otomatis membuat situs publik HTTPS. Flag `--domain` memberi nama host dan mengaktifkan upaya HTTPS untuk nama itu.

Karena direktori kerja menjadi root default, jalankan dari direktori yang memang boleh dipublikasikan atau tentukan `--root`. Directory listing memerlukan `--browse`; jangan mengaktifkannya untuk direktori yang berisi materi privat. Kemudahan command tidak menggantikan pemeriksaan file yang dapat diakses.

Untuk konfigurasi lebih lengkap, Caddyfile menyediakan sintaks ringkas. Format native Caddy adalah JSON; adapter menerjemahkan format lain ke konfigurasi tersebut. Admin API mendukung perubahan saat server berjalan, bukan mengharuskan edit file untuk setiap perubahan.

## Reverse proxy, load balancing, dan protokol

Reverse proxy adalah kemampuan modul standar, bukan fitur yang selalu memerlukan plugin pihak ketiga. Directive `reverse_proxy` dapat menunjuk satu atau beberapa upstream, dengan pengaturan transport, load balancing, header, dan health checks.

Kebijakan load balancing default adalah `random`, bukan round-robin. Retries tidak aktif secara default. Active health checks memerlukan konfigurasi; daftar beberapa upstream saja tidak membuktikan failover aplikasi sudah benar. Dynamic upstreams juga memiliki batas: active health checks tidak dijalankan untuk upstream dinamis.

Koneksi ke upstream tanpa pengaturan TLS memakai HTTP plaintext. HTTPS pada koneksi client ke Caddy tidak otomatis mengenkripsi koneksi Caddy ke backend. Bila backend memakai TLS, pertahankan verifikasi sertifikat dan konfigurasi trust yang sesuai; jangan menjadikan penonaktifan verifikasi sebagai penyelesaian permanen.

Dukungan protokol server mencakup HTTP/1.1, HTTP/2, dan HTTP/3. Dokumentasi mencantumkan ketiganya sebagai default. HTTP/3 memerlukan jalur UDP yang sesuai, umumnya port 443; dukungan protokol tidak menjamin setiap client akan menegosiasikannya.

## Modul dan operasi

Caddy memisahkan command, core pengelola konfigurasi, dan modul. Banyak modul sudah masuk build standar. Ekstensi tambahan digabungkan ke binary saat build, bukan diasumsikan sebagai file plugin yang selalu bisa dimuat dinamis. Periksa modul yang tersedia pada binary aktual sebelum menyalin konfigurasi.

Dalam [[References/Deployment\|Deployment]], gunakan service manager dan pertahankan storage sertifikat serta state saat mengganti container atau executable. Melupakan volume data dapat membuang state penting dan memicu pengurusan sertifikat ulang. Simpan private key dengan izin akses yang terbatas.

`caddy validate --config Caddyfile --adapter caddyfile` memeriksa konfigurasi melalui tahap loading dan provisioning tanpa menjalankan layanan. Setelah validasi, perubahan dapat diterapkan melalui reload; tidak perlu menghentikan service hanya untuk mengganti konfigurasi. Tetap uji routing, sertifikat, dan backend setelah perubahan.

Admin API secara default memakai `localhost:2019`. Endpoint ini mengendalikan konfigurasi dan proses sehingga tidak boleh dibuka untuk pihak yang tidak dipercaya. Pada host yang menjalankan kode tidak tepercaya, isolasi proses atau socket dengan permissions membantu membatasi akses.

## Kapan memilih Caddy

Caddy layak dipertimbangkan ketika otomatisasi TLS dan konfigurasi yang relatif ringkas mengurangi pekerjaan operasional. Ini bukan batas bahwa Caddy hanya cocok untuk proyek kecil sampai menengah, ataupun bukti bahwa ia selalu lebih sederhana daripada [[References/Apache\|Apache]] dan [[References/Nginx\|Nginx]] pada seluruh deployment.

Evaluasi kebutuhan modul, traffic, health checks, observabilitas, pengelolaan sertifikat, dan kemampuan tim. Dokumentasi yang digunakan adalah dokumentasi Caddy 2 yang terus diperbarui, diperiksa 12 September 2026; tidak ada klaim versi patch terbaru, benchmark, atau uji kapasitas dalam catatan ini.

## Sumber

- [Caddy Documentation](https://caddyserver.com/docs/)
- [Automatic HTTPS](https://caddyserver.com/docs/automatic-https)
- [Static files quick-start](https://caddyserver.com/docs/quick-starts/static-files)
- [reverse_proxy](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy)
- [Architecture](https://caddyserver.com/docs/architecture)
- [Keep Caddy Running](https://caddyserver.com/docs/running)
- [Caddyfile global options](https://caddyserver.com/docs/caddyfile/options)
- [Command Line](https://caddyserver.com/docs/command-line)
- [Standard HTTP modules — source code](https://raw.githubusercontent.com/caddyserver/caddy/master/modules/caddyhttp/standard/imports.go)
- [Admin API](https://caddyserver.com/docs/api)
