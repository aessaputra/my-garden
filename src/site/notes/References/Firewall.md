---
{"dg-publish":true,"dg-path":"Firewall.md","permalink":"/firewall/","title":"Firewall","hideInFiletree":true,"tags":["references","security","networking"],"noteIcon":"","dg-note-properties":{"title":"Firewall","aliases":["Network Firewall","Packet Filter","NGFW"],"categories":["Security"],"type":"reference","tags":["references","security","networking"],"sources":["_raw/articles/checkpoint-what-is-firewall-2026-11-26.md","_raw/articles/cisco-what-is-firewall-2026-11-26.md","_raw/articles/tutorialspoint-what-is-firewall-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

Firewall adalah kontrol akses jaringan yang memeriksa lalu lintas dan memutuskan apakah paket atau koneksi boleh lewat, berdasarkan aturan yang ditetapkan. Ia bukan antivirus dan bukan pengganti autentikasi: tugasnya menegakkan kebijakan jaringan, bukan memindai isi file secara menyeluruh. Firewall dapat berupa perangkat keras, perangkat lunak, appliance virtual, layanan cloud, atau komponen di dalam platform (security group, network policy).

Posisinya tidak selalu "antara internet dan kantor". Firewall juga dipakai untuk segmentasi internal, membatasi lalu lintas antar-VLAN, mengisolasi database dari server aplikasi, dan pada host (host-based firewall seperti `ufw`, `nftables`, atau Windows Defender Firewall). [[References/Server Security\|Server Security]] menempatkan firewall sebagai satu lapisan hardening, bukan satu-satunya kontrol.

## Cara kerja

Aturan firewall umumnya dicocokkan pada **5-tuple**: alamat IP sumber dan tujuan, port sumber dan tujuan, serta protokol. Firewall modern menambah dimensi lain: pengguna, aplikasi, URL, kategori konten, dan reputasi.

- **Rule order.** Banyak firewall memakai first-match: aturan pertama yang cocok menentukan aksi, dan aturan sesudahnya tidak dievaluasi. Urutan karena itu bagian dari kebijakan, bukan detail kosmetik.
- **Default policy.** Di akhir daftar biasanya ada implicit deny. Kebijakan default-deny lebih aman, tetapi menuntut aturan eksplisit untuk setiap layanan yang sah.
- **Stateful inspection.** Firewall menyimpan tabel koneksi. Paket balasan untuk koneksi yang sudah diizinkan diteruskan tanpa aturan arah balik terpisah. Firewall stateless harus mengizinkan kedua arah secara eksplisit.
- **Layer inspection.** Packet filter bekerja di L3/L4. Proxy dan NGFW naik ke L7 sehingga bisa membedakan HTTP dari SSH di port yang sama, atau aplikasi tertentu di dalam TLS.
- **NAT dan VPN.** Banyak firewall juga melakukan NAT dan terminasi VPN. Keduanya fungsi jaringan; NAT menyembunyikan alamat, tetapi bukan kontrol akses.

TLS inspection memungkinkan firewall membaca isi HTTPS, tetapi memutus TLS, membutuhkan CA yang dipercaya client, menambah beban, dan menimbulkan persoalan privasi serta certificate pinning. Tanpa itu, firewall hanya melihat SNI, alamat IP, dan metadata koneksi.

## Jenis

| Jenis | Lapisan | Catatan |
|---|---|---|
| Packet filtering | L3/L4 | Cepat dan sederhana; tidak melihat isi paket maupun state koneksi. |
| Stateful inspection | L3/L4 + state | Melacak sesi; keputusan lebih tepat daripada packet filter murni. |
| Proxy firewall | L7 | Memutus koneksi dan menjadi perantara; bisa caching dan inspeksi konten, dengan biaya latency. Lihat [[References/Proxy Server\|Proxy Server]]. |
| NGFW | L3-L7 | Stateful plus App-ID, IPS, antivirus, URL filtering, TLS inspection. |
| WAF | HTTP | Melindungi aplikasi web dari injeksi, XSS, dan pola serangan HTTP. Bukan pengganti secure coding. |
| UTM | Gabungan | Beberapa fungsi dalam satu perangkat, biasanya untuk kemudahan pengelolaan. |
| Virtual / cloud-native | Virtual | Appliance virtual atau kontrol yang mengikuti lifecycle cloud (security group, network policy). |
| FWaaS / managed | Layanan | Dikelola penyedia; cocok untuk tim kecil, dengan ketergantungan pada vendor. |

Label "AI-powered firewall" yang dipakai vendor bukan kategori teknis yang ketat; biasanya berarti model ML dipakai untuk klasifikasi traffic atau deteksi anomali di dalam NGFW.

## Evolusi generasi

Check Point membagi sejarah firewall menjadi lima generasi: virus pada PC standalone (akhir 1980-an), firewall jaringan (pertengahan 1990-an), firewall sadar aplikasi, firewall dengan inspeksi payload dan IPS, lalu firewall berskala besar dengan cloud dan AI. Kerangka ini berguna sebagai ringkasan arah industri, tetapi merupakan taksonomi vendor dan mencampur sejarah antivirus dengan firewall. Yang bertahan sebagai fakta: dari pencocokan port menuju inspeksi aplikasi dan identitas.

## Praktik konfigurasi

- **Least privilege.** Izinkan hanya layanan yang dibutuhkan, dengan sumber, tujuan, dan port eksplisit. Ganti aturan `Any/Any` dengan allow/deny yang spesifik.
- **Dokumentasi.** Catat tujuan aturan, layanan, pengguna, tanggal, masa berlaku, dan pemilik. Aturan temporer yang lupa dihapus adalah sumber lubang yang umum.
- **Lindungi bidang manajemen.** Nonaktifkan Telnet dan SNMPv1, batasi akses admin, backup konfigurasi, dan terapkan aturan yang menolak scan ke firewall itu sendiri.
- **Segmentasi.** Kelompokkan aturan dan jaringan sesuai tingkat kepercayaan; jangan campur server publik, database, dan workstation dalam satu zona.
- **Patch dan audit.** Firmware firewall juga punya kerentanan. Tinjau log, aturan yang tidak terpakai, dan konflik antar-aturan secara berkala.
- **High availability.** Untuk layanan penting, pasang cluster atau pair agar kegagalan satu perangkat tidak memutus jaringan.

## Batas dan kesalahpahaman

Firewall tidak menghentikan malware yang masuk lewat jalur yang diizinkan. Unduhan HTTPS, lampiran email, phishing, credential theft, dan insider melewati aturan port yang sah. NGFW dengan IPS dan antivirus menambah kemampuan, tetapi tidak menjamin deteksi.

Firewall juga tidak menggantikan patch management, autentikasi, otorisasi per request, dan logging. Dalam arsitektur Zero Trust, firewall adalah salah satu titik penegakan kebijakan, bukan satu-satunya. Untuk konteks aplikasi web, lihat [[References/Web Security Knowledge\|Web Security Knowledge]] dan [[References/HTTPS\|HTTPS]]; untuk perlindungan edge, [[References/Cloudflare\|Cloudflare]] menyediakan WAF dan DDoS mitigation sebagai layanan.

Kesalahan yang sering terjadi: aturan terlalu lebar, urutan aturan menutupi aturan lain, aturan sementara yang menetap, egress tidak dibatasi, IPv6 tidak dikonfigurasi sehingga lolos dari aturan IPv4, dan asumsi bahwa NAT sudah cukup sebagai keamanan.

## Koreksi klaim sumber

1. **Check Point: firewall sebagai "barrier antara private network dan public Internet".** Terlalu sempit. Firewall juga dipakai untuk segmentasi internal, east-west traffic, dan host.
2. **Check Point: "AI-powered firewall".** Istilah pemasaran; tidak ada definisi industri yang mengikat.
3. **Check Point: generasi pertama "virus".** Kerangka lima generasi adalah taksonomi vendor dan mencampur sejarah antivirus dengan firewall.
4. **Check Point: inspeksi network layer "performs better" daripada application layer.** Terlalu umum. Throughput bergantung pada implementasi dan hardware; NGFW modern melakukan keduanya.
5. **Check Point: NAT sebagai "defense against reconnaissance".** Penyembunyian alamat adalah efek samping, bukan kontrol keamanan. NAT tidak mengautentikasi dan tidak memblokir akses.
6. **Cisco: firewall sebagai "first line of defense".** Positioning vendor. Banyak serangan dimulai dari identitas, endpoint, atau email, bukan dari port yang terbuka.
7. **Cisco: "AI-powered firewall".** Sama seperti di atas, label pemasaran.
8. **Tutorialspoint: kualitas dan tanggal.** Artikel diperbarui 2022, bergaya spun, dan memuat saran konsumen seperti mengubah NAT PlayStation yang tidak relevan untuk firewall enterprise.
9. **Tutorialspoint: "firewalls are powerless to stop viruses".** Terlalu absolut. NGFW dengan IPS/antivirus bisa memblokir sebagian malware, meski tidak ada jaminan dan tetap butuh endpoint protection.
10. **Tutorialspoint: "Zero Trust ... every access layer of the OSI Model".** Tidak koheren. Zero Trust adalah model kebijakan berbasis identitas dan konteks, bukan properti tiap lapisan OSI.
11. **Tutorialspoint: "hardware or software device".** Melewatkan FWaaS dan firewall cloud-native yang berupa layanan, bukan perangkat.
12. **Ketiganya konten vendor/tertiary.** Tidak ada yang merupakan standar. Rujukan normatif untuk arsitektur firewall adalah NIST SP 800-41, yang belum di-ingest ke vault ini.

## Verifikasi

Di mesin ini `ufw`, `nft`, dan `iptables` terpasang; `ufw` aktif dan `firewalld` tidak aktif. Tidak ada perubahan aturan atau uji lalu lintas yang dilakukan, jadi perilaku filtering tidak diuji. Halaman Cisco bertanggal 2024-08-09 dan Tutorialspoint diperbarui 2022-03-15; keduanya dibaca 26 November 2026. Ketiga sumber adalah vendor atau tertiary, sehingga halaman ini diberi `confidence: medium`.

## Sumber

- [What is a Firewall? The Different Types of Firewalls](https://www.checkpoint.com/cyber-hub/network-security/what-is-firewall/), Check Point Cyber Hub, diakses 26 November 2026.
- [What Is a Firewall?](https://www.cisco.com/c/en_in/products/security/firewalls/what-is-a-firewall.html), Cisco, dipublikasikan 9 Agustus 2024, diakses 26 November 2026.
- [What Is a Firewall and Why Do You Need One?](https://www.tutorialspoint.com/what-is-a-firewall-and-why-do-you-need-one), Tutorialspoint, diperbarui 15 Maret 2022, diakses 26 November 2026.
