---
{"dg-publish":true,"dg-path":"SHA Family.md","permalink":"/sha-family/","title":"SHA Family","hideInFiletree":true,"tags":["references","security","algorithms"],"noteIcon":"","dg-note-properties":{"title":"SHA Family","aliases":["Secure Hash Algorithm","SHA-1","SHA-2","SHA-3"],"categories":["Cryptography"],"type":"reference","tags":["references","security","algorithms"],"sources":["_raw/articles/sha-family-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

SHA (Secure Hash Algorithm) adalah keluarga fungsi hash kriptografis yang menghasilkan digest berukuran tetap. Keluarga ini mencakup SHA-1, SHA-2, dan SHA-3 dengan tingkat keamanan berbeda.

Dalam peta [[References/Web Security Knowledge\|Web Security Knowledge]], pemilihan anggota keluarga SHA yang tepat adalah bagian dari menghindari cryptographic failure (A04 OWASP).

## Anggota keluarga

- **SHA-1**: digest 160-bit. Collision praktis didemonstrasikan sejak 2017 (SHAttered), sehingga tidak aman untuk signature dan integritas yang menghadapi penyerang. Masih muncul sebagai fingerprint lama (misalnya SHA-1 certificate fingerprint pada setup Android), tetapi bukan untuk keputusan keamanan baru.
- **SHA-2**: digest 224–512 bit (SHA-224, SHA-256, SHA-384, SHA-512). Aman untuk integritas umum dan signature per 2026. SHA-256 adalah default yang paling umum.
- **SHA-3**: desain berbeda berbasis Keccak, digest 224–512 bit. Alternatif modern bila diperlukan diversifikasi algoritme dari SHA-2.

Berbeda dengan [[References/MD5\|MD5]] yang sepenuhnya rusak, SHA-1 hanya lemah (hindari untuk hal baru) sedangkan SHA-2 dan SHA-3 masih aman.

## Batas pemakaian

Fungsi hash cepat seperti SHA bukan alat penyimpanan password. Password membutuhkan fungsi yang sengaja lambat dan boros memori ([[References/Scrypt\|scrypt]], [[References/Bcrypt\|bcrypt]], atau Argon2 dengan salt) agar brute force mahal. Lihat [[References/Authentication Strategies\|Authentication Strategies]] untuk penyimpanan kredensial yang benar.

Untuk signature digital, keamanan bergantung pada keseluruhan rantai: algoritme hash yang aman, kunci yang dikelola benar, dan verifikasi sertifikat. SHA-2/SHA-3 yang aman tidak memperbaiki kunci bocor atau validasi yang dilewati.

## Sumber

- [NIST FIPS 180-4 — Secure Hash Standard](https://csrc.nist.gov/pubs/fips/180-4/final)
- [NIST FIPS 202 — SHA-3 Standard](https://csrc.nist.gov/pubs/fips/202/final)
