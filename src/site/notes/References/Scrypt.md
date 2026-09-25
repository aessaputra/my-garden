---
{"dg-publish":true,"dg-path":"Scrypt.md","permalink":"/scrypt/","title":"Scrypt","hideInFiletree":true,"tags":["references","security","algorithms"],"noteIcon":"","dg-note-properties":{"title":"Scrypt","aliases":["scrypt"],"categories":["Cryptography"],"type":"reference","tags":["references","security","algorithms"],"sources":["_raw/articles/scrypt-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

scrypt adalah key derivation function yang memory-hard, dirancang oleh Colin Percival (RFC 7914). Ia menggabungkan fungsi hash dengan kebutuhan memori besar dan komputasi CPU intensif agar serangan brute force skala besar menjadi mahal dan tidak praktis, termasuk yang memakai GPU atau ASIC.

Berbeda dengan fungsi hash cepat seperti anggota [[References/SHA Family\|SHA Family]] — yang cocok untuk integritas tetapi salah untuk password — scrypt sengaja lambat dan boros memori. Itu melengkapi larangan memakai [[References/MD5\|MD5]] untuk kredensial yang sudah dinyatakan di [[References/Web Security Knowledge\|Web Security Knowledge]].

## Cara kerja singkat

scrypt memakai tiga parameter biaya: N (faktor CPU/memori), r (ukuran blok), dan p (paralelisasi). Nilai N yang besar memaksa penyerang menyediakan RAM besar per tebakan, sehingga hardware khusus kehilangan keunggulan biaya dibanding CPU biasa.

Parameter harus dipilih sesuai kemampuan server dan ancaman saat ini, lalu ditinjau berkala. Nilai yang terlalu rendah melemahkan proteksi; terlalu tinggi membuka risiko denial-of-service lewat login yang mahal.

## Batas pemakaian

Gunakan [[References/Scrypt\|scrypt]] (atau [[References/Bcrypt\|bcrypt]], Argon2) dengan salt acak per password untuk penyimpanan kredensial. Lihat [[References/Authentication Strategies\|Authentication Strategies]] untuk pemilihan strategi dan [[References/Password Security di Supabase\|Password Security di Supabase]] untuk contoh operasional bcrypt.

Untuk mining cryptocurrency, scrypt dipakai sebagai proof-of-work yang tahan ASIC pada waktunya — konteks berbeda dari keamanan password dan tidak dibahas lebih jauh di sini.

## Sumber

- [RFC 7914 — The scrypt Password-Based Key Derivation Function](https://www.rfc-editor.org/rfc/rfc7914.html)
