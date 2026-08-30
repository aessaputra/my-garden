---
{"dg-publish":true,"dg-path":"HTTP","permalink":"/http/","title":"HTTP","hideInFiletree":true,"tags":["network","http","ssl","security","guide"],"dg-note-properties":{"title":"HTTP","category":"references","tags":["network","http","ssl","security","guide"],"sources":["_raw/articles/http-in-depth-cs-fyi.md","_raw/articles/http-cloudflare.md","_raw/articles/http3-thenewstack.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

HTTP (Hypertext Transfer Protocol) adalah protokol aplikasi berbasis TCP/IP yang menstandarkan komunikasi client-server di web. Halaman ini menyintesis 3 sumber: sejarah lengkap dari cs.fyi, anatomi request/response dari Cloudflare, dan HTTP/3 dari The New Stack.

## Apa itu HTTP

### HTTP/3 (standar IETF 2022)
HTTP-over-QUIC. QUIC dikembangkan Google, memakai UDP (fire-and-forget) alih-alih TCP. Mengatasi masalah TCP di jaringan mobile: koneksi terputus saat pindah menara seluler; QUIC mempertahankan koneksi dengan connection identifier dan stream yang tidak saling mengganggu.

- Selalu terenkripsi (built-in security).
- Chrome v87+ mendukung; Safari tertinggal.
- Cek dukungan: cloudflare-quic.com, quic.nginx.org, http3.is; tes situs: geekflare.com/tools/http3-test.
- Cara termudah pakai: lewat CDN (Cloudflare, Fastly).
- Implementasi: aioquic (Python), quic-go (Go), quiche/Quinn/Neqo/s2n-quic (Rust), mvfst/MsQuic/LSQUIC/picoquic/quicly (C/C++). Ruby belum ada.

## HTTP dan keamanan

HTTP stateless: tiap command independen. HTTP/1.1+ mendukung persistent connection. Request HTTP dalam jumlah besar bisa dipakai untuk DDoS: termasuk application layer attacks (layer 7).

Untuk komunikasi aman: HTTPS = HTTP + SSL/TLS (lihat [[References/How Does Internet Work\|How Does Internet Work]] bagian SSL/TLS).

## Lihat juga

- [[What is Internet\|What is Internet]]: dasar internet, HTTP/HTML sebagai bagian dari stack
- [[References/How Does Internet Work\|How Does Internet Work]]: TCP/IP, port, socket, TLS, infrastruktur Internet, serta perbedaan Internet dan Web
- [[References/Jaringan (Network)\|Jaringan (Network)]]: implementasi networking di React Native (Fetch API, WebSocket)