---
{"dg-publish":true,"dg-path":"MD5.md","permalink":"/md-5/","title":"MD5","hideInFiletree":true,"tags":["references","security","algorithms"],"noteIcon":"","dg-note-properties":{"title":"MD5","aliases":["Message-Digest Algorithm 5"],"categories":["Cryptography"],"type":"reference","tags":["references","security","algorithms"],"sources":["_raw/articles/md5-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

MD5 (Message-Digest Algorithm 5) adalah fungsi hash yang menghasilkan digest 128-bit, biasanya ditulis sebagai 32 karakter heksadesimal. Dirancang oleh Ronald Rivest pada 1991 (RFC 1321), MD5 dulu populer untuk checksum integritas data dan penyimpanan password.

Dalam peta [[References/Web Security Knowledge\|Web Security Knowledge]], MD5 adalah contoh kegagalan kriptografi (A04 OWASP): fungsi hash yang rusak tidak boleh dipakai untuk keputusan keamanan.

## Mengapa dianggap rusak

MD5 rentan terhadap collision: dua input berbeda dapat menghasilkan digest yang sama. Collision praktis didemonstrasikan sejak 2004 (Wang dkk.) dan terus membaik hingga chosen-prefix collision, sehingga penyerang dapat memalsukan file atau sertifikat yang lolos verifikasi MD5.

Collision yang dapat direproduksi berarti MD5 tidak lagi memberi jaminan integritas terhadap penyerang aktif, bukan sekadar kelemahan teoretis.

## Batas pemakaian

Jangan memakai MD5 untuk password, tanda tangan digital, sertifikat, atau verifikasi integritas yang menghadapi penyerang. Lihat [[References/Authentication Strategies\|Authentication Strategies]] untuk penyimpanan kredensial yang benar (password hashing seperti [[References/Bcrypt\|bcrypt]], [[References/Scrypt\|scrypt]], atau Argon2 dengan salt).

Untuk integritas umum, gunakan [[References/SHA Family\|SHA-2]] (misalnya SHA-256) atau SHA-3. MD5 masih dapat dipakai untuk checksum non-adversarial seperti deteksi korupsi acak, tetapi pemakaian itu pun sebaiknya dimigrasi karena alternatif yang aman sudah murah dan luas didukung.

## Sumber

- [RFC 1321 — The MD5 Message-Digest Algorithm](https://www.rfc-editor.org/rfc/rfc1321.html)
