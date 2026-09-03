---
{"dg-publish":true,"permalink":"/session-server-memberi-revocation-cepat-dengan-biaya-state/","title":"Session server memberi revocation cepat dengan biaya state","hideInFiletree":true,"tags":["auth","security","sessions"],"noteIcon":"","dg-note-properties":{"title":"Session server memberi revocation cepat dengan biaya state","categories":["Authentication Strategies"],"tags":["auth","security","sessions"],"sources":["_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-03","updated":"2026-09-04"}}
---

Aplikasi web first-party butuh logout seketika saat perangkat hilang atau kredensial dicuri penyerang.

Session server menukar hasil login dengan ID opaque, sehingga pencabutan terpusat menjadi cepat dan teraudit.

Server menyimpan state autentikasi lalu memberi client session ID acak melalui cookie menurut [OWASP Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

Cookie web membutuhkan flag Secure dan HttpOnly plus kebijakan SameSite dan proteksi CSRF yang sesuai.

Store terpusat menyederhanakan pencabutan dan audit tetapi menuntut replikasi, cleanup, dan availability tinggi.

Jika [[JWT adalah format klaim, bukan strategi autentikasi\|JWT adalah format klaim, bukan strategi autentikasi]] mengorbankan revocation, session memilih arah berlawanan. [[OAuth mendelegasikan akses, bukan membuktikan identitas\|OAuth mendelegasikan akses, bukan membuktikan identitas]] tetap butuh session lokal untuk logout konsisten.

Session server tepat untuk web first-party yang butuh pencabutan ketat dan audit terpusat.
