---
{"dg-publish":true,"dg-path":"SSL.md","permalink":"/ssl/","title":"SSL","hideInFiletree":true,"tags":["references","security","networking","ssl"],"noteIcon":"","dg-note-properties":{"title":"SSL","aliases":["Secure Sockets Layer","SSL/TLS"],"categories":["Security"],"type":"reference","tags":["references","security","networking","ssl"],"sources":["_raw/articles/cloudflare-what-is-ssl-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

SSL (*Secure Sockets Layer*) adalah protokol keamanan Internet berbasis enkripsi yang menjadi pendahulu TLS. SSL dikembangkan Netscape: SSL 2.0 dirilis 1995 dan SSL 3.0 pada 1996. TLS 1.0 (RFC 2246, Januari 1999) menggantikannya, dan SSL 3.0 dinyatakan tidak layak dipakai oleh RFC 7568 (Juni 2015). Istilah "SSL" masih dipakai di pasar untuk menyebut TLS, sehingga penawaran "SSL" modern hampir selalu berarti TLS.

SSL/TLS berjalan di atas TCP; lihat [[References/TCP IP\|TCP/IP]]. Pemakaian praktisnya di web, termasuk HSTS dan mixed content, dibahas di [[References/HTTPS\|HTTPS]].

## Sejarah dan versi

| Versi | Tahun | Status |
|---|---|---|
| SSL 1.0 | tidak dirilis | Tidak pernah dipublikasikan |
| SSL 2.0 | 1995 | Dilarang RFC 6176 (2011) |
| SSL 3.0 | 1996 | Dinyatakan deprecated oleh RFC 7568 (2015); rentan POODLE |
| TLS 1.0 | 1999 (RFC 2246) | Dinyatakan deprecated oleh RFC 8996 (2021) |
| TLS 1.2 | 2008 (RFC 5246) | Masih diizinkan sebagai batas minimum praktik |
| TLS 1.3 | 2018 (RFC 8446) | Versi yang disarankan |

Yang benar-benar dipakai adalah SSL 2.0 dan SSL 3.0, lalu TLS 1.0 yang spesifikasinya berbasis SSL 3.0.

## Tiga jaminan

- **Kerahasiaan.** Data aplikasi dienkripsi dengan kunci simetris; TLS 1.3 mewajibkan AEAD.
- **Autentikasi.** Sertifikat server membuktikan kendali atas domain. Autentikasi client bersifat opsional dan jarang dipakai di web publik.
- **Integritas.** Setiap record dilindungi MAC atau AEAD; tanda tangan digital dipakai pada sertifikat dan pesan `CertificateVerify` di handshake, bukan pada setiap record data.

## Handshake singkat

Pada TLS 1.3, client mengirim `ClientHello` berisi key share, server membalas `ServerHello`, lalu `EncryptedExtensions`, `Certificate`, `CertificateVerify`, dan `Finished`. Kunci sesi diturunkan dari pertukaran (EC)DHE. TLS 1.2 membutuhkan lebih banyak round trip dan masih mengizinkan static RSA key transport, sedangkan TLS 1.3 menghapusnya.

## Sertifikat

Sertifikat X.509 mengikat nama domain (atau daftar SAN) pada kunci publik, lalu ditandatangani CA. Browser membangun rantai dari sertifikat leaf ke intermediate hingga root yang ada di trust store. Sertifikat bersifat publik dan dikirim saat handshake; yang rahasia adalah private key.

Pada TLS modern, private key menandatangani handshake dan tidak mendekripsi data aplikasi. Kunci sesi berasal dari pertukaran ephemeral, sehingga pencurian private key tidak otomatis membuka rekaman lama yang memakai forward secrecy.

| Jenis | Cakupan |
|---|---|
| Single-domain | Satu nama host, misalnya `www.example.com` |
| Wildcard | `*.example.com`: satu label di kiri, misalnya `www` atau `blog` |
| Multi-domain (SAN) | Beberapa nama tidak berhubungan dalam satu sertifikat |

Wildcard tidak mencakup domain dasarnya (`example.com`) dan tidak mencakup beberapa level (`a.b.example.com`); RFC 6125 menempatkan wildcard sebagai label paling kiri secara utuh.

Tingkat validasi:

- **DV (Domain Validation):** CA membuktikan kendali atas domain. Cepat dan murah; tidak membuktikan keberadaan organisasi.
- **OV (Organization Validation):** CA memverifikasi organisasi pemohon.
- **EV (Extended Validation):** verifikasi organisasi lebih ketat. Browser modern tidak lagi menampilkan indikator EV khusus, jadi tampilan gembok sama dengan sertifikat DV.

## SSL versus TLS

TLS 1.0 adalah kelanjutan langsung SSL 3.0. Saat IETF mengambil alih pengembangan, namanya diganti menjadi TLS. Perbedaannya tidak hanya nama: TLS 1.0 mengubah PRF, MAC, kode alert, dan cipher suite. TLS 1.3 adalah desain ulang yang jauh lebih besar: hanya AEAD, handshake 1-RTT, tanpa static RSA, dan tanpa renegosiasi.

Sebutan "SSL encryption" atau "SSL certificate" adalah kebiasaan pasar, bukan penanda protokol. Yang menentukan keamanan adalah versi TLS dan konfigurasi cipher yang benar-benar dinegosiasikan.

## Mengapa SSL ditinggalkan

- SSL 3.0 rentan POODLE (CVE-2014-3566) dan serangan padding oracle pada mode CBC.
- SSL 2.0 lebih lemah lagi: MAC lemah, tidak ada perlindungan terhadap truncation, dan cipher suite export.
- Fallback ke SSLv3 membuka downgrade attack; server harus menolak SSLv2/SSLv3 dan memakai `TLS_FALLBACK_SCSV`.
- Browser modern dan OpenSSL 3.x tidak lagi menyediakan pemilihan SSLv3.

## Koreksi terhadap artikel Cloudflare

1. **"SSL pertama dikembangkan Netscape pada 1995."** SSL 1.0 tidak pernah dirilis publik; 1995 adalah SSL 2.0, dan SSL 3.0 menyusul pada 1996. Artikel sendiri kemudian menyebut "sejak SSL 3.0 pada 1996", jadi kalimat 1995 mencampur pengembangan awal dengan rilis.
2. **"Handshake memastikan kedua perangkat benar-benar seperti yang diklaim."** Pada HTTPS biasa hanya server yang diautentikasi sertifikat. Autentikasi client opsional dan jarang dipakai di web publik.
3. **"SSL menandatangani data secara digital untuk integritas."** Integritas record memakai MAC atau AEAD, bukan tanda tangan digital per record. Tanda tangan muncul di sertifikat dan `CertificateVerify` saat handshake.
4. **"Pada 1999 SSL diperbarui menjadi TLS."** TLS 1.0 distandarkan sebagai RFC 2246 (Januari 1999) dan berbasis SSL 3.0, tetapi bukan sekadar ganti nama: PRF, MAC, alert, dan cipher suite berubah.
5. **"Penyadap hanya melihat campuran karakter yang hampir mustahil didekripsi."** Benar untuk TLS modern yang dikonfigurasi baik, tetapi berlebihan sebagai pernyataan umum tentang SSL 2.0/3.0 yang punya serangan praktis. Metadata seperti alamat IP dan panjang data juga tidak disembunyikan.
6. **"SSL menghentikan serangan tertentu dan mencegah pengubahan data."** TLS melindungi integritas selama transit, tetapi tidak menghentikan phishing dengan sertifikat valid untuk domain mirip, XSS, SQL injection, atau malware. Autentikasi server hanya membuktikan kendali atas domain.
7. **"Private key mendekripsi data yang dienkripsi public key."** Itu berlaku untuk static RSA key transport di TLS 1.2 ke bawah. TLS 1.3 menghapusnya; dengan (EC)DHE, private key hanya menandatangani handshake dan tidak mendekripsi record aplikasi.
8. **"Perangkat pengguna melihat public key lalu membentuk kunci enkripsi dengan server."** Tidak tepat untuk TLS modern: kunci sertifikat mengautentikasi server, sedangkan kunci sesi berasal dari pertukaran ephemeral.
9. **"Wildcard mencakup subdomain domain tersebut."** `*.example.com` mencocokkan satu label di kiri (`www`, `blog`), bukan `example.com` dan bukan `a.b.example.com`. RFC 6125 menempatkan wildcard sebagai label paling kiri secara utuh.
10. **"Single-domain, wildcard, multi-domain."** Kategorinya benar, tetapi multi-domain diimplementasikan sebagai SAN; satu sertifikat bisa memuat banyak nama, dan wildcard dapat digabung dengan entri SAN.
11. **"DV cukup membuktikan bisnis mengontrol domain."** DV membuktikan kendali atas domain, bukan keberadaan atau identitas bisnis. Siapa pun yang mengontrol domain bisa memperolehnya, dan sertifikatnya tidak menyatakan apa pun tentang organisasi.
12. **"OV lebih dipercaya pengguna; EV memerlukan pemeriksaan latar belakang penuh."** OV dan EV memang memverifikasi organisasi, tetapi browser modern tidak lagi menampilkan indikator EV khusus; tampilan gembok sama dengan DV. Kepercayaan berasal dari validasi CA, bukan dari UI.
13. **"SSL tidak diperbarui sejak 1996 dan kini deprecated."** Benar, tetapi sebutkan dasar formalnya: RFC 7568 (Juni 2015) melarang SSLv3, dan SSLv2 sudah dilarang RFC 6176 (Maret 2011).
14. **"Vendor yang menawarkan SSL hampir pasti menyediakan TLS."** Benar sebagai pemakaian pasar, tetapi label adalah pemasaran. Yang perlu diperiksa adalah versi protokol dan cipher yang benar-benar dinegosiasikan.
15. **"Sertifikat SSL disimpan dan ditampilkan oleh server."** Sertifikat bersifat publik dan dikirim saat handshake, bukan rahasia. Yang harus dijaga adalah private key.

## Verifikasi

Fakta protokol dicek ke dokumen primer: RFC 7568 (Juni 2015) menyatakan SSLv3 tidak layak dipakai; RFC 2246 (Januari 1999) adalah TLS 1.0; RFC 8446 (Agustus 2018) adalah TLS 1.3; RFC 6125 (Maret 2011) mengatur pencocokan identitas domain dan wildcard. Pemeriksaan lokal: OpenSSL 3.0.13; `openssl s_client -help` menyediakan `-no_ssl3` tetapi tidak menyediakan pemilih `-ssl3`, jadi SSLv3 tidak bisa dinegosiasikan. Halaman Cloudflare yang diekstrak tidak mencantumkan tanggal terbit. Tidak ada perubahan konfigurasi atau layanan.

## Sumber

- [What is SSL? oleh Cloudflare Learning](https://www.cloudflare.com/learning/ssl/what-is-ssl/): definisi, sejarah SSL/TLS, sertifikat, jenis, dan tingkat validasi. Disimpan sebagai `_raw/articles/cloudflare-what-is-ssl-2026-11-26.md`.
- [RFC 7568: Deprecating Secure Sockets Layer Version 3.0](https://www.rfc-editor.org/rfc/rfc7568), Juni 2015.
- [RFC 2246: The TLS Protocol Version 1.0](https://www.rfc-editor.org/rfc/rfc2246), Januari 1999.
- [RFC 8446: The Transport Layer Security (TLS) Protocol Version 1.3](https://www.rfc-editor.org/rfc/rfc8446), Agustus 2018.
- [RFC 6125: Representation and Verification of Domain-Based Application Service Identity](https://www.rfc-editor.org/rfc/rfc6125), Maret 2011.
