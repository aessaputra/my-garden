---
{"dg-publish":true,"dg-path":"Basic Authentication.md","permalink":"/basic-authentication/","title":"Basic Authentication","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"Basic Authentication","categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/basic-authentication-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Basic Authentication adalah skema [[References/Authentication\|Authentication]] pada HTTP yang mengirim pasangan `username:password` dalam header `Authorization`, setelah dikodekan dengan Base64.

```http
Authorization: Basic dXNlcjpwYXNz
```

Contoh tersebut adalah encoding dari kredensial ilustratif `user:pass`, bukan enkripsi. Siapa pun yang memperoleh nilai header dapat mengembalikan teks aslinya.

## Keamanan dan penggunaan

Basic harus digunakan melalui [[References/HTTPS\|HTTPS]] dengan validasi sertifikat yang benar. TLS melindungi kredensial selama transit, tetapi tidak melindunginya dari endpoint yang disusupi atau pencatatan header ke log.

Implementasinya sederhana dan dukungannya luas, tetapi kredensial reusable dikirim pada setiap request yang memakai Basic. Protokol ini tidak menyediakan expiry token, scope, MFA, atau logout terstandar secara native.

Basic dapat memadai untuk integrasi terbatas atau sistem legacy dengan TLS, pembatasan akses, rate limiting, serta pengelolaan dan rotasi secret. Label "risiko rendah" saja tidak cukup untuk menentukan kelayakannya. Hindari kredensial dalam URL, source code, dan log.

Basic bukan fallback yang otomatis aman: jalur alternatif yang melewati MFA atau autentikasi lebih kuat dapat melemahkan seluruh sistem. [[References/Authentication Strategies\|Authentication Strategies]] membandingkan alternatif berdasarkan kebutuhan dan risiko.

## Sumber

- [RFC 7617: The 'Basic' HTTP Authentication Scheme](https://www.rfc-editor.org/rfc/rfc7617.html).
- [[References/Authentication Strategies\|Authentication Strategies]]: sintesis keamanan dan trade-off dari paket riset vault.
