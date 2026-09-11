---
{"dg-publish":true,"dg-path":"OpenID.md","permalink":"/open-id/","title":"OpenID","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"OpenID","categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/openid-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

OpenID merujuk pada standar autentikasi federatif yang memungkinkan aplikasi mempercayai hasil login dari identity provider. Pengguna mengautentikasi diri kepada provider, bukan membagikan password provider kepada setiap website.

## OpenID dan OpenID Connect

OpenID Authentication 2.0 adalah protokol terdahulu yang berbeda dari OpenID Connect (OIDC). Keduanya tidak boleh diperlakukan sebagai nama yang saling menggantikan.

OIDC adalah lapisan identitas di atas [[References/OAuth\|OAuth 2.0]]. Jadi, untuk OIDC, hubungannya bukan sekadar "sering dipakai bersama OAuth": OAuth 2.0 menjadi fondasi protokolnya. OAuth mengatur delegasi akses, sedangkan OIDC menambahkan semantik [[References/Authentication\|Authentication]] pengguna.

Istilah decentralized tidak berarti semua website menerima semua provider. Aplikasi tetap menentukan provider yang dipercaya dan aturan validasi yang harus dipenuhi.

## Alur OIDC secara ringkas

1. Aplikasi mengarahkan pengguna ke OpenID Provider untuk autentikasi.
2. Provider menangani login dan persetujuan sesuai kebijakan serta session yang tersedia.
3. Dalam Authorization Code flow, aplikasi menerima code lalu menukarnya untuk memperoleh token.
4. Aplikasi memvalidasi ID Token sebelum memakai hasil login dan membentuk session lokal.

ID Token berbentuk [[References/JWT\|JWT]] dan membawa klaim tentang hasil autentikasi bagi client yang dituju. ID Token bukan pengganti access token untuk API. Validasi mencakup signature, issuer, audience, expiry, serta nonce bila digunakan atau diwajibkan flow.

## SSO dan batas keamanan

OIDC dapat mendukung Single Sign-On: session pada provider memungkinkan pengguna masuk ke beberapa aplikasi tanpa selalu memasukkan kredensial lagi. Namun aplikasi tetap dapat meminta autentikasi ulang atau persetujuan tambahan sesuai risiko dan kebijakan.

[[SSO adalah hasil federation, bukan satu protokol\|SSO adalah hasil federation, bukan satu protokol]] menjelaskan bahwa SSO juga dapat memakai protokol lain. Logout pada satu aplikasi tidak otomatis mengakhiri seluruh session aplikasi lain atau session provider.

Pemusatan login memudahkan pengelolaan identitas, tetapi menambah ketergantungan pada keamanan dan ketersediaan provider. Izin atas resource tetap diperiksa oleh aplikasi; login federatif tidak menggantikan authorization.

## Sumber

- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html): lapisan identitas, ID Token, dan validasi.
- [[References/Authentication Strategies\|Authentication Strategies]]: evidence vault tentang OIDC, federation, SSO, dan lifecycle session.
