---
{"dg-publish":true,"dg-path":"JWT.md","permalink":"/jwt/","title":"JWT","hideInFiletree":true,"tags":["references","security","jwt"],"noteIcon":"","dg-note-properties":{"title":"JWT","aliases":["JSON Web Token"],"categories":["APIs"],"type":"reference","tags":["references","security","jwt"],"sources":["_raw/articles/jwt-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md","https://www.rfc-editor.org/rfc/rfc7519.html"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

JWT (JSON Web Token) adalah format ringkas untuk membawa klaim sebagai objek JSON. JWT sering digunakan dalam alur [[References/Authentication\|Authentication]] dan authorization pada aplikasi web atau mobile, tetapi bukan protokol login atau jaminan keamanan dengan sendirinya.

## Struktur

JWT bertanda tangan dalam format JWS Compact memiliki tiga bagian yang dipisahkan titik: `header.payload.signature`.

- **Header**: metadata, termasuk algoritme dan penanda tipe bila digunakan.
- **Payload**: klaim, misalnya subjek (`sub`), penerbit (`iss`), penerima (`aud`), dan waktu kedaluwarsa (`exp`).
- **Signature**: memungkinkan penerima memverifikasi integritas serta keaslian dengan kunci yang sesuai. Pada algoritme simetris, bagian ini berupa MAC.

Header dan payload menggunakan encoding Base64url, bukan enkripsi. JWT terenkripsi memakai JWE, yang memiliki lima bagian dalam format compact. Jadi, tidak semua JWT terdiri dari tiga bagian.

## Penggunaan dan batasan

Access token berbentuk JWT sering dikirim melalui header HTTP `Authorization: Bearer <token>`. Klaim yang dibawanya memungkinkan validasi lokal sesuai profil token, tetapi tidak menghilangkan kebutuhan kunci tepercaya dan kebijakan akses.

Server harus memverifikasi signature, membatasi algoritme yang diterima, serta memeriksa issuer, audience, masa berlaku, dan klaim wajib sesuai profilnya. Membaca atau mendekode payload saja bukan validasi.

Payload bertanda tangan biasa dapat dibaca pemegang token. Jangan menaruh secret di dalamnya; gunakan HTTPS dan lindungi token dari kebocoran. Logout juga tidak otomatis membatalkan semua JWT yang masih berlaku.

[[JWT adalah format klaim, bukan strategi autentikasi\|JWT adalah format klaim, bukan strategi autentikasi]] membahas implikasi desainnya. [[References/Authentication Strategies\|Authentication Strategies]] membandingkan JWT dengan session dan pendekatan lain, termasuk kebutuhan pencabutan akses.

## Sumber

- [RFC 7519: JSON Web Token](https://www.rfc-editor.org/rfc/rfc7519.html)
