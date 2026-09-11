---
{"dg-publish":true,"dg-path":"Token Authentication.md","permalink":"/token-authentication/","title":"Token Authentication","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"Token Authentication","categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/token-authentication-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Token-based authentication menggunakan token sebagai kredensial untuk request berikutnya setelah login berhasil atau grant lain diberikan. Client tidak perlu mengirim ulang password selama token diterima server. Token dapat mewakili pengguna atau aplikasi, bukan selalu identitas manusia.

## Cara kerja

Client memperoleh token, lalu mengirimkannya saat mengakses resource yang dilindungi. Salah satu bentuk pengiriman adalah header berikut, dengan nilai ilustratif:

```http
Authorization: Bearer <access_token>
```

Server memvalidasi token dan memeriksa izin terhadap resource yang diminta. Seperti tiket, token membawa bukti akses, tetapi penerima tetap harus memeriksa keabsahan dan batas penggunaannya. [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] menjelaskan pemisahan ini.

Token dapat berupa nilai opaque yang diperiksa melalui state server atau introspection, maupun [[References/JWT\|JWT]] yang dapat divalidasi secara lokal. Token tidak harus berbentuk JWT, dan penggunaan token tidak selalu berarti sistem stateless.

## Logout dan keamanan

Menghapus token dari client saat logout tidak membatalkan salinan yang sudah dicuri. Opaque token dapat dicabut melalui state server bila setiap request memeriksa statusnya. JWT yang hanya divalidasi lokal dapat tetap diterima sampai expiry, kecuali server menerapkan mekanisme pencabutan seperti denylist atau pemeriksaan status tambahan.

Bearer token dapat digunakan siapa pun yang memilikinya. Lindungi dengan [[References/HTTPS\|HTTPS]], penyimpanan yang sesuai dengan jenis client, dan hindari kebocoran melalui URL atau log. Batasi masa berlaku, audience, dan scope sesuai kebutuhan.

Token bukan faktor keamanan kedua seperti MFA. Kontrol administratif, pembatasan izin, rotasi, dan pencabutan harus diterapkan oleh sistem; semuanya bukan jaminan otomatis dari penggunaan token. [[References/Authentication Strategies\|Authentication Strategies]] membahas trade-off dengan session, Basic, dan pendekatan lain.

## Sumber

- [RFC 6750: OAuth 2.0 Bearer Token Usage](https://www.rfc-editor.org/rfc/rfc6750.html).
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).
- [[References/Authentication Strategies\|Authentication Strategies]]: sintesis format token, validasi, dan pencabutan dari paket riset vault.
