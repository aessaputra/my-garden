---
{"dg-publish":true,"dg-path":"OAuth.md","permalink":"/o-auth/","title":"OAuth","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"OAuth","aliases":["OAuth 2.0"],"categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/oauth-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md","https://www.rfc-editor.org/rfc/rfc6749.html","https://www.rfc-editor.org/rfc/rfc9700.html"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

OAuth 2.0 adalah framework authorization yang memungkinkan aplikasi memperoleh akses terbatas ke resource tanpa meminta pengguna menyerahkan password akun layanan tersebut kepada aplikasi. Akses diberikan melalui access token, bukan dengan membagikan password pengguna.

## Empat peran

- **Resource owner**: pihak yang berwenang memberikan akses, biasanya pengguna.
- **Client**: aplikasi yang meminta dan menggunakan akses.
- **Authorization server**: memproses authorization grant dan menerbitkan token.
- **Resource server**: API yang menerima token dan menegakkan hak akses.

Authorization server dan resource server dapat menjadi bagian dari layanan yang sama, tetapi tanggung jawabnya berbeda.

## Alur umum

Pada Authorization Code flow, client mengarahkan pengguna ke authorization server. Setelah autentikasi dan persetujuan sesuai kebijakan layanan, client menerima authorization code, menukarnya dengan access token, lalu memakai token untuk memanggil API.

Tidak setiap penerbitan token membutuhkan persetujuan interaktif baru. Client Credentials grant, misalnya, melayani akses aplikasi atas namanya sendiri tanpa pengguna yang sedang login.

## Batas keamanan

OAuth bukan protokol [[References/Authentication\|Authentication]] pengguna. [[References/OpenID\|OpenID Connect]] menambahkan lapisan identitas untuk login; [[OAuth mendelegasikan akses, bukan membuktikan identitas\|OAuth mendelegasikan akses, bukan membuktikan identitas]] menjelaskan perbedaan ini.

Access token dapat berupa string opaque atau [[References/JWT\|JWT]]. Scope membatasi cakupan akses, tetapi server tetap harus memeriksa izin pada resource dan operasi yang diminta. Token yang bocor dapat disalahgunakan sesuai hak yang dibawanya.

Untuk aplikasi browser dan native, gunakan Authorization Code dengan PKCE, HTTPS, validasi redirect URI yang ketat, serta validasi token sesuai profilnya. OAuth tidak otomatis aman hanya karena memakai token.

## Sumber

- [RFC 6749: The OAuth 2.0 Authorization Framework](https://www.rfc-editor.org/rfc/rfc6749.html)
- [RFC 9700: Best Current Practice for OAuth 2.0 Security](https://www.rfc-editor.org/rfc/rfc9700.html)
