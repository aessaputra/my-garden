---
{"dg-publish":true,"dg-path":"Proxy Server.md","permalink":"/proxy-server/","title":"Proxy Server","hideInFiletree":true,"tags":["references","networking","security","http"],"noteIcon":"","dg-note-properties":{"title":"Proxy Server","aliases":["Proxy","Forward Proxy","Reverse Proxy"],"categories":["Web Technologies"],"type":"reference","tags":["references","networking","security","http"],"sources":["_raw/articles/fortinet-proxy-server-2026-11-26.md","_raw/articles/rfc9110-intermediaries-2026-11-26.md","_raw/articles/cloudflare-reverse-proxy-2026-11-26.md","_raw/articles/nginx-reverse-proxy-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

Proxy adalah perantara yang menerima pesan HTTP dan meneruskannya. Menurut RFC 9110 bagian 3.7, proxy adalah *message-forwarding agent* yang **dipilih oleh client**, biasanya lewat aturan konfigurasi lokal, untuk menerima request bagi URI absolut tertentu dan memenuhinya lewat antarmuka HTTP. Jadi proxy adalah peran pada jalur pesan, bukan jenis perangkat: router, server, atau proses di cloud bisa menjalankannya, tetapi "router" bukan definisi proxy.

RFC 9110 membedakan tiga bentuk intermediary:

| Bentuk | Dipilih oleh | Perilaku |
|---|---|---|
| Proxy | Client | Meneruskan request atas nama client; sering dipakai untuk layanan keamanan, anotasi, atau cache bersama. |
| Gateway (reverse proxy) | Origin | Bertindak sebagai origin server bagi client, lalu meneruskan request ke server lain; dipakai untuk load balancing, caching accelerator, dan membungkus layanan lama. |
| Tunnel | Client | Relai buta yang tidak mengubah pesan; contohnya `CONNECT` untuk membawa TLS melewati proxy. |

## Cara kerja

Client diberi tahu alamat proxy lewat konfigurasi manual (host dan port), file PAC (Proxy Auto-Config), atau variabel environment seperti `http_proxy`. Untuk request `http`, client mengirim request ke proxy dan proxy yang menghubungi origin. Untuk `https`, client membuka tunnel `CONNECT host:443`; proxy melihat nama host dan port, tetapi isi TLS tetap terenkripsi ujung ke ujung.

Konsekuensinya, proxy biasa **tidak** membaca isi HTTPS. Untuk memeriksa isi, proxy harus melakukan intersepsi TLS: memutus TLS, membuat sertifikat baru, dan client harus mempercayai CA proxy tersebut. Intersepsi seperti ini terdeteksi lewat certificate pinning, pemeriksaan CA, atau certificate transparency, dan memutus jaminan end-to-end.

Karena berada di jalur, proxy bisa melakukan autentikasi (`407 Proxy Authentication Required`), pencatatan, penyaringan berdasarkan URL atau kategori, dan cache bersama. Cache bersama mengikuti semantik HTTP: response `private` tidak boleh disimpan untuk banyak pengguna.

## Reverse proxy

Reverse proxy duduk di depan satu atau beberapa server dan menjadi "wajah publik" situs: alamat yang diumumkan adalah alamat proxy, sedangkan client tidak pernah menghubungi origin secara langsung. Ini kebalikan dari forward proxy, yang duduk di depan client. Implementasi umumnya [[References/Nginx\|Nginx]], [[References/Caddy\|Caddy]], [[References/Cloudflare\|Cloudflare]], Envoy, dan HAProxy.

Reverse proxy dan load balancer sering tertukar. Load balancer membagi request ke banyak server dan baru berguna bila server lebih dari satu; algoritma dan health check-nya dibahas di [[References/Load Balancing\|Load Balancing]]. Reverse proxy berguna bahkan dengan satu origin karena menambah lapisan kontrol: terminasi TLS, routing berdasarkan path atau host, kompresi, cache, pembatasan laju, dan penyembunyian topologi internal. Produk yang sama sering menjalankan keduanya, jadi batasnya konsep, bukan merek.

Yang perlu dipahami soal koneksi:

- **Full proxy** memakai dua koneksi terpisah, client-proxy dan proxy-origin, sehingga masing-masing sisi punya TCP stack sendiri. Ini yang membuat proxy bisa memeriksa dan mengubah pesan, berbeda dari tunnel yang hanya meneruskan byte.
- **Connection pooling** membuat proxy memakai ulang koneksi ke origin. Pada HTTP/1.1 koneksi dipakai bergantian; pada HTTP/2 dan HTTP/3 banyak request dari client berbeda bisa berbagi satu koneksi sebagai stream terpisah. Pemisahan tetap ada di level stream, bukan selalu di level koneksi.
- **TLS termination** di proxy berarti segmen client-proxy berakhir di proxy. Segmen proxy-origin harus dilindungi sendiri, misalnya TLS ulang ke origin. [[References/Cloudflare\|Cloudflare]] memakai dua sertifikat untuk dua segmen ini; menganggap "sudah HTTPS di edge" sebagai cukup adalah kesalahan konfigurasi yang umum.

Menyembunyikan IP origin hanya efektif bila tidak ada jalur bypass. Record DNS lama, subdomain yang tidak diproxykan, record MX, log certificate transparency, dan akses langsung ke IP origin bisa membocorkan alamat asli. Origin sebaiknya hanya menerima koneksi dari IP proxy, bukan dari internet umum.

## Fungsi operasional

- **Kontrol egress.** Membatasi tujuan keluar, mencatat akses, dan menerapkan kebijakan per pengguna. Ini pengawasan lalu lintas keluar, bukan pencegahan penyusup masuk.
- **Cache dan penghematan bandwidth.** Response yang cacheable disimpan dan dipakai ulang.
- **Load balancing dan terminasi TLS.** Umum pada reverse proxy di depan beberapa origin.
- **Alamat keluar dan geolokasi.** Request tampak berasal dari IP proxy, bukan IP client.
- **Anonimitas terbatas.** Tujuan tidak melihat IP client, tetapi operator proxy melihat seluruh request, dan cookie, login, serta fingerprint browser tetap melacak.

## Jenis

- **Forward proxy** berada di depan client dan menjadi satu titik keluar bagi jaringan internal.
- **Reverse proxy** berada di depan server; RFC 9110 menyebutnya gateway. Lihat [[References/Nginx\|Nginx]], [[References/Caddy\|Caddy]], dan [[References/Cloudflare\|Cloudflare]].
- **Transparent/interception proxy** tidak dipilih client; ia membelokkan lalu lintas TCP, biasanya di firewall atau access point. RFC 9110 menegaskan intermediary seperti ini tidak bisa dibedakan dari penyerang on-path pada level protokol dan sering merusak semantik HTTP.
- **Anonymous dan high-anonymity** berusaha menyembunyikan identitas client; tingkat anonimitas bergantung pada operator dan pencatatannya, bukan pada label.
- **Distorting** mengirim identitas palsu (misalnya `X-Forwarded-For` yang salah) sambil mengaku sebagai proxy.
- **Data center** memakai IP dari pusat data komersial; cepat dan murah, anonimitas rendah.
- **Residential** memakai IP perangkat rumah; terlihat seperti pengguna biasa, biaya lebih tinggi, dan etikanya bergantung pada persetujuan pemilik perangkat.
- **Public** gratis dan terbuka; lambat dan berisiko karena operatornya tidak diketahui.
- **Shared** dipakai banyak pengguna pada satu IP; murah, tetapi reputasi IP menanggung kesalahan pengguna lain.
- **SSL/TLS-intercepting** memutus TLS untuk memeriksa isi; butuh CA tepercaya di client.
- **Rotating** mengganti IP keluar secara berkala atau per request, bukan "satu IP unik per pengguna" seperti klaim artikel.

## Batas keamanan

Proxy bukan firewall. Firewall memeriksa paket dan menerapkan aturan jaringan; proxy adalah perantara level aplikasi yang bisa memfilter, mencatat, dan mengautentikasi, tetapi tidak otomatis menjadi batas masuk. Sebuah forward proxy bahkan tidak menghadap internet untuk melindungi jaringan dari penyusup; ia mengatur lalu lintas keluar.

Karena operator proxy melihat request, memakai proxy berarti memindahkan kepercayaan ke pihak lain. Klaim "tidak bisa dilacak" atau "anonim penuh" tidak benar selama operator mencatat, dan pelacakan berbasis cookie, login, serta fingerprint tidak hilang hanya karena IP berubah.

Risiko operasional yang perlu dijaga: proxy terbuka bisa dipakai pihak lain (open proxy), proxy internal bisa disalahgunakan untuk SSRF ke jaringan privat, log proxy berisi data pribadi, dan reverse proxy yang salah mempercayai header `X-Forwarded-*` bisa dilewati. Untuk konteks perlindungan web, lihat [[References/Web Security Knowledge\|Web Security Knowledge]] dan [[References/HTTPS\|HTTPS]].

## Koreksi terhadap sumber vendor

Artikel glossary Fortinet benar soal gambaran umum dan daftar jenis, tetapi beberapa klaimnya terlalu longgar atau salah:

1. **"System or router" sebagai definisi.** RFC 9110 mendefinisikan proxy sebagai agen penerus pesan yang dipilih client. Perangkat keras hanyalah salah satu tempat menjalankannya.
2. **"Membantu mencegah penyerang masuk ke jaringan privat."** Forward proxy mengatur request keluar; pencegahan masuk adalah tugas firewall atau reverse proxy. Artikel sendiri kemudian membedakan proxy dari firewall, sehingga klaim awal bertentangan dengan FAQ-nya.
3. **"Proxy menjalankan fungsi firewall."** Tidak. Proxy bekerja di level aplikasi; firewall memeriksa paket. RFC 9110 bahkan menyebut network intermediary tidak terbedakan dari penyerang on-path.
4. **"Browsing tidak mungkin dilacak."** Berlebihan. Tujuan tidak melihat IP client, tetapi operator proxy melihat semua request; cookie, login, dan fingerprint tetap ada.
5. **"SSL proxy menyembunyikan keberadaannya dari client dan server."** Intersepsi TLS justru butuh CA proxy yang dipercaya client, bisa dideteksi lewat pinning atau pemeriksaan sertifikat, dan melanggar jaminan end-to-end.
6. **"SSL proxy membantu peringkat Google."** Salah konteks: HTTPS adalah sinyal sisi server; proxy intersepsi di sisi client tidak mengubah penilaian mesin pencari atas situs.
7. **"Transparent proxy memberi pengalaman mulus."** RFC 9110 menyoroti risikonya: tidak dipilih client, menyerupai penyerang on-path, dan rawan merusak semantik HTTP serta interoperabilitas.
8. **"Rotating proxy memberi IP berbeda untuk setiap pengguna."** Rotasi terjadi per request atau per sesi, bukan per pengguna.
9. **"Residential proxy lebih tepercaya."** Ini opini vendor; kepercayaan bergantung pada persetujuan pemilik perangkat, kebijakan operator, dan tujuan penggunaan.
10. **"Data center proxy untuk mengumpulkan intelijen tentang orang atau organisasi."** Use case scraping/OSINT disajikan tanpa catatan hukum dan ketentuan layanan tujuan.

Dua artikel vendor berikut menambahkan detail yang berguna, dengan catatan:

11. **Cloudflare: "SSL encryption ... decrypt incoming, encrypt outgoing".** Yang dimaksud adalah terminasi TLS di proxy, bukan enkripsi end-to-end. Segmen proxy-origin tetap perlu dilindungi sendiri, dan istilah "SSL" sudah digantikan TLS.
12. **Cloudflare: "IP origin tidak pernah terekspos".** Hanya benar bila tidak ada kebocoran lewat DNS lama, subdomain tanpa proxy, MX, atau certificate transparency. Origin juga harus menolak koneksi selain dari proxy.
13. **Cloudflare: cache "di reverse proxy Paris untuk origin Los Angeles".** Itu cache CDN terdistribusi. Satu reverse proxy di dekat origin tidak mempercepat pengguna di benua lain.
14. **Cloudflare: "cara termudah adalah berlangganan CDN".** Promosi. Nginx, Caddy, dan Envoy berjalan sebagai reverse proxy tanpa investasi hardware khusus.
15. **Nginx: "full proxy ... tidak pernah ada pencampuran koneksi".** Benar untuk stream TCP, tetapi upstream HTTP/2 atau HTTP/3 memultipleks stream dari banyak client pada satu koneksi. Pemisahan ada di level stream.
16. **Nginx: load balancer "hanya masuk akal dengan banyak server".** Benar secara konsep, tetapi Nginx, HAProxy, dan Envoy menjalankan routing dan load balancing sekaligus; batasnya bukan batas produk.
17. **Bias vendor.** Cloudflare menutup artikel dengan ajakan memakai CDN-nya, dan halaman "Nginx" adalah konten F5 dengan bagian khusus produk BIG-IP. Bagian implementasi kedua artikel adalah product placement, bukan perbandingan netral.

## Verifikasi

RFC 9110 dibaca langsung dari teks resmi (bagian 3.7, excerpt disimpan di `_raw/`). Di mesin ini `curl 8.5.0` mendukung `-x/--proxy` dan tidak ada variabel `*_proxy` yang aktif, tetapi tidak ada proxy server yang dijalankan, sehingga perilaku tunnel, cache, dan intersepsi tidak diuji lokal. Halaman Fortinet, Cloudflare, dan Nginx adalah sumber sekunder; ekor promosi ketiganya dibuang saat pengarsipan.

## Sumber

- [What is a Proxy Server?](https://www.fortinet.com/resources/cyberglossary/proxy-server), Fortinet Cyber Glossary, diakses 26 November 2026.
- [RFC 9110 §3.7 Intermediaries](https://www.rfc-editor.org/rfc/rfc9110.html#section-3.7), kutipan baris 863-953 dari teks RFC, diakses 26 November 2026.
- [What is a reverse proxy?](https://www.cloudflare.com/en-gb/learning/cdn/glossary/reverse-proxy/), Cloudflare Learning Center, diakses 26 November 2026.
- [Reverse proxy](https://www.nginx.com/resources/glossary/reverse-proxy-server/), NGINX (F5) Glossary, diakses 26 November 2026.
