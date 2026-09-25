---
{"dg-publish":true,"dg-path":"Bcrypt.md","permalink":"/bcrypt/","title":"Bcrypt","hideInFiletree":true,"tags":["references","security","algorithms"],"noteIcon":"","dg-note-properties":{"title":"Bcrypt","aliases":["bcrypt"],"categories":["Cryptography"],"type":"reference","tags":["references","security","algorithms"],"sources":["_raw/articles/bcrypt-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

bcrypt adalah fungsi password-hashing yang dirancang untuk menyimpan kredensial user di database backend. Berbeda dengan hash general-purpose seperti [[References/SHA Family\|SHA Family]] atau [[References/MD5\|MD5]] yang dioptimalkan untuk kecepatan, bcrypt sengaja lambat dan boros sumber daya — penalti performa ini adalah fitur keamanan yang menetralkan brute force dan cracking berbasis hardware.

Dalam peta [[References/Web Security Knowledge\|Web Security Knowledge]], bcrypt adalah jawaban yang benar untuk penyimpanan password, menggantikan pemakaian hash cepat atau rusak.

## Cara kerja

bcrypt berbasis Blowfish block cipher dengan dua mekanisme utama:

- **Integrated salting**: salt acak yang aman secara kriptografis digabungkan dengan password sebelum hashing. Tanpa salt, password identik menghasilkan hash identik sehingga penyerang bisa memakai rainbow table untuk memecahkan jutaan hash sekaligus. Salt dan hasil akhir disimpan bersama dalam string output.
- **Adaptive work factor (cost factor)**: jumlah iterasi sebesar 2^cost. Saat verifikasi login, server harus menjalankan ulang seluruh algoritme lambat dengan salt dan cost yang tersimpan. Cost dapat dinaikkan seiring membaiknya hardware — biasanya dituning sekitar 100–300 ms per hash. Keterlambatan ini tidak terasa bagi satu user, tetapi membuat brute force skala besar tidak viable secara ekonomi dan komputasi.

Format output bcrypt memuat semua metadata verifikasi, misalnya `$2b$12$...`: `$2b$` adalah varian algoritme, `12` adalah cost (2^12 = 4.096 iterasi), 22 karakter berikut adalah salt base64, sisanya adalah hash. Backend tidak perlu kolom terpisah untuk salt atau cost.

## Batas pemakaian

Hashing adalah transformasi satu arah — tidak mungkin mendekripsi hash bcrypt kembali ke plaintext. Verifikasi hanya membandingkan hasil hashing input baru dengan parameter yang sama.

Cost factor harus di-benchmark pada hardware backend yang sebenarnya dan ditinjau berkala. Cost terlalu rendah melemahkan proteksi terhadap GPU modern; terlalu tinggi membuka risiko denial-of-service saat lonjakan login konkuren menghabiskan CPU.

Dibanding [[References/Scrypt\|Scrypt]] yang memory-hard, bcrypt lebih menuntut CPU daripada memori. Keduanya (dan Argon2) adalah pilihan yang sah dengan salt acak per password. Lihat [[References/Authentication Strategies\|Authentication Strategies]] untuk pemilihan strategi dan [[References/Password Security di Supabase\|Password Security di Supabase]] untuk contoh operasional bcrypt di `auth.users.encrypted_password`.

## Sumber

- [Provos & Mazières — A Future-Adaptable Password Scheme (USENIX 1999)](https://www.usenix.org/conference/1999-usenix-annual-technical-conference/future-adaptable-password-scheme)
