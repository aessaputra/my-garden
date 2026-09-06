---
{"dg-publish":true,"dg-path":"Backend Development.md","permalink":"/backend-development/","title":"Backend Development","hideInFiletree":true,"tags":["references","backend","programming","security"],"noteIcon":"","dg-note-properties":{"title":"Backend Development","categories":["Backend Systems"],"tags":["references","backend","programming","security"],"sources":["_raw/articles/backend-development-evidence-addendum-2026-09-07.md"],"created":"2026-09-07","updated":"2026-09-07","confidence":"high"}}
---

Backend development adalah pengembangan logika server yang menerima request, memproses aturan aplikasi, mengakses data, dan menghasilkan response untuk client.

[MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction) mencakup validasi request, penyimpanan data, dan pemilihan konten response sebagai pekerjaan server-side.

## Alur dan pembagian tanggung jawab

Client mengirim request HTTP dengan resource, method, dan data yang diperlukan. Server memprosesnya lalu mengembalikan status serta response body. Output dapat berupa HTML, JSON, atau bentuk konten lain menurut MDN.

Backend bukan sekadar penghubung frontend dengan database. MDN juga mencakup hasil software tools dan komunikasi dengan layanan lain sebagai sumber response.

Framework membantu fungsi umum seperti routing, sessions, authentication, database access, dan templating. Pemilihan framework tidak menggantikan penetapan aturan aplikasi.

## API sebagai kontrak

[Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design) menyarankan REST API berorientasi resource dan tidak menyalin struktur tabel database secara langsung.

Kontrak mencakup operasi, representasi data, status, dan perilaku error. Loose coupling berarti client tidak perlu mengetahui implementasi internal server. REST merupakan salah satu pendekatan, bukan definisi seluruh backend.

[[References/HTTP\|HTTP]] membahas protokolnya, sedangkan [[References/GraphQL\|GraphQL]] membahas pendekatan API berbasis schema. Browser APIs pada [[References/Web APIs\|Web APIs]] bukan sinonim endpoint backend.

## Data dan transaksi

[PostgreSQL](https://www.postgresql.org/docs/current/tutorial-transactions.html) menjelaskan transaksi sebagai penggabungan beberapa langkah menjadi operasi all-or-nothing.

Untuk perubahan yang saling bergantung, tentukan batas transaksi sebelum menulis operasi penyimpanan. Ini rekomendasi desain dari sifat transaksi, bukan jaminan atomisitas lintas database dan layanan eksternal.

## Authentication, authorization, dan sessions

Authentication menetapkan identitas terautentikasi; authorization menentukan tindakan dan resource yang diizinkan. [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) meminta pengecekan izin pada setiap request.

Login berhasil tidak otomatis memberi akses ke semua resource. Kebijakan deny-by-default, least privilege, dan pengujian akses terlarang perlu diterapkan sesuai panduan OWASP.

[Session ID](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html) membawa authority autentikasi selama session aktif. Transport, expiry, invalidation, dan perlindungan token merupakan bagian desain keamanan.

Rincian pilihan login berada di [[References/Authentication Strategies\|Authentication Strategies]]. [[References/Deployment\|Deployment]] memberi konteks lifecycle operasional di luar penanganan request.

## Batas riset

Lima dokumen lengkap diambil melalui 9Router. Paket sebelumnya menyebut fetch terpotong sebagai snapshot lengkap; koreksinya berada di [[_raw/articles/backend-development-evidence-addendum-2026-09-07\|addendum bukti]].

Tidak ada benchmark, implementasi aplikasi, atau audit keamanan. Panduan ini tidak menentukan bahasa terbaik, kewajiban microservices, maupun jaminan komunikasi tanpa kegagalan.

## Lanjutan

- [[Backend APIs should expose business contracts rather than database tables\|Backend APIs should expose business contracts rather than database tables]]
- [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]]
- [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]]
- [[Session tokens carry authentication authority beyond the login request\|Session tokens carry authentication authority beyond the login request]]
