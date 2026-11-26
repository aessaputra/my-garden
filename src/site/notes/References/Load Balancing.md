---
{"dg-publish":true,"dg-path":"Load Balancing.md","permalink":"/load-balancing/","title":"Load Balancing","hideInFiletree":true,"tags":["references","networking","http","performance"],"noteIcon":"","dg-note-properties":{"title":"Load Balancing","aliases":["Load Balancer","Penyeimbang Beban"],"categories":["Web Technologies"],"type":"reference","tags":["references","networking","http","performance"],"sources":["_raw/articles/nginx-load-balancing-glossary-2026-11-26.md","_raw/articles/cloudflare-what-is-load-balancing-2026-11-26.md","_raw/articles/dzone-nginx-reverse-proxy-load-balancing-2026-11-26.md","_raw/articles/nginx-upstream-module-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

Load balancing adalah pembagian request atau pekerjaan ke beberapa server agar tidak ada satu mesin yang menanggung seluruh beban. Komponen yang memilih tujuan disebut load balancer. Load balancer bukan syarat agar aplikasi berjalan: satu server tetap bisa melayani pengguna. Manfaatnya baru muncul ketika ada lebih dari satu backend yang bisa menerima pekerjaan, dan ketika kegagalan satu backend tidak boleh menghentikan layanan.

Load balancer sering satu produk dengan reverse proxy. [[References/Nginx\|Nginx]], HAProxy, Envoy, dan [[References/Cloudflare\|Cloudflare]] menjalankan keduanya. Bedanya ada di konsep: reverse proxy menjadi wajah publik dan berguna bahkan dengan satu origin, sedangkan load balancer membagi beban dan baru bermakna dengan beberapa backend. Pembahasan perantara HTTP, terminasi TLS, dan penyembunyian IP origin ada di [[References/Proxy Server\|Proxy Server]].

## Lapisan dan bentuk

| Bentuk | Yang dipakai untuk memilih | Kelebihan | Batas |
|---|---|---|---|
| DNS / GSLB | Record A/AAAA banyak, atau DNS yang memeriksa kesehatan | Murah, global, tidak perlu perangkat | Client dan resolver menyimpan cache sesuai TTL; failover tidak instan; tidak melihat beban per request |
| L4 | Alamat IP, port, protokol, 5-tuple | Cepat, tidak peduli protokol, bisa meneruskan TCP/UDP apa pun | Tidak bisa routing berdasarkan path, header, atau cookie; TLS harus passthrough atau diterminasi di tempat lain |
| L7 | HTTP: host, path, header, cookie, method | Routing konten, retry, buffering, terminasi TLS, header rewrite | Lebih mahal, butuh pemahaman protokol, dan tidak berlaku untuk protokol non-HTTP |
| Anycast | Rute BGP ke PoP terdekat | Latensi rendah dan tahan kegagalan PoP | Bukan health check per backend; satu IP tidak menjamin semua PoP siap |

L4 bisa bekerja dalam beberapa mode. Mode NAT menulis ulang alamat tujuan ke backend; mode direct server return (DSR) membiarkan response keluar langsung dari server sehingga IP client tetap terlihat; mode full proxy memutus koneksi dan membuka koneksi baru ke backend. Pada mode proxy, IP client asli bisa diteruskan dengan PROXY protocol, tetapi backend harus mengerti header itu dan hanya mempercayainya dari load balancer.

L7 memerlukan akses ke isi HTTP. Jika traffic masih TLS passthrough, load balancer hanya bisa bekerja di L4. Untuk routing berdasarkan path atau header, TLS harus diterminasi di load balancer, lalu segmen ke backend dilindungi sendiri jika diperlukan. Ini pola yang sama dengan reverse proxy di [[References/Proxy Server\|Proxy Server]].

## Algoritma

Algoritma statis tidak membaca kondisi runtime; algoritma dinamis membaca koneksi, latensi, atau kesehatan server. Tidak ada algoritma yang terbaik untuk semua beban: pilih berdasarkan bentuk traffic dan biaya kegagalan.

| Metode | Perilaku | Risiko |
|---|---|---|
| Round robin | Giliran tetap, dengan bobot opsional | Server lambat tetap kebagian traffic; cocok untuk request homogen |
| Least connections | Ke server dengan koneksi aktif paling sedikit, memperhitungkan bobot | Koneksi panjang (WebSocket, gRPC, streaming) membuat hitungan tidak mewakili beban sebenarnya |
| Least time | Kombinasi latensi respons dan koneksi aktif | Butuh pengukuran yang stabil; bisa mengarahkan traffic ke server yang kebetulan cepat sesaat |
| Random two choices | Pilih dua server acak, lalu pilih yang lebih ringan | Hampir sebaik least connections dengan biaya koordinasi lebih kecil |
| IP hash / source hash | Hash IP client; IPv4 memakai tiga oktet pertama, IPv6 seluruh alamat | Client di belakang NAT berbagi server; perubahan jumlah server mengacak pemetaan |
| URL hash | Hash URL atau key lain | Bagus untuk cache; tanpa `consistent`, penambahan server memindahkan banyak key |
| Consistent hashing | Ring dengan banyak titik per server | Hanya sebagian kecil key yang dipindah saat server berubah; lebih rumit dioperasikan |

Bobot (`weight`) membuat server lebih kuat menerima porsi lebih besar. Bobot bukan pengganti health check: server berbobot besar yang mati tetap harus dikeluarkan dari pool.

## Health check dan failover

Health check pasif menilai kegagalan dari traffic nyata. Di Nginx, `max_fails` (default 1) dan `fail_timeout` (default 10 detik) menentukan berapa kali gagal dalam jendela waktu tertentu sebelum server dianggap tidak tersedia; apa yang dihitung sebagai gagal ditentukan `proxy_next_upstream`. Health check aktif mengirim probe berkala ke endpoint kesehatan. Di Nginx open source, probe aktif bukan fitur bawaan; modul health check aktif adalah bagian dari langganan komersial. HAProxy dan load balancer cloud umumnya menyertakan probe aktif.

Failover berarti memindahkan traffic ke server lain saat satu server gagal. Mekanismenya bisa berupa server `backup`, pengeluaran server dari pool, atau `drain` untuk menghentikan traffic baru sambil menyelesaikan request yang berjalan. Dua kesalahan umum: probe terlalu agresif sehingga server sehat dikeluarkan saat lambat sesaat, dan probe terlalu dangkal (hanya cek port TCP) sehingga server yang prosesnya hidup tetapi tidak siap tetap menerima request.

Failover tidak menggandakan data. Load balancer hanya memindahkan request; session, upload, dan transaksi tetap harus disimpan di tempat yang bisa dibaca semua backend, misalnya database atau store bersama. Lihat [[References/Caching\|Caching]] untuk pola cache bersama dan [[References/HTTPS\|HTTPS]] untuk perlindungan transport.

## Sesi, retry, dan timeout

Session persistence atau sticky session mengikat client ke server yang sama, biasanya lewat cookie yang disisipkan load balancer atau lewat hash IP. Ini menyelesaikan aplikasi lama yang menyimpan state di memori proses, tetapi punya biaya: beban bisa tidak rata, session hilang saat server mati, dan client di balik NAT berbagi server. Untuk aplikasi baru, menyimpan session di store bersama lebih tahan gagal dan membuat backend benar-benar stateless.

Retry hanya aman untuk request idempoten atau request yang membawa idempotency key. Mengulang `POST` yang sudah sempat diproses bisa membuat transaksi ganda. Timeout connect, read, dan send harus lebih pendek dari timeout client, dan retry tidak boleh berubah menjadi badai yang memperberat backend yang sedang pulih. `proxy_next_upstream` di Nginx menentukan kondisi apa yang memicu percobaan ke server berikutnya; menambahkan status 5xx ke daftar itu berarti `POST` juga bisa diulang, jadi gunakan dengan sadar.

## Contoh Nginx

Konfigurasi minimal dengan dua backend, least connections, dan koneksi keepalive ke upstream:

```nginx
upstream app {
    least_conn;
    server 10.0.0.11:8080 max_fails=3 fail_timeout=10s;
    server 10.0.0.12:8080 max_fails=3 fail_timeout=10s;
    keepalive 32;
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate     /etc/nginx/tls/fullchain.pem;
    ssl_certificate_key /etc/nginx/tls/privkey.pem;

    location / {
        proxy_pass http://app;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

`least_conn` cocok untuk request pendek yang tidak seragam. Untuk cache atau aplikasi yang butuh key stabil, `hash $request_uri consistent` lebih tepat daripada `ip_hash`. `max_fails` dan `fail_timeout` adalah health check pasif; Nginx open source tidak menyediakan probe aktif. `keepalive` di upstream mengurangi biaya membuka koneksi baru, tetapi jumlahnya harus lebih kecil dari kapasitas backend.

## Operasional dan kegagalan umum

- **Load balancer sebagai single point of failure.** Satu instance adalah titik mati baru. Pasang pair dengan failover, atau pakai anycast/cloud LB yang memang dirancang redundan.
- **Health check yang salah.** Probe harus menguji kesiapan aplikasi (database, dependency), bukan hanya port. Sebaliknya, probe yang terlalu ketat bisa mengeluarkan semua server sekaligus.
- **Koneksi panjang.** WebSocket, gRPC streaming, dan HTTP/2 membuat least connections bias. Pertimbangkan batas waktu, drain, dan algoritma yang menghitung request, bukan hanya koneksi.
- **Bottleneck di hilir.** Load balancer membagi request, bukan kapasitas database. Jika query atau lock sudah menjadi bottleneck, menambah backend hanya menambah antrean.
- **Header yang dipercaya buta.** `X-Forwarded-For` bisa dipalsukan client. Backend hanya boleh mempercayai header itu dari load balancer, dan load balancer harus menimpa, bukan meneruskan, nilai dari luar.
- **Sertifikat dan konfigurasi.** Terminasi TLS memindahkan tanggung jawab sertifikat ke load balancer; rotasi yang terlambat memutus semua layanan. Perubahan aturan yang tidak diuji bisa mengarahkan traffic ke pool yang salah.
- **Observability.** Pantau 5xx per upstream, latency p95, koneksi aktif, status health check, dan jumlah request yang di-retry. Tanpa itu, kegagalan terlihat hanya sebagai keluhan pengguna.

## Koreksi terhadap sumber

### Nginx glossary (konten F5)

1. **"Memastikan semua server dipakai sama rata."** Tidak benar untuk bobot, least connections, atau server yang sedang drain. Tujuannya membagi beban sesuai kapasitas dan kesehatan, bukan menyamakan angka.
2. **Definisi static load balancing.** Artikel menyebut algoritma statis "memiliki informasi kapasitas kinerja server", padahal ciri utamanya adalah tidak membaca kondisi runtime. Definisi Cloudflare lebih tepat.
3. **Round robin "menggunakan DNS".** Round robin bisa diterapkan di DNS, L4, atau L7. Round robin bawaan Nginx adalah per request, bukan DNS.
4. **"Firewall di L1-L3, load balancing di L4-L7."** Terlalu rapi. NGFW memeriksa sampai L7, dan load balancer bekerja di L4 atau L7; L5 dan L6 bukan lapisan tempat keputusan routing dibuat.
5. **Daftar protokol L4 "(IP, TCP, FTP, UDP)".** FTP adalah protokol aplikasi (L7), dan IP adalah L3. Yang relevan untuk L4 adalah TCP dan UDP.
6. **"Load balancer lapisan IP mengubah alamat tujuan."** Itu mode NAT. Mode DSR tidak menulis ulang alamat, dan full proxy memutus koneksi lalu membuka koneksi baru.
7. **"L7 memakai SSL session ID."** Jika TLS diterminasi di load balancer, session TLS terlihat, tetapi keputusan L7 tetap berdasarkan HTTP. Jika TLS passthrough, load balancer tidak bisa membaca header HTTP.
8. **"Load balancer cloud memberi predictive analytics."** Klaim produk, bukan sifat load balancing. Tidak semua load balancer menyediakan analitik prediktif.
9. **Consistent hashing "dirutekan secara dinamis searah jarum jam".** Pemetaan ring bersifat deterministik, bukan dinamis. Yang dinamis adalah penempatan key saat server berubah, dan itupun hanya sebagian kecil key.
10. **"Tanpa session persistence, informasi harus disinkronkan dan itu tidak efisien."** Sticky session adalah satu pilihan, bukan satu-satunya. Store session bersama sering lebih baik karena tidak mengikat client ke server.
11. **"Jika satu server mati, load balancer mengarahkan traffic ke server lain."** Hanya terjadi jika ada health check atau deteksi kegagalan. Tanpa itu, request tetap dikirim ke server mati.

### Cloudflare

12. **"Load balancing penting untuk hampir semua aplikasi Internet."** Berlebihan. Aplikasi kecil dengan satu server bisa berjalan tanpa load balancer; kebutuhan muncul saat ada target ketersediaan atau beban yang menuntut lebih dari satu backend.
13. **"Algoritma dinamis mengirim lebih sedikit traffic ke server lambat."** Hanya jika algoritma memakai latensi atau kesehatan. Round robin dengan health check mengeluarkan server, bukan mengurangi porsinya secara bertahap.
14. **"Weighted round-robin DNS" sebagai metode umum.** DNS bisa mengembalikan beberapa alamat, tetapi bobot per request dan failover cepat sulit dicapai lewat DNS yang di-cache. Pembobotan yang halus biasanya dilakukan di L4/L7.
15. **Analogi delapan kasir "sekitar delapan kali lebih cepat".** Artikel memberi catatan "tergantung", tetapi analogi mengabaikan bottleneck bersama seperti database dan lock. Menambah kasir tidak mempercepat satu database yang sudah penuh.
16. **GSLB tanpa catatan TTL.** DNS global load balancing tetap tunduk pada TTL dan cache resolver; failover tidak instan dan tidak boleh diandalkan untuk pemulihan detik.

### DZone (2021)

17. **"Client tidak punya informasi backend, jadi tidak ada cara client jahat mengaksesnya langsung."** Kalimat ini mengandung negasi ganda dan kesimpulan yang salah. Menyembunyikan IP bukan kontrol akses: IP origin bisa bocor lewat DNS lama, subdomain tanpa proxy, MX, certificate transparency, atau akses langsung. Origin tetap harus firewall dan hanya menerima koneksi dari load balancer. Lihat [[References/Proxy Server\|Proxy Server]].
18. **"SSL/TLS mahal karena handshake dan enkripsi untuk setiap request."** Handshake terjadi per koneksi, bukan per request; keepalive, HTTP/2, HTTP/3, session resumption, dan TLS 1.3 mengurangi biayanya. Beban CPU TLS tetap nyata pada skala besar, tetapi bukan "setiap request".
19. **"Nginx open source mendukung empat metode."** Snapshot 2021. Dokumentasi Nginx sekarang mencantumkan round robin, `least_conn`, `ip_hash`, `hash`, `random` (sejak 1.15.1), dan `least_time` yang tersedia di open source sejak 1.31.0.
20. **Konfigurasi TLS `TLSv1 TLSv1.1 TLSv1.2`.** TLS 1.0 dan 1.1 sudah dinyatakan usang oleh RFC 8996 (2021). Gunakan TLS 1.2 dan 1.3; `ssl_ciphers HIGH:!aNULL:!MD5` juga tidak mengatur cipher suite TLS 1.3.
21. **"Nginx open source tidak mendukung health probe."** Benar untuk probe aktif, tetapi menyesatkan jika dibaca sebagai "tidak ada health check". `max_fails` dan `fail_timeout` memberi deteksi pasif, dan `slow_start`/`drain` membantu pemulihan.
22. **`ip_hash` "selalu ke server yang sama kecuali tidak tersedia".** Benar untuk client yang sama, tetapi penambahan atau penghapusan server mengacak pemetaan, dan client di balik NAT berbagi server. Untuk cache, `hash ... consistent` lebih tepat.
23. **Contoh `location /api/creditcard{`.** Kurang spasi sebelum `{`; kosmetik, tetapi contoh konfigurasi sebaiknya disalin dengan hati-hati.

### Dokumentasi Nginx

24. **`keepalive` berubah default.** Sejak 1.29.7, cache koneksi ke upstream aktif secara default dengan batas 32 per worker, dan parameter `local` mencegah berbagi koneksi antar-location. Konfigurasi lama yang tidak menyebut `keepalive` berperilaku berbeda di versi baru.
25. **`backup` dan `slow_start` tidak bisa dipakai dengan `hash`, `ip_hash`, atau `random`.** Ini batasan yang mudah terlewat saat menambah server cadangan.
26. **Fitur yang tampak "standar" ternyata komersial.** `queue`, probe aktif `health_check`, dan `least_time` sebelum 1.31.0 adalah bagian langganan komersial. `resolve` baru tersedia di open source sejak 1.27.3.

## Verifikasi

Nginx tidak terpasang di mesin ini (`nginx: command not found`), dan tidak ada paket HAProxy atau Traefik, sehingga konfigurasi contoh tidak diuji lokal. Fakta direktif, versi, dan default diambil langsung dari dokumentasi `ngx_http_upstream_module` yang diakses 26 November 2026. Artikel DZone bertanggal 17 Maret 2021, jadi klaim TLS dan daftar metodenya adalah snapshot usang. Tidak ada perubahan konfigurasi atau layanan di mesin ini.

## Sumber

- [What Is a Load Balancer?](https://www.nginx.com/resources/glossary/load-balancing/), NGINX (F5) Glossary, diakses 26 November 2026; ekor promosi F5 dibuang.
- [What is load balancing?](https://www.cloudflare.com/en-gb/learning/performance/what-is-load-balancing/), Cloudflare Learning Center, diakses 26 November 2026; navigasi Learning Center dan CTA dibuang.
- [Nginx: Reverse Proxy and Load Balancing](https://dzone.com/articles/nginx-reverse-proxy-and-load-balancing), DZone, 17 Maret 2021; body diambil lewat ekstraktor dan blok kode dipulihkan dari HTML.
- [Module ngx_http_upstream_module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html), dokumentasi resmi Nginx, diakses 26 November 2026.
