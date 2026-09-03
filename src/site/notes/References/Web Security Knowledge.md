---
{"dg-publish":true,"dg-path":"Web Security Knowledge.md","permalink":"/web-security-knowledge/","title":"Web Security Knowledge","hideInFiletree":true,"tags":["references","security","programming","network","auth"],"noteIcon":"","dg-note-properties":{"title":"Web Security Knowledge","category":"references","tags":["references","security","programming","network","auth"],"sources":["_raw/articles/web-security-knowledge-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04","confidence":"high"}}
---

Web security melindungi website dari cyber threats selama in transit, di browser, dan di server. Ia memadukan HTTPS/TLS, XSS/SQL injection/CSRF prevention, CSP, secure authentication, input validation, dan regular updates.

Halaman ini merangkum peta lintas lapisan. Detail tiap lapisan tetap di [[References/OWASP Security Risks\|OWASP Security Risks]], [[References/Content Security Policy\|Content Security Policy]], [[References/HTTPS\|HTTPS]], dan [[References/Authentication Strategies\|Authentication Strategies]] tanpa disalin ulang ke sini.

## OWASP Top 10:2025 sebagai awareness

OWASP Top 10 adalah awareness document bagi developers dan web application security practitioners. Ia merekam broad consensus soal most critical security risks, bukan complete verification standard. Lihat [halaman proyek OWASP](https://owasp.org/www-project-top-ten).

Edisi 2025 memuat ten categories: A01 Broken Access Control, A02 Security Misconfiguration, A03 Software Supply Chain Failures, A04 Cryptographic Failures, A05 Injection, A06 Insecure Design, A07 Authentication Failures, A08 Software or Data Integrity Failures, A09 Security Logging and Alerting Failures, dan A10 Mishandling of Exceptional Conditions. Lihat [pengantar edisi 2025](https://owasp.org/Top10/2025/0x00_2025-Introduction).

Global ranking tidak menentukan remediation order tiap system. Team menggabungkan Top 10 dengan asset inventory, data classification, threat model, architecture, business context, dan regulasi. Lihat [[OWASP Top 10:2025\|OWASP Top 10:2025]].

## HTTPS dan TLS

HTTPS adalah HTTP melalui TLS-secured channel dengan tiga properties: confidentiality, integrity, dan authentication. Proteksi berlaku pada header dan body setelah handshake selesai.

Handshake menyepakati protocol version dan cipher suite, membentuk shared secret, serta melakukan server authentication. Lihat [panduan TLS MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security).

TLS 1.3 adalah current version, 1.2 masih dipakai sebagian website, sedangkan 1.0 dan 1.1 tidak boleh dipakai lagi. Banyak powerful Web API hanya tersedia dalam secure contexts. Lihat [dokumen secure contexts](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Secure_Contexts).

Ikon gembok hanya membuktikan encrypted channel ke valid certificate holder. Ia tidak menjamin isi website trustworthy. Lihat [[References/HTTPS\|HTTPS]].

## Content Security Policy

CSP adalah browser security mechanism yang membatasi resource dan behavior yang diizinkan pada sebuah document. Policy dikirim lewat response header `Content-Security-Policy` pada semua responses, bukan hanya main document. Lihat [panduan CSP MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP).

Policy disusun dari semicolon-separated directives dengan name-value pair tanpa tanda baca internal. Fetch directives mengontrol allowed locations tiap resource type. Lihat [referensi header CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy).

Strict CSP berbasis nonce atau hash direkomendasikan untuk scripts. Nonce dibuat ulang per response, hash mengunci content digest dengan SHA-256, SHA-384, atau SHA-512. CSP adalah layered defense, bukan pengganti input validation, output encoding, dan sanitization. Lihat [[References/Content Security Policy\|Content Security Policy]].

## Injection

Injection terjadi saat untrusted input dikirim ke interpreter dan sebagian input diperlakukan sebagai command. Bentuknya mencakup SQL injection, NoSQL, OS command, LDAP, expression language, dan XSS.

Pertahanan utama adalah memisahkan data dari commands lewat secure API dan parameterized queries. Allowlist validation sisi server-side dan context-aware escaping melengkapi, tidak menggantikan parameterization. SAST, DAST, IAST, dan fuzzing menjadi detection layer di CI/CD pipeline.

## CSRF dan session

Browser mengirim cookies otomatis, sehingga attacker-crafted request ikut terautentikasi. Web cookies memakai flags `Secure`, `HttpOnly`, dan kebijakan `SameSite` yang sesuai, tetapi explicit CSRF protection tetap wajib pada risky contexts.

Aplikasi memakai server-side session manager dengan random identifier baru setelah login, menyimpannya dalam secure cookie, dan melakukan invalidation saat logout atau expiry. Lihat [[References/Authentication Strategies\|Authentication Strategies]].

## Supply chain, update, dan logging

Kategori supply chain failures meluas dari vulnerable components ke seluruh build, distribution, dan update process. Pencegahannya memakai component inventory dan SBOM, trusted package sources, signature verification, risk-based updates, serta access control dan MFA pada CI/CD.

Aplikasi yang tidak melakukan security logging, monitoring, atau alerting atas security events tidak bisa merespons serangan. Failed logins, log integrity, alert threshold, dan response playbook perlu dirancang sejak awal.

## Memilih

Gunakan OWASP Top 10 untuk priority, HTTPS untuk transit, strict CSP untuk script containment, parameterized queries untuk injection, anti-CSRF token untuk session, serta updates dan logging untuk response loop.

Periksa runtime support dan konteks tiap control, karena availability berbeda antar browser, framework, dan environment.

## Batasan

Fetch halaman redirect OWASP 2025 hanya stub 258 char sehingga digantikan intro page 0x00. Dua category URLs tanpa year prefix 404 pada fetch, detailnya mengandalkan raw vault 2026-08-29.

Fetch MDN dibatasi 5000 char per URL. Impact magnitude dan remediation order per organisasi tidak diklaim di sini.

## Terkait

- [[OWASP Top 10 builds awareness, not a complete checklist\|OWASP Top 10 builds awareness, not a complete checklist]]
- [[HTTPS protects transit, not application logic\|HTTPS protects transit, not application logic]]
- [[Strict CSP contains injected scripts by default\|Strict CSP contains injected scripts by default]]
- [[Parameterized queries separate data from commands\|Parameterized queries separate data from commands]]
- [[Secure cookies still need explicit CSRF defense\|Secure cookies still need explicit CSRF defense]]
- [[Updates and logging close the loop attackers exploit\|Updates and logging close the loop attackers exploit]]
- [[References/OWASP Security Risks\|OWASP Security Risks]]
- [[References/Content Security Policy\|Content Security Policy]]
- [[References/HTTPS\|HTTPS]]
- [[References/Authentication Strategies\|Authentication Strategies]]
- [[OWASP Top 10:2025\|OWASP Top 10:2025]]

## Sumber

- [OWASP Top Ten Web Application Security Risks](https://owasp.org/www-project-top-ten): OWASP, awareness document dan broad consensus most critical risks.
- [Introduction - OWASP Top 10:2025](https://owasp.org/Top10/2025/0x00_2025-Introduction): OWASP, daftar category A01 sampai A10 edisi 2025.
- [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP): MDN, response header delivery dan directive composition.
- [Content-Security-Policy header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy): MDN, syntax dan fetch directives.
- [Transport Layer Security](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security): MDN, handshake, versions, dan cipher suite.
- [Secure contexts](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Secure_Contexts): MDN, authentication dan confidentiality requirements untuk powerful API.
