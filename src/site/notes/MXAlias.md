---
{"dg-publish":true,"dg-permalink":"craft/mx-alias","permalink":"/craft/mx-alias/","title":"MX Alias","hideInFiletree":true,"tags":["craft"],"noteIcon":"","dg-note-properties":{"title":"MX Alias","tags":["craft"],"created":"2026-09-14","updated":"2026-09-14"}}
---

![mx-alias-full-front.webp\|412](/img/user/Attachments/mx-alias-full-front.webp)
MX Alias dashboard web self-hosted berbasis Next.js 16, React 19, dan Node.js 24 untuk mengelola email forwarder MXroute per domain tanpa database, dengan fokus pada kecepatan, kesederhanaan, dan arsitektur stateless.

Akses admin mendukung password dan OIDC. Keamanan diperkuat token HMAC pada cookie HttpOnly/Secure, validasi Origin anti-CSRF, dan isolasi kredensial.

Pengelolaan alias mencakup pemilihan domain, pembuatan manual atau generator adjective-noun-angka, tujuan forwarding, daftar forwarder, salin alamat, penghapusan dengan konfirmasi, filter DISALLOWED_DOMAINS, dan Refresh.

Data domain dicache 5 menit dan forwarder per domain 60 detik.

# Tautan
* [MX Alias](https://mxalias.aes.my.id)