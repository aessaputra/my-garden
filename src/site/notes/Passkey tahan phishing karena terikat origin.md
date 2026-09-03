---
{"dg-publish":true,"permalink":"/passkey-tahan-phishing-karena-terikat-origin/","title":"Passkey tahan phishing karena terikat origin","hideInFiletree":true,"tags":["auth","security"],"noteIcon":"","dg-note-properties":{"title":"Passkey tahan phishing karena terikat origin","categories":["Authentication Strategies"],"tags":["auth","security"],"sources":["_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-03","updated":"2026-09-04"}}
---

Password dan OTP satu arah dapat diketik ulang korban ke situs palsu yang meniru halaman login.

Passkey mengganti secret bersama dengan kredensial kunci publik yang terikat pada origin relying party.

Server hanya menyimpan public key sedangkan private key bertahan di authenticator menurut [WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/).

Binding origin membuat kredensial gagal dipakai ulang pada domain palsu walau pengguna tertipu tampilan.

Sinkronisasi antarperangkat dan jalur recovery tetap harus dirancang menurut [FIDO Passkeys](https://fidoalliance.org/passkeys/) agar fallback tidak melemahkan jaminan.

Dibangun di atas [[Session server memberi revocation cepat dengan biaya state\|Session server memberi revocation cepat dengan biaya state]], passkey memperkecil phishing pada faktor awal. [[OAuth mendelegasikan akses, bukan membuktikan identitas\|OAuth mendelegasikan akses, bukan membuktikan identitas]] mendapat assurance lebih tinggi karenanya.

Passkey tepat untuk assurance tinggi bila recovery dan fallback diaudit ketat.
