---
{"dg-publish":true,"dg-path":"HTTPS.md","permalink":"/https/","title":"HTTPS","hideInFiletree":true,"tags":["references","security","programming","http","networking"],"dg-note-properties":{"title":"HTTPS","category":"references","tags":["references","security","programming","http","networking"],"sources":["_raw/articles/https-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

![2026-08-29-how-https-works.jpeg](/img/user/Attachments/2026-08-29-how-https-works.jpeg)
HTTPS (*Hypertext Transfer Protocol Secure*) adalah penggunaan HTTP melalui saluran yang diamankan oleh TLS (*Transport Layer Security*). TLS melindungi komunikasi selama transit melalui tiga sifat utama: kerahasiaan, integritas, dan autentikasi. Perlindungan ini berlaku pada header dan isi pesan HTTP setelah *handshake* selesai.

HTTPS diperlukan bukan hanya untuk layanan perbankan dan surel. Situs biasa juga memerlukan perlindungan karena pihak yang menguasai jaringan dapat membaca lalu lintas HTTP, mengubah halaman, menyisipkan skrip, atau mengarahkan pengguna ke konten palsu. Browser juga memperlakukan halaman HTTPS sebagai *secure context*, dan banyak Web API hanya tersedia dalam konteks tersebut.

## Jaminan yang diberikan HTTPS

Kerahasiaan berarti data aplikasi yang dikirim setelah saluran terbentuk hanya dapat dibaca oleh kedua endpoint. Integritas membuat perubahan terhadap data selama transit dapat dideteksi, sedangkan autentikasi memungkinkan klien memastikan identitas server dan, pada penggunaan tertentu, server juga dapat mengautentikasi klien.

HTTPS tidak membuat situs otomatis aman dari seluruh serangan. TLS tidak memperbaiki kerentanan aplikasi seperti injeksi SQL, *cross-site scripting*, kontrol akses yang lemah, malware pada endpoint, atau kebocoran data setelah informasi mencapai server. TLS 1.3 juga tidak menyembunyikan panjang data yang dikirim, sehingga metadata dan pola lalu lintas tertentu masih dapat diamati.

Ikon koneksi aman menunjukkan bahwa browser membangun saluran terenkripsi ke pemegang sertifikat yang valid untuk domain tersebut. Ikon itu tidak menjamin bahwa isi situs dapat dipercaya, bahwa perusahaan di balik situs bereputasi baik, atau bahwa pengguna sedang berinteraksi dengan layanan yang memang mereka maksud jika domainnya menyerupai domain lain.

## Cara kerja TLS

Sebelum HTTP dipertukarkan, klien dan server menjalankan TLS *handshake*. Tahap ini menegosiasikan versi protokol dan algoritma kriptografi, membentuk material kunci bersama, serta mengautentikasi server. Autentikasi klien bersifat opsional.

Pada TLS 1.3, klien mengirim `ClientHello` yang memuat versi yang ditawarkan, pasangan algoritma simetris dan hash, serta material pertukaran kunci atau identitas PSK bila tersedia. Server memilih parameter melalui `ServerHello`; pesan sesudah tahap pertukaran kunci dilindungi dengan kunci yang telah diturunkan. Pesan sertifikat, `CertificateVerify`, dan `Finished` mengikat identitas endpoint pada pertukaran kunci serta membuktikan integritas *handshake*.

Setelah *handshake* selesai, *record protocol* membagi lalu lintas aplikasi menjadi rekaman dan melindungi setiap rekaman dengan kunci lalu lintas. Enkripsi simetris digunakan untuk data aplikasi karena efisien, sementara kriptografi kunci publik berperan dalam autentikasi dan pembentukan rahasia bersama.

TLS 1.3 menghapus algoritma lama, mewajibkan algoritma AEAD, mengenkripsi seluruh pesan *handshake* setelah `ServerHello`, dan menyediakan *forward secrecy* untuk mekanisme pertukaran kunci publiknya. Mode 0-RTT dapat mengurangi latensi pada koneksi lanjutan, tetapi perlindungannya lebih lemah terhadap pemutaran ulang dan tidak memberikan jaminan *forward secrecy* yang sama, sehingga penggunaannya perlu dibatasi pada operasi yang aman untuk diulang.

## Sertifikat dan rantai kepercayaan

Sertifikat digital mengikat nama domain pada kunci publik server melalui tanda tangan digital. Browser memeriksa kecocokan nama host, masa berlaku, tanda tangan, penggunaan sertifikat, dan jalur sertifikasi menuju akar yang dipercaya. Validasi nama host bersifat menentukan karena sertifikat yang sah tetapi tidak cocok dengan domain tujuan tidak membuktikan identitas server yang dimaksud.

Sertifikat situs biasanya ditandatangani oleh CA perantara, lalu CA perantara terhubung ke CA akar yang berada dalam *trust store* browser atau sistem operasi. Server perlu mengirim sertifikat endpoint beserta sertifikat perantara yang diperlukan agar klien dapat membangun jalur tanda tangan menuju akar terpercaya. Kegagalan mengirim rantai yang tepat dapat menyebabkan sertifikat bekerja pada sebagian klien tetapi gagal pada klien lain.

Certificate Authority seperti Let's Encrypt mengotomatiskan penerbitan melalui ACME. Klien ACME membuktikan kendali atas domain dengan tantangan DNS atau HTTP, membuat CSR yang ditandatangani oleh kunci privat, kemudian meminta CA menerbitkan sertifikat untuk kunci publik tersebut. Perpanjangan mengulangi validasi domain dan permintaan sertifikat, sehingga pengelolaan sertifikat sebaiknya diotomatisasi.

Kunci privat harus tetap berada di bawah kendali operator dan tidak dikirim bersama sertifikat. Jika kunci privat bocor, penyerang dapat menyamar sebagai server selama sertifikat masih diterima, sehingga operator perlu mengganti kunci, menerbitkan sertifikat baru, dan mencabut sertifikat yang terdampak.

## Versi TLS dan konfigurasi

IETF melarang negosiasi SSL 2.0, SSL 3.0, TLS 1.0, dan TLS 1.1. Panduan IETF tahun 2022 mewajibkan dukungan TLS 1.2, menganjurkan dukungan TLS 1.3, dan mengharuskan TLS 1.3 diprioritaskan ketika tersedia. RFC 9846 kemudian memperbarui spesifikasi TLS 1.3 dan melarang negosiasi TLS 1.0 serta TLS 1.1.

Konfigurasi server menentukan versi TLS, algoritma, sertifikat, dan parameter lain yang dapat dinegosiasikan. Operator sebaiknya memakai konfigurasi modern dari perangkat lunak server atau generator konfigurasi yang terpelihara, bukan merakit daftar *cipher suite* lama secara manual. Dukungan protokol usang demi kompatibilitas menambah jalur serangan dan sebaiknya dipertahankan hanya jika kebutuhan klien benar-benar mengharuskannya.

## Pengalihan HTTP dan HSTS

Situs publik dapat tetap mendengarkan port 80 untuk mengalihkan permintaan HTTP ke URL HTTPS pada host dan path yang sama. Namun, permintaan HTTP pertama belum dilindungi TLS sehingga penyerang jaringan dapat mencegah pengalihan atau melakukan *SSL stripping*.

Header `Strict-Transport-Security` atau HSTS memberi tahu browser agar memakai HTTPS pada permintaan berikutnya dan menolak opsi untuk melewati galat sertifikat. Header ini hanya diproses ketika diterima melalui HTTPS; browser mengabaikannya jika dikirim melalui HTTP.

Contoh kebijakan HSTS:

```http
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

`max-age` menentukan berapa lama kebijakan disimpan, sedangkan `includeSubDomains` memperluasnya ke subdomain. Opsi `preload` memungkinkan domain diajukan ke daftar yang ditanamkan pada browser, sehingga permintaan pertama dapat ditingkatkan ke HTTPS sebelum browser pernah menerima header dari situs. Penerapan `includeSubDomains` dan *preload* harus didahului audit seluruh subdomain karena host yang belum mendukung HTTPS dapat menjadi tidak dapat diakses.

## Mixed content

Perlindungan halaman HTTPS dapat rusak jika halaman memuat skrip, stylesheet, gambar, font, atau unduhan melalui HTTP. Kondisi ini disebut *mixed content* karena dokumen utama memakai saluran aman sedangkan sebagian sumber daya melewati saluran yang dapat dibaca atau diubah penyerang.

Browser membagi *mixed content* menjadi konten yang dapat ditingkatkan otomatis dan konten yang harus diblokir. Permintaan aktif seperti skrip, stylesheet, `fetch()`, `XMLHttpRequest`, dan font termasuk konten yang diblokir, sedangkan sebagian gambar, audio, dan video dapat ditingkatkan dari HTTP ke HTTPS oleh browser. Solusi yang tepat adalah menyediakan seluruh dokumen, subresource, dan unduhan melalui HTTPS serta menguji konsol browser untuk menemukan referensi yang masih tidak aman.

Kebijakan berikut dapat membantu migrasi URL lama:

```http
Content-Security-Policy: upgrade-insecure-requests
```

Kebijakan tersebut meminta browser memperlakukan URL sumber daya HTTP sebagai HTTPS, tetapi tidak menggantikan HSTS untuk navigasi menuju situs.

## Batas perlindungan

HTTPS melindungi data ketika bergerak antara endpoint TLS, bukan sebelum data dienkripsi atau setelah data didekripsi. Browser yang telah disusupi, ekstensi berbahaya, server yang diretas, pencatatan aplikasi yang tidak aman, dan pihak yang sah di endpoint masih dapat mengakses data dalam bentuk terbuka.

TLS juga tidak menyembunyikan seluruh metadata koneksi. Spesifikasi TLS menyatakan bahwa panjang data tidak disembunyikan, meskipun rekaman dapat diberi padding untuk mengurangi informasi yang tersedia bagi analisis lalu lintas. Alamat IP tujuan dan karakteristik koneksi juga tetap dapat terlihat oleh infrastruktur jaringan yang meneruskan paket.

Proxy penghenti TLS, CDN, dan *load balancer* membentuk endpoint TLS tersendiri. Jika TLS berakhir pada perantara tersebut, operator perlu memastikan koneksi lanjutan ke server asal juga terlindungi dan bahwa kepercayaan antarbagian sistem dikonfigurasi dengan benar.

## Praktik operasional

Penerapan HTTPS yang sehat mencakup penerbitan dan perpanjangan sertifikat secara otomatis, perlindungan kunci privat, pemantauan masa berlaku, pengiriman rantai sertifikat yang lengkap, serta pengujian konfigurasi setelah setiap perubahan. Semua halaman, API, subresource, dan unduhan harus tersedia melalui HTTPS agar tidak ada bagian aplikasi yang kembali ke saluran tidak aman.

Operator juga perlu memantau kegagalan *handshake*, galat nama host, sertifikat kedaluwarsa, dan klien yang masih meminta protokol lama. HSTS sebaiknya diterapkan bertahap dengan `max-age` rendah saat pengujian, lalu dinaikkan setelah seluruh host dan subdomain dipastikan mendukung HTTPS. Dengan konfigurasi dan operasi yang tepat, HTTPS menyediakan autentikasi server, kerahasiaan, dan integritas selama transit, tetapi keamanan aplikasi tetap memerlukan kontrol pada setiap lapisan lain.

## Lihat juga

- [[References/HTTP\|HTTP]]
- [[References/Content Security Policy\|Content Security Policy]]
- [[References/Web Browser\|Web Browser]]
- [[References/Jaringan (Network)\|Jaringan (Network)]]
- [[References/DNS\|DNS]]

## Sumber

- [Transport Layer Security: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security): konsep HTTPS, jaminan TLS, *handshake*, autentikasi server, dan batas perlindungan selama transit.
- [TLS configuration: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/TLS): konfigurasi TLS, pengalihan HTTP, HSTS, serta pemuatan sumber daya yang aman.
- [Mixed content: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Mixed_content): klasifikasi, risiko, peningkatan otomatis, dan pemblokiran sumber daya HTTP.
- [Strict-Transport-Security: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security): sintaks, masa berlaku, subdomain, dan *preload* HSTS.
- [RFC 9846: TLS 1.3](https://www.rfc-editor.org/rfc/rfc9846.html): spesifikasi TLS 1.3, alur *handshake*, AEAD, 0-RTT, dan *forward secrecy*.
- [RFC 9325: Secure Use of TLS and DTLS](https://www.rfc-editor.org/rfc/rfc9325.html): rekomendasi versi protokol dan konfigurasi TLS yang aman.
- [How Let's Encrypt Works](https://letsencrypt.org/how-it-works): validasi domain, penerbitan, perpanjangan, dan pencabutan sertifikat melalui ACME.
- [Let's Encrypt Chains of Trust](https://letsencrypt.org/certificates): CA akar, CA perantara, dan pembentukan rantai sertifikat menuju akar tepercaya.
