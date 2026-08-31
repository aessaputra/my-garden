---
{"dg-publish":true,"dg-path":"Cloudflare.md","permalink":"/cloudflare/","title":"Cloudflare","hideInFiletree":true,"tags":["references","security","performance","network","ssl","devops","deployment","edge-functions"],"dg-note-properties":{"title":"Cloudflare","category":"references","tags":["references","security","performance","network","ssl","devops","deployment","edge-functions"],"sources":["_raw/articles/cloudflare-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Cloudflare adalah platform jaringan dan cloud yang dapat bertindak sebagai penyedia [[References/DNS\|DNS]], reverse proxy, [[Content Delivery Network\|CDN]], serta lapisan keamanan di depan website atau aplikasi. Dalam konfigurasi proxied, pengunjung terhubung ke jaringan Cloudflare lebih dahulu. Cloudflare kemudian menjawab dari cache, menerapkan aturan keamanan, menjalankan kode edge, atau meneruskan request ke origin server.

Deskripsi Cloudflare sebagai layanan yang "berada di antara website dan pengunjung" tepat untuk hostname berstatus proxied. Namun, Cloudflare tidak selalu berada di jalur traffic. Record berstatus DNS only hanya memakai layanan DNS Cloudflare dan tetap mengarahkan klien ke origin. Produk seperti Workers dan Pages juga dapat menjadi tempat aplikasi berjalan, bukan sekadar perantara menuju server lain.

## Alur request

Pada full DNS setup, nameserver domain diarahkan ke Cloudflare sehingga Cloudflare menjadi authoritative DNS provider. Untuk record proxied, jawaban DNS berisi alamat anycast Cloudflare, bukan alamat origin. Request [[References/HTTP\|HTTP]] atau [[References/HTTPS\|HTTPS]] masuk ke lokasi jaringan Cloudflare, diproses oleh cache dan kontrol keamanan, lalu diteruskan ke origin jika Cloudflare tidak dapat menjawabnya sendiri.

Model ini memberi beberapa manfaat:

- alamat IP origin tidak perlu terlihat melalui record DNS yang diproksikan;
- konten yang sudah ada di cache dapat dilayani tanpa menghubungi origin;
- serangan dan request yang melanggar aturan dapat dihentikan sebelum mencapai origin;
- terminasi TLS, redirect, header, load balancing, serta kode Workers dapat dijalankan di jaringan Cloudflare.

Perlindungan tersebut berlaku pada traffic yang benar-benar melewati Cloudflare. Penyerang masih dapat melewati proxy jika menemukan IP origin atau mengakses port dan hostname yang tidak diproksikan. Origin perlu membatasi koneksi masuk, memperbarui software, dan mempertahankan kontrol keamanannya sendiri.

## Cache dan CDN

Cache Cloudflare menyimpan salinan konten di pusat data yang lebih dekat dengan pengguna. Cache hit mengurangi latensi dan beban origin; cache miss membuat Cloudflare mengambil respons dari origin sebelum menyimpannya jika respons memenuhi aturan cache.

Cloudflare tidak menyimpan semua halaman secara otomatis. Perilaku default dipengaruhi metode request, ekstensi berkas, status respons, `Cache-Control`, `Expires`, dan `Set-Cookie`. Secara default, HTML dan JSON tidak di-cache hanya berdasarkan ekstensi. Operator dapat mengubah perilaku dengan Cache Rules, Edge Cache TTL, cache key, Tiered Cache, dan purge.

Caching yang agresif dapat menyajikan konten usang atau membocorkan respons yang seharusnya khusus pengguna. Respons privat, halaman autentikasi, dan data personal harus memakai header serta aturan cache yang tepat. Purge memaksa Cloudflare mengambil versi baru, tetapi tidak memperbaiki sumber konten yang salah di origin.

## Keamanan jaringan dan aplikasi

Cloudflare menyediakan mitigasi DDoS otomatis untuk traffic jaringan dan aplikasi yang melewati produknya. Sistem ini berusaha menyerap atau menolak traffic serangan sambil mempertahankan traffic sah. Cakupan dan kemampuan konfigurasi berbeda menurut produk dan paket.

Cloudflare Web Application Firewall memeriksa request web dan API dengan ruleset. Managed Rules membantu mendeteksi pola serangan umum, sedangkan custom rules dapat memfilter berdasarkan alamat IP, URL, header, atau properti request lain. Rate limiting, bot management, dan analitik keamanan memperluas kontrol tersebut, tetapi tidak semua fitur tersedia sama pada setiap paket.

WAF bukan pengganti perbaikan aplikasi. Aturan dapat menghasilkan false positive, tidak mengetahui seluruh logika bisnis, dan tidak menghapus kerentanan pada origin. Aplikasi tetap memerlukan validasi input, autentikasi, otorisasi, patch, logging, serta pengujian keamanan.

## SSL/TLS

Karena Cloudflare menjadi reverse proxy, satu request dapat melibatkan dua koneksi TLS: pengunjung ke Cloudflare dan Cloudflare ke origin. Edge certificate melindungi koneksi pertama. Origin certificate atau sertifikat dari CA publik melindungi koneksi kedua.

Mode enkripsi menentukan perlakuan pada kedua koneksi:

- Flexible mengenkripsi koneksi pengunjung ke Cloudflare tetapi memakai HTTP ke origin;
- Full memakai HTTPS ke origin tanpa memvalidasi sertifikat origin;
- Full (strict) memakai HTTPS dan memvalidasi nama host, masa berlaku, serta rantai sertifikat origin.

Full (strict) adalah pilihan yang tepat ketika origin mendukung TLS dengan sertifikat valid. Flexible tidak menyediakan enkripsi end-to-end dan sering memicu redirect loop jika origin memaksa HTTPS tanpa memahami terminasi proxy. Universal SSL menerbitkan edge certificate untuk hostname yang memenuhi syarat, tetapi operator tetap perlu mengamankan koneksi origin dan memilih mode enkripsi yang benar.

## DNS dan onboarding

Onboarding domain pada full setup biasanya mencakup penambahan domain, pemeriksaan seluruh record DNS, pemilihan status proxied atau DNS only, perubahan nameserver di registrar, lalu pemeriksaan konfigurasi SSL/TLS. Pemindaian record otomatis tidak dijamin menemukan semua record, sehingga mail, verifikasi domain, subdomain, dan layanan non-web harus ditinjau manual.

Orange cloud berarti traffic web untuk record tersebut melewati Cloudflare. Gray cloud berarti Cloudflare hanya menjawab DNS. Proxy HTTP/HTTPS standar juga hanya mendukung port tertentu; layanan pada port lain perlu dibuat DNS only, dipindahkan ke port yang didukung, atau memakai produk seperti Spectrum.

## Platform pengembang

Cloudflare kini lebih luas daripada CDN. Cloudflare Workers menyediakan runtime serverless untuk API dan aplikasi full-stack. Pages menerbitkan frontend dari Git provider atau direct upload serta dapat menambahkan fungsi server. R2 menyediakan object storage, sedangkan D1, KV, Durable Objects, Queues, dan produk lain memberi penyimpanan atau koordinasi bagi aplikasi Workers.

Cloudflare One mencakup layanan Zero Trust dan jaringan perusahaan seperti Access, Tunnel, Secure Web Gateway, dan Data Loss Prevention. Cloudflare Tunnel membuat koneksi keluar dari infrastruktur menuju jaringan Cloudflare sehingga layanan internal tidak harus membuka inbound port atau mengekspos IP publik.

Produk-produk ini memiliki model eksekusi, batas, harga, dan jaminan berbeda. Memakai DNS atau CDN Cloudflare tidak otomatis berarti aplikasi juga berjalan di Workers, memakai WAF lengkap, atau memperoleh semua fitur Zero Trust.

## Keandalan dan trade-off

Reverse proxy mengurangi beban origin dan dapat menahan serangan, tetapi juga menambah lapisan konfigurasi. Kesalahan nameserver, record DNS, cache rule, redirect, WAF, atau mode TLS dapat membuat situs gagal walaupun origin sehat. Error `525` dan `526`, redirect loop, mixed content, cache usang, serta pemblokiran request sah termasuk masalah yang perlu dibedakan dari kegagalan origin.

Untuk diagnosis, operator dapat memeriksa DNS, status proxy, respons cache, event WAF, sertifikat origin, dan log server. Development Mode melewati cache sementara tanpa mematikan perlindungan lain. Menonaktifkan proxy pada satu record atau mem-pause Cloudflare mengirim traffic langsung ke origin, tetapi sekaligus melepaskan cache, WAF, dan sertifikat edge pada jalur tersebut.

Cloudflare tepat ketika situs membutuhkan DNS terkelola, distribusi konten, mitigasi serangan, kontrol traffic, atau komputasi edge tanpa mengelola seluruh infrastruktur sendiri. Ia tidak menghapus kebutuhan akan origin yang sehat, arsitektur aplikasi yang aman, observabilitas, backup, dan rencana pemulihan.

## Lihat juga

- [[References/DNS\|DNS]]
- [[References/HTTPS\|HTTPS]]
- [[References/HTTP\|HTTP]]
- [[References/Web Hosting\|Web Hosting]]
- [[References/Content Security Policy\|Content Security Policy]]
- [[References/GitHub Pages\|GitHub Pages]]
- [[References/Astro\|Astro]]

## Sumber

- [How Cloudflare DNS works](https://developers.cloudflare.com/fundamentals/concepts/how-cloudflare-works): authoritative DNS, anycast, status proxied, reverse proxy, serta alur request ke origin.
- [Onboard a domain](https://developers.cloudflare.com/fundamentals/setup/manage-domains/add-site/): full DNS setup, pemeriksaan record, nameserver, status proxy, DNSSEC, dan SSL/TLS.
- [DNS concepts](https://developers.cloudflare.com/dns/concepts/): domain, registrar, nameserver, authoritative DNS, dan setup DNS.
- [Cloudflare Cache](https://developers.cloudflare.com/cache/): cache terdistribusi, Cache Rules, Tiered Cache, purge, dan integrasi produk.
- [Default cache behavior](https://developers.cloudflare.com/cache/concepts/default-cache-behavior/): header cache, metode request, ekstensi berkas, cache lock, serta perilaku HTML dan JSON.
- [Cloudflare DDoS Protection](https://developers.cloudflare.com/ddos-protection/): mitigasi otomatis, managed ruleset, layer jaringan dan aplikasi, serta perbedaan ketersediaan fitur.
- [Cloudflare Web Application Firewall](https://developers.cloudflare.com/waf/): ruleset, custom rules, managed rules, rate limiting, analitik, dan ketersediaan per paket.
- [Cloudflare SSL/TLS](https://developers.cloudflare.com/ssl/): edge certificate, origin certificate, Universal SSL, dan pengelolaan TLS.
- [Encryption modes](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/): Flexible, Full, Full (strict), validasi sertifikat, dan dua koneksi TLS.
- [Pause Cloudflare](https://developers.cloudflare.com/fundamentals/setup/manage-domains/pause-cloudflare/): pause, DNS only, Development Mode, dan dampaknya pada cache serta keamanan.
- [Network ports](https://developers.cloudflare.com/fundamentals/reference/network-ports/): port HTTP/HTTPS yang diproksikan dan opsi untuk traffic pada port lain.
- [Cloudflare Workers](https://developers.cloudflare.com/workers/): runtime serverless, framework, binding, storage, observabilitas, dan produk terkait.
- [Cloudflare Pages](https://developers.cloudflare.com/pages/): deployment frontend, direct upload, integrasi Git, Pages Functions, dan rollback.
- [Cloudflare R2](https://developers.cloudflare.com/r2/): object storage, skenario penggunaan, public bucket, dan integrasi Workers.
- [Cloudflare One](https://developers.cloudflare.com/cloudflare-one/): SASE, Zero Trust, Access, Tunnel, Secure Web Gateway, dan DLP.
