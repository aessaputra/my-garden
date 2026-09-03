---
{"dg-publish":true,"permalink":"/https-protects-transit-not-application-logic/","title":"HTTPS protects transit, not application logic","hideInFiletree":true,"tags":["security","network"],"noteIcon":"","dg-note-properties":{"title":"HTTPS protects transit, not application logic","categories":["Web Security"],"tags":["security","network"],"sources":["_raw/articles/web-security-knowledge-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A login page shows a padlock icon while storing passwords with reversible encryption behind it.

I treat HTTPS as protection for data in transit, never as proof that the application itself is safe.

MDN [describes the TLS handshake](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security) as agreement on version, cipher suite, server authentication, and a shared secret.

Version 1.3 is current, 1.2 lingers on some sites, and versions 1.0 and 1.1 must no longer be used.

If [[OWASP Top 10 builds awareness, not a complete checklist\|OWASP Top 10 builds awareness, not a complete checklist]] names injection and weak access, then [[Parameterized queries separate data from commands\|Parameterized queries separate data from commands]] fixes what encryption cannot.

I enforce HTTPS everywhere, then harden the code that sits behind it.
