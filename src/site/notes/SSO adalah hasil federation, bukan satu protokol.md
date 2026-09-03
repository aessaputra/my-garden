---
{"dg-publish":true,"permalink":"/sso-adalah-hasil-federation-bukan-satu-protokol/","title":"SSO adalah hasil federation, bukan satu protokol","hideInFiletree":true,"tags":["auth","security","architecture"],"noteIcon":"","dg-note-properties":{"title":"SSO adalah hasil federation, bukan satu protokol","categories":["Authentication Strategies"],"tags":["auth","security","architecture"],"sources":["_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-03","updated":"2026-09-04"}}
---

Karyawan ingin satu login membuka banyak aplikasi tanpa mengetik ulang password di setiap layanan.

SSO adalah pengalaman hasil federation, sehingga keamanannya ditentukan oleh trust ke identity provider.

Enterprise SSO umum memakai SAML atau OIDC dengan autentikasi terpusat menurut [SAML Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html).

Pemusatan menyederhanakan onboarding, offboarding, dan audit tetapi memperbesar dampak gangguan identity provider.

Single logout lintas aplikasi tidak selalu konsisten sehingga session lokal tetap harus dikelola eksplisit.

Jika [[OAuth mendelegasikan akses, bukan membuktikan identitas\|OAuth mendelegasikan akses, bukan membuktikan identitas]] mendelegasikan akses, federation menaikkannya menjadi satu login. [[Session server memberi revocation cepat dengan biaya state\|Session server memberi revocation cepat dengan biaya state]] tetap mengatur logout lokal yang konsisten.

SSO tepat bila MFA, monitoring, dan recovery dirancang untuk menahan blast radius IdP.
