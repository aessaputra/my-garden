---
{"dg-publish":true,"dg-path":"Authentication.md","permalink":"/authentication/","title":"Authentication","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"Authentication","categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/authentication-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Authentication adalah proses memverifikasi identitas pemanggil melalui kredensial yang dapat divalidasi. Pada API, proses ini membantu membedakan pemanggil yang dikenali dari request tanpa kredensial valid. Authorization kemudian menentukan tindakan dan resource yang boleh diakses.

## Authentication pada API

Pada [[References/APIs\|APIs]], autentikasi memvalidasi kredensial pemanggil. API key biasanya mengidentifikasi aplikasi atau proyek, bukan otomatis pengguna manusia. Key dapat membantu atribusi penggunaan dan kuota jika server menerapkan pencatatan serta pembatasannya.

API key dan [[References/Basic Authentication\|Basic Authentication]] adalah mekanisme kredensial; [[References/OAuth\|OAuth 2.0]] adalah framework authorization; [[References/JWT\|JWT]] adalah format token. Istilah tersebut bukan kategori yang setara. Untuk login berbasis OAuth, [[References/OpenID\|OpenID Connect]] menyediakan lapisan identitas.

[[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] menjelaskan batas utamanya: kredensial valid tidak memberikan izin ke semua resource. Kontrol granular memerlukan pemeriksaan scope, role, kepemilikan objek, atau kebijakan lain pada server.

[[References/OpenAPI Specification\|OpenAPI Specification]] dapat mendokumentasikan skema keamanan API, tetapi tidak menegakkannya. Lindungi kredensial dengan HTTPS, jangan sertakan secret dalam URL atau log, dan sediakan rotasi serta pencabutan.

[[References/Token Authentication\|Token Authentication]] menjelaskan penggunaan token pada request berikutnya serta batas expiry, logout, dan pencabutan. [[References/Cookie-Based Authentication\|Cookie-Based Authentication]] membahas pengiriman kredensial melalui browser, session server, dan proteksi CSRF.

## Pembahasan lanjutan

[[References/SAML\|SAML]] menjelaskan pertukaran assertion antara identity provider dan service provider untuk login federatif.

[[References/Authentication Strategies\|Authentication Strategies]] membandingkan pendekatan autentikasi, pengelolaan session dan token, MFA, serta risiko penerapannya. Halaman ini berfokus pada pengantar authentication untuk API.

## Sumber

- [[References/Authentication Strategies\|Authentication Strategies]]: pembahasan strategi dan sumber riset yang mendasarinya.
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html): pemeriksaan izin pada setiap request.
