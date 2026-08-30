---
{"dg-publish":true,"dg-path":"How Does Internet Work","permalink":"/how-does-internet-work/","title":"How Does Internet Work","hideInFiletree":true,"tags":["network","http","ssl","security","architecture","guide"],"dgShowInlineTitle":true,"dg-note-properties":{"title":"How Does Internet Work","category":"references","tags":["network","http","ssl","security","architecture","guide"],"sources":["_raw/articles/how-does-internet-work-cs-fyi.md","_raw/articles/how-does-internet-work-mdn.md"],"created":"2026-08-21","updated":"2026-08-30"}}
---

Internet adalah jaringan dari banyak jaringan yang memakai protokol bersama agar perangkat dapat bertukar data. Halaman ini menggabungkan penjelasan bertahap MDN tentang infrastruktur Internet dengan pembahasan teknis cs.fyi tentang packet, TCP/IP, port, socket, dan TLS.

## Dari dua komputer ke Internet

- Dua komputer dapat terhubung langsung melalui kabel Ethernet atau koneksi nirkabel seperti Wi-Fi.
- Banyak komputer dalam satu jaringan lokal memakai switch. Switch meneruskan pesan ke perangkat tujuan di jaringan tersebut.
- Router menghubungkan satu jaringan dengan jaringan lain dan menentukan jalur packet menuju tujuan.
- Modem menghubungkan jaringan lokal ke infrastruktur penyedia akses.
- ISP menghubungkan pelanggan ke jaringan miliknya, lalu bertukar lalu lintas dengan ISP dan jaringan lain. Gabungan jaringan ini membentuk Internet.

## Packet, router, dan IP

Data tidak dikirim sebagai satu blok utuh. Data dipecah menjadi packet kecil, dikirim melalui beberapa router, lalu disusun kembali di tujuan.

- Packet: unit kecil data yang membawa payload dan informasi pengiriman.
- Router: perangkat yang membaca informasi tujuan dan meneruskan packet ke jaringan berikutnya.
- IP address: alamat numerik perangkat atau interface pada jaringan, misalnya `192.168.1.1` untuk IPv4.
- Domain name: nama yang mudah dibaca manusia, misalnya `google.com`.
- DNS: sistem yang menerjemahkan domain name menjadi IP address.

IP menangani pengalamatan dan routing. Pengiriman packet bersifat best effort: IP sendiri tidak menjamin packet tiba, tetap berurutan, atau hanya diterima sekali.

## TCP dan UDP

TCP dan UDP bekerja di atas IP untuk kebutuhan komunikasi yang berbeda.

### TCP

TCP menyediakan komunikasi andal dan berurutan antara dua endpoint.

- Port mengidentifikasi aplikasi atau layanan pada suatu perangkat.
- Socket adalah endpoint komunikasi yang biasanya dikenali melalui kombinasi alamat IP, protokol, dan port.
- Connection terbentuk antara endpoint client dan server.
- Sequence number, acknowledgement, retransmission, dan flow control membantu memastikan data tiba lengkap dan berurutan.

HTTP versi 1 dan 2, FTP, serta SMTP umumnya memakai TCP.

### UDP

UDP mengirim datagram tanpa membentuk koneksi andal seperti TCP. Overhead dan latensinya lebih rendah, tetapi aplikasi harus menangani sendiri packet yang hilang atau tidak berurutan bila hal itu penting. UDP digunakan antara lain oleh DNS, komunikasi real-time, dan QUIC yang menjadi dasar HTTP/3.

## HTTP, HTTPS, dan TLS

HTTP mengatur pertukaran request dan response antara client dan server. HTTPS adalah HTTP yang diamankan dengan TLS.

TLS memberi tiga perlindungan utama:

- Autentikasi: sertifikat membantu client memverifikasi identitas server.
- Kerahasiaan: data dienkripsi selama transit.
- Integritas: perubahan data selama perjalanan dapat dideteksi.

Saat TLS handshake, client dan server menyepakati versi protokol, algoritma kriptografi, dan kunci sesi. Certificate Authority menandatangani sertifikat untuk membentuk rantai kepercayaan. SSL adalah pendahulu TLS dan tidak lagi layak digunakan sebagai protokol keamanan modern.

## Internet dan Web berbeda

Internet adalah infrastruktur jaringan global. Web adalah salah satu layanan yang berjalan di atas Internet menggunakan URL, HTTP, browser, dan server web. Email, messaging, SSH, dan berbagai layanan lain juga memakai Internet tanpa menjadi bagian dari Web.

- Intranet: jaringan privat untuk anggota suatu organisasi.
- Extranet: bagian intranet yang dibuka secara terbatas bagi mitra atau client.

Keduanya dapat memakai teknologi dan protokol yang sama dengan Internet, tetapi aksesnya dibatasi.

## Perjalanan singkat saat membuka website

1. Browser meminta DNS menerjemahkan domain menjadi IP address.
2. Perangkat mengirim packet melalui router lokal dan ISP menuju server.
3. Client membangun koneksi transport, biasanya TCP atau QUIC.
4. Untuk HTTPS, client dan server menyelesaikan TLS handshake.
5. Browser mengirim HTTP request.
6. Server mengirim HTTP response dalam sejumlah packet.
7. Browser menyusun response dan merender halaman.

## Perkembangan jaringan

- 5G meningkatkan kapasitas jaringan seluler dan dapat menurunkan latensi pada kondisi yang mendukung.
- IoT menambah jumlah perangkat yang terhubung dan memperbesar kebutuhan keamanan serta pengelolaan identitas perangkat.
- Edge computing memproses data lebih dekat ke pengguna atau sumber data untuk mengurangi latensi dan beban jaringan pusat.
- AI dipakai untuk optimasi jaringan, deteksi anomali, dan layanan berbasis data, tetapi bukan komponen dasar protokol Internet.
- Blockchain merupakan aplikasi jaringan terdistribusi, bukan lapisan wajib dalam arsitektur Internet.

## Lihat juga

- [[References/HTTP\|HTTP]]: request, response, versi protokol, dan HTTP/3
- [[References/HTTPS\|HTTPS]]: TLS, sertifikat, HSTS, dan praktik keamanan
- [[References/DNS\|DNS]]: proses penerjemahan domain menjadi IP address
- [[References/Domain Name\|Domain Name]]: struktur dan pengelolaan nama domain
- [[References/Jaringan (Network)\|Jaringan (Network)]]: networking di React Native melalui Fetch API dan WebSocket

## Sumber

- [How Does the Internet Work? oleh cs.fyi](https://cs.fyi/guide/how-does-internet-work): packet routing, TCP/IP, UDP, port, socket, TLS, dan perkembangan jaringan.
- [How does the Internet work? oleh MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work): penjelasan bertahap tentang switch, router, modem, ISP, Internet, Web, intranet, dan extranet.
