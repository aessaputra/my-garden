---
{"dg-publish":true,"dg-path":"TCP IP.md","permalink":"/tcp-ip/","title":"TCP/IP","hideInFiletree":true,"tags":["references","networking","network"],"noteIcon":"","dg-note-properties":{"title":"TCP/IP","aliases":["TCP IP","Transmission Control Protocol/Internet Protocol"],"categories":["Networking"],"type":"reference","tags":["references","networking","network"],"sources":["_raw/articles/fortinet-what-is-tcp-ip-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

TCP/IP adalah nama suite protokol Internet sekaligus nama model empat lapis yang mendeskripsikannya. Ia bukan satu protokol: TCP dan IP adalah dua protokol inti di dalamnya, ditemani UDP, ICMP, ARP, dan protokol aplikasi seperti HTTP serta DNS. [[References/How Does Internet Work\|How Does Internet Work]] memakai TCP/IP sebagai kerangka saat menjelaskan perjalanan packet dari browser ke server.

Pembagian tugasnya sederhana: IP memberi alamat dan mengantar packet antarjaringan, sedangkan TCP menjaga urutan dan keutuhan byte di atas jalur yang tidak menjamin apa pun.

## Peran TCP dan IP

**IP** (RFC 791, 1981) adalah protokol connectionless di internet layer. Tugasnya mengalamatkan dan merutekan packet antarjaringan. IP bersifat best effort: packet bisa hilang, duplikat, atau datang tidak berurutan, dan IP tidak memperbaikinya. Alamat IP mengidentifikasi interface pada satu scope, bukan perangkat secara unik.

**TCP** (RFC 9293, STD 7, Agustus 2022; menggantikan RFC 793) adalah protokol transport yang menyediakan byte stream andal, berurutan, dan full-duplex antara dua socket. TCP memecah stream menjadi segmen, memberi nomor sequence, meminta ACK, mengirim ulang segmen yang hilang, serta mengatur laju lewat flow control dan congestion control. TCP tidak mempertahankan batas record; aplikasi harus menentukan sendiri framing pesannya.

Kombinasi keduanya menjelaskan namanya: IP menentukan tujuan, TCP memastikan data sampai ke aplikasi tujuan dalam urutan yang benar selama koneksi hidup.

## Empat lapisan

Model TCP/IP yang dipakai RFC 1122 punya empat lapisan. Banyak buku menampilkan lima dengan physical dipisah; itu pilihan penyajian, bukan protokol berbeda.

| Lapisan TCP/IP | Padanan OSI | Isi |
|---|---|---|
| Link | Physical + Data Link | Frame, MAC, driver, Ethernet, Wi-Fi |
| Internet | Network | IP, ICMP, routing |
| Transport | Transport | TCP, UDP, QUIC |
| Application | Session + Presentation + Application | HTTP, DNS, SMTP, SSH |

Data turun melewati lapisan saat dikirim (encapsulation) dan naik kembali saat diterima (decapsulation). Setiap lapisan menambahkan header sendiri. Istilah "packet" paling tepat untuk unit di internet layer; di link disebut frame, di transport disebut segment (TCP) atau datagram (UDP).

## Cara kerja koneksi TCP

1. **Handshake.** Client mengirim SYN, server membalas SYN-ACK, client mengirim ACK. Ketiganya menyepakati initial sequence number dan opsi seperti MSS serta window scaling.
2. **Transfer.** Data dipecah menjadi segmen, diberi sequence number, dan di-ACK. Segmen hilang dikirim ulang; ACK duplikat memicu fast retransmit.
3. **Flow dan congestion control.** Receive window mencegah pengirim membanjiri penerima; congestion window menyesuaikan laju terhadap jaringan (slow start, congestion avoidance).
4. **Penutupan.** FIN/ACK di kedua arah, lalu TIME-WAIT. Reset (RST) memutus koneksi secara paksa.

Koneksi dikenali oleh 4-tuple: alamat IP sumber, port sumber, alamat IP tujuan, dan port tujuan. Port memisahkan aplikasi pada satu alamat; tanpa port, satu host hanya bisa melayani satu percakapan per alamat. [[References/Firewall\|Firewall]] memakai tuple ini untuk mencocokkan aturan.

## TCP versus UDP

| Aspek | TCP | UDP |
|---|---|---|
| Koneksi | Connection-oriented, handshake | Connectionless |
| Keandalan | ACK, retransmisi, urutan | Tidak ada jaminan (RFC 768) |
| Checksum | Ya, 16-bit | Ya; opsional di IPv4, wajib di IPv6 |
| Overhead | Header 20+ byte, state koneksi | Header 8 byte |
| Cocok untuk | HTTP/1.1, HTTP/2, SSH, SMTP, transfer file | DNS, VoIP, game, streaming, QUIC/HTTP/3 |

UDP lebih murah karena tidak memelihara state dan tidak menunggu ACK, bukan karena tidak punya checksum. Aplikasi di atas UDP harus menangani sendiri kehilangan, duplikasi, dan urutan bila memang dibutuhkan; QUIC melakukan itu di userspace.

## Privasi dan keamanan

TCP/IP tidak menyediakan kerahasiaan maupun autentikasi. Isi packet bisa dibaca di jalur yang tidak terenkripsi. Perlindungan datang dari lapisan lain: TLS untuk HTTP ([[References/HTTPS\|HTTPS]]), IPsec untuk jaringan, atau VPN untuk jalur remote. [[References/Firewall\|Firewall]] dan [[References/Proxy Server\|Proxy Server]] memeriksa metadata TCP/IP seperti alamat, port, dan state koneksi; tanpa TLS termination mereka tidak melihat isi payload.

## Alamat IP

- **Private versus public.** RFC 1918 (10/8, 172.16/12, 192.168/16) dipakai ulang di banyak jaringan; NAT menerjemahkan ke alamat publik. Karena itu alamat privat tidak unik global.
- **Satu perangkat, banyak alamat.** Satu host bisa punya loopback, alamat LAN, alamat VPN, dan alamat container sekaligus. Probe di mesin ini menunjukkan `127.0.0.1`, `192.168.110.237`, `100.98.64.116`, dan `172.17.0.1` pada satu host.
- **IPv4 versus IPv6.** IPv4 32-bit; IPv6 128-bit (RFC 8200). IPv6 menghapus broadcast dan fragmentasi oleh router, memakai Neighbor Discovery, serta mengandalkan PMTUD.
- **"TCP/IP address" bukan istilah protokol.** Yang ada adalah alamat IP. TCP memakai port, bukan alamat.

## Koreksi terhadap artikel Fortinet

1. **"TCP menjamin integritas data."** TCP mendeteksi error transmisi dengan checksum 16-bit dan mengirim ulang, tetapi checksum bukan fungsi kriptografis dan bisa dilewati penyerang. Integritas dalam arti keamanan berasal dari TLS/IPsec. TCP juga tidak menjamin pengiriman jika koneksi putus.
2. **"TCP bertanggung jawab merutekan data."** Routing adalah tugas IP. TCP hanya menangani segmentasi, urutan, ACK, dan pengiriman ke port aplikasi; header TCP tidak memuat informasi routing.
3. **"IP memperoleh dan mendefinisikan alamat."** Alamat diberikan oleh konfigurasi (statis, DHCP, SLAAC) dan dibawa di header IP. IP tidak "memperoleh" alamat.
4. **"Setiap perangkat punya alamat IP yang mengidentifikasinya secara unik."** Tidak benar secara global: alamat privat dipakai ulang di balik NAT, dan satu perangkat bisa punya banyak alamat (lihat probe di atas). Alamat mengidentifikasi interface pada satu scope.
5. **"TCP/IP memakai three-way handshake."** Handshake adalah milik TCP; IP connectionless dan tidak punya handshake. Nama suite membuat batas ini kabur.
6. **"Handshake memastikan banyak koneksi socket bisa dipindahkan dua arah bersamaan."** Handshake membangun satu koneksi dan menyepakati sequence number serta opsi. Konkurensi datang dari banyak socket, bukan dari handshake.
7. **"Datalink layer juga disebut physical layer."** Link layer menggabungkan physical dan data link OSI; menyebutnya "physical layer" menghilangkan framing dan pengalamatan MAC.
8. **"UDP tidak menyediakan error connection."** Maksudnya error correction. UDP tetap punya checksum, tetapi tidak punya retransmisi, urutan, atau proteksi duplikat (RFC 768).
9. **"UDP tidak memberi sinyal tujuan sebelum mengirim."** Benar bahwa tidak ada handshake, tetapi setiap datagram membawa port sumber/tujuan dan header IP membawa alamat. Bukan berarti tanpa tujuan.
10. **"TCP/IP address."** Tidak ada alamat TCP/IP; yang ada alamat IP. TCP memakai port.
11. **"IP adalah standar komunikasi cepat dan aman."** IP tidak menyediakan keamanan; TLS, IPsec, atau VPN yang menyediakannya. "Cepat" juga bukan sifat protokol, melainkan hasil implementasi dan jalur.
12. **"TCP menyumbang sebagian besar traffic."** Bergantung titik pengukuran dan waktu. QUIC di atas UDP membawa porsi web yang terus tumbuh, dan DNS umumnya UDP. Klaim ini tidak diverifikasi artikel.
13. **"Dikembangkan oleh Departemen Pertahanan AS."** ARPANET dibiayai DARPA dan RFC 791 adalah standar DoD, tetapi desain TCP/IP dikerjakan Cerf, Kahn, dan komunitas IETF. Ini penyederhanaan sejarah.
14. **"Packet otomatis disusun ulang."** Artikel mencampur fragmentasi IP dengan segmentasi TCP. IP menyusun ulang fragment di tujuan; TCP menyusun ulang byte stream. Keduanya berbeda.
15. **Telnet dan FTP sebagai contoh protokol saat ini.** Keduanya mengirim kredensial dalam cleartext dan sudah digantikan SSH/SFTP. Menyebutnya tanpa catatan usang menyesatkan.

## Verifikasi

Fakta protokol dicek ke dokumen primer: RFC 9293 (STD 7, Agustus 2022) menyatakan TCP sebagai protokol transport dan menggantikan RFC 793; RFC 768 (STD 6, Agustus 1980) menyatakan delivery dan duplicate protection UDP tidak dijamin; RFC 791 (STD 5, September 1981) adalah spesifikasi IP DoD. Pemeriksaan lokal: `man 7 tcp` (manpages 6.7-2) menyebut TCP reliable, stream-oriented, full-duplex, mengirim ulang packet hilang, memakai checksum, dan tidak mempertahankan batas record; `ip -brief addr` menunjukkan satu host dengan beberapa alamat termasuk privat dan loopback. Halaman Fortinet yang diekstrak tidak mencantumkan tanggal terbit. Tidak ada perubahan konfigurasi atau layanan.

## Sumber

- [What is TCP/IP in Networking? oleh Fortinet](https://www.fortinet.com/uk/resources/cyberglossary/tcp-ip): definisi TCP dan IP, perbandingan, model empat lapis, dan FAQ. Disimpan sebagai `_raw/articles/fortinet-what-is-tcp-ip-2026-11-26.md`.
- [RFC 9293: Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293), STD 7, Agustus 2022.
- [RFC 768: User Datagram Protocol](https://www.rfc-editor.org/rfc/rfc768), STD 6, Agustus 1980.
- [RFC 791: Internet Protocol](https://www.rfc-editor.org/rfc/rfc791), STD 5, September 1981.
