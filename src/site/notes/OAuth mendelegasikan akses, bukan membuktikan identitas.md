---
{"dg-publish":true,"permalink":"/o-auth-mendelegasikan-akses-bukan-membuktikan-identitas/","title":"OAuth mendelegasikan akses, bukan membuktikan identitas","hideInFiletree":true,"tags":["auth","security","oauth"],"noteIcon":"","dg-note-properties":{"title":"OAuth mendelegasikan akses, bukan membuktikan identitas","categories":["Authentication Strategies"],"tags":["auth","security","oauth"],"sources":["_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-03","updated":"2026-09-04"}}
---

Tombol Login with Google sering dikira OAuth murni padahal identitasnya berasal dari lapisan OpenID Connect.

OAuth hanya mendelegasikan akses terbatas, sehingga pembuktian identitas membutuhkan lapisan autentikasi terpisah.

[RFC 6749](https://www.rfc-editor.org/rfc/rfc6749.html) memosisikan OAuth sebagai framework akses pihak ketiga atas nama resource owner.

Authorization Code dengan PKCE menjadi baseline browser dan native menurut [RFC 9700](https://www.rfc-editor.org/rfc/rfc9700.html) tentang keamanan terkini.

Scope dan audience harus sempit agar kebocoran satu token tidak membuka seluruh resource pengguna.

Jika [[JWT adalah format klaim, bukan strategi autentikasi\|JWT adalah format klaim, bukan strategi autentikasi]] dipakai sebagai access token, delegasi tetap butuh validasi ketat. [[SSO adalah hasil federation, bukan satu protokol\|SSO adalah hasil federation, bukan satu protokol]] dibangun dari delegasi aman semacam itu.

OAuth tepat untuk delegasi API sedangkan OIDC tepat untuk login federatif yang terverifikasi.
