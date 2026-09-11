---
{"dg-publish":true,"dg-path":"SAML.md","permalink":"/saml/","title":"SAML","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"SAML","aliases":["Security Assertion Markup Language"],"categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/saml-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

SAML (Security Assertion Markup Language) adalah standar berbasis XML untuk pertukaran pernyataan keamanan antarpihak yang saling percaya. SAML 2.0 umum dipakai untuk identity federation dan Single Sign-On pada aplikasi enterprise.

## Peran dan assertion

- **Identity Provider (IdP)** mengautentikasi pengguna dan menerbitkan assertion.
- **Service Provider (SP)** menerima serta memvalidasi assertion sebelum memberikan akses melalui session aplikasi.

Assertion dapat membawa pernyataan autentikasi, atribut pengguna, atau keputusan authorization. Tidak setiap assertion memuat seluruh jenis pernyataan tersebut. Atribut seperti grup atau role dapat menjadi masukan kebijakan, tetapi SP tetap menentukan dan menegakkan izin atas resource.

## Alur SSO umum

Dalam SP-initiated Web Browser SSO, pengguna membuka aplikasi dan diarahkan ke IdP. Setelah login atau penggunaan session IdP yang masih berlaku, IdP mengirim response SAML melalui browser ke endpoint penerima SP. SP memvalidasinya sebelum membentuk session lokal.

Password pengguna tidak perlu diserahkan kepada setiap SP. [[SSO adalah hasil federation, bukan satu protokol\|SSO adalah hasil federation, bukan satu protokol]] menjelaskan manfaat satu login lintas aplikasi dan ketergantungannya pada provider. [[References/OpenID\|OpenID Connect]] adalah pendekatan federasi lain, bukan nama lain untuk SAML.

## Keamanan dan batas

Gunakan library SAML yang terpelihara. SP harus memverifikasi signature sesuai profil dan konfigurasi trust, issuer, audience, tujuan penerima, batas waktu, serta keterkaitan dengan request bila berlaku. Pencegahan replay diperlukan agar assertion yang sudah dipakai tidak diterima kembali secara tidak sah.

XML dan Base64 bukan enkripsi. Lindungi pertukaran dengan [[References/HTTPS\|HTTPS]]; signature menjamin integritas dan autentisitas bila diverifikasi benar, bukan kerahasiaan isi. Kelola metadata dan rotasi sertifikat secara terkendali.

Pemusatan autentikasi dapat menyederhanakan pengelolaan akses, tetapi SAML sendiri tidak otomatis membuat, memperbarui, atau menghapus akun di semua aplikasi. Provisioning dan offboarding memerlukan integrasi serta kebijakan tambahan.

Logout pada satu SP juga tidak otomatis mengakhiri seluruh session. Dukungan Single Logout dan session lokal perlu dikelola eksplisit. [[References/Authentication Strategies\|Authentication Strategies]] membandingkan trade-off SAML, OIDC, dan pendekatan lain.

## Sumber

- [OASIS: SAML V2.0 Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html): peran, assertion, dan federation.
- [[References/Authentication Strategies\|Authentication Strategies]]: sintesis serta evidence SSO yang tersedia di vault.
