---
{"dg-publish":true,"dg-path":"Database Migrations.md","permalink":"/database-migrations/","title":"Database Migrations","hideInFiletree":true,"tags":["references","database","programming"],"noteIcon":"","dg-note-properties":{"title":"Database Migrations","categories":["Databases"],"tags":["references","database","programming"],"sources":["_raw/articles/database-migrations-research-packet.md"],"created":"2026-09-09","updated":"2026-09-09"}}
---

Database migrations adalah perubahan terstruktur yang menerapkan evolusi schema database secara bertahap. Bentuknya dapat berupa SQL atau operasi deklaratif framework, bukan hanya script SQL manual. [Django](https://docs.djangoproject.com/en/5.2/topics/migrations/) memisahkan pembuatan migration files dari penerapannya ke database.

Migrasi membantu membuat perubahan terkendali dan dapat direproduksi, tetapi **tidak otomatis menjamin integritas data, kesamaan environment, atau zero downtime**. Hasil tetap bergantung pada keadaan awal, isi data, operasi, dan perilaku database yang digunakan.

## Apa yang berubah

Schema migration dapat membuat, mengubah, atau menghapus tabel, kolom, index, dan foreign key. Data migration mengubah isi atau representasi record, misalnya mengisi kolom baru dari data lama. [Flyway](https://documentation.red-gate.com/flyway/flyway-concepts/migrations/versioned-migrations) mencakup perubahan struktur, reference data, dan koreksi user data; [Django](https://docs.djangoproject.com/en/5.2/topics/migrations/) menyediakan `RunPython` untuk transformasi data yang tidak dihasilkan otomatis seperti schema migration.

[[References/Relational Databases\|Relational Databases]] menjelaskan struktur dan constraints yang diubah. [[Database constraints enforce shared invariants at the write boundary\|Database constraints enforce shared invariants at the write boundary]] menjelaskan aturan yang perlu tetap berlaku setelah perubahan, bukan jaminan yang muncul hanya karena migration file tersedia.

## Riwayat, urutan, dan pengulangan

Migration files disimpan bersama source code dan diterapkan ke environment tujuan. [Django](https://docs.djangoproject.com/en/5.2/topics/migrations/) mengurutkan migrasi berdasarkan dependencies, termasuk antar-app; nomor filename saja bukan penentu universal.

Pada [Flyway versioned migrations](https://documentation.red-gate.com/flyway/flyway-concepts/migrations/versioned-migrations), setiap migrasi memiliki version unik, description, dan checksum. Schema history mencatat yang sudah diterapkan. Sesudah mencapai permanent downstream environment, koreksi dilakukan melalui migrasi baru. [[Applied migrations preserve history when corrections move forward\|Applied migrations preserve history when corrections move forward]] menjelaskan alasan menghindari perubahan diam-diam terhadap riwayat bersama.

Repeatable process tidak berarti semua script idempotent. [Flyway repeatable migrations](https://documentation.red-gate.com/flyway/flyway-concepts/migrations/repeatable-migrations) dijalankan ulang ketika checksum berubah, setelah pending versioned migrations; penulis bertanggung jawab memastikan operasi aman diulang. Aturan ini khusus Flyway, bukan sifat semua migration tools.

## Rollout yang kompatibel

[Expand and contract](https://www.prisma.io/dataguide/types/relational/expand-and-contract-pattern) memisahkan penambahan struktur dari penghapusan struktur lama:

- Tambahkan struktur baru tanpa memutus client lama, termasuk default atau nullability yang sesuai.
- Sesuaikan penulisan agar perubahan baru tidak tertinggal selama perpindahan data; panduan mencontohkan penulisan ke kedua struktur.
- Migrasikan data lama dengan menjaga maknanya, lalu uji query dan perilaku baru.
- Alihkan pembacaan, hentikan penulisan lama setelah verifikasi, dan hapus struktur lama ketika seluruh client telah berpindah.

Urutan perubahan client dan perpindahan data perlu disesuaikan dengan desain, bukan disalin sebagai resep universal. [[Schema changes need compatibility until old clients retire\|Schema changes need compatibility until old clients retire]] menghubungkan proses ini dengan [[References/Deployment\|Deployment]]: sukses menjalankan DDL belum membuktikan semua versi aplikasi tetap bekerja.

## Transaksi, rollback, dan lock

Tiga hal berbeda perlu dipisahkan: rollback transaksi yang belum commit, migrasi balik setelah perubahan diterapkan, dan rollback artifact aplikasi. [Django 5.2](https://docs.djangoproject.com/en/5.2/topics/migrations/) menggunakan satu transaksi per migrasi secara default pada backend yang mendukung DDL transactions, seperti PostgreSQL dan SQLite; MySQL tidak memberi dukungan transaksi schema alteration yang sama. Operasi irreversible juga dapat membuat migrasi tidak dapat dibalik.

[[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] tetap berlaku dalam transaction boundary yang didukung. Ia bukan janji bahwa semua DDL atau seluruh deployment dapat dibatalkan bersama. [Prisma Data Guide](https://www.prisma.io/dataguide/types/relational/expand-and-contract-pattern) juga memperingatkan bahwa data yang hanya dapat direpresentasikan schema baru dapat hilang atau memerlukan pekerjaan tambahan ketika kembali ke schema lama.

[PostgreSQL 18 ALTER TABLE](https://www.postgresql.org/docs/18/sql-altertable.html) umumnya memperoleh `ACCESS EXCLUSIVE` lock kecuali subform menyatakan lain. Sebagian perubahan menyebabkan table rewrite. Untuk constraints yang mendukungnya, `NOT VALID` menunda pemeriksaan baris lama dan `VALIDATE CONSTRAINT` memeriksanya kemudian dengan lock lebih ringan; ini bukan penghapusan seluruh lock.

## Pemeriksaan sebelum produksi

Sebagai checklist operasional yang disintesis dari [Django](https://docs.djangoproject.com/en/5.2/topics/migrations/), [Prisma migration guide](https://www.prisma.io/docs/guides/database/data-migration), dan [expand-contract](https://www.prisma.io/dataguide/types/relational/expand-and-contract-pattern):

- Review operasi yang dihasilkan dan dependencies sebelum penerapan.
- Uji pada salinan data produksi yang dilindungi sesuai sensitivitasnya, bukan hanya database kosong.
- Periksa hasil transformasi, kompatibilitas client lama dan baru, serta dampak lock dan durasi.
- Siapkan backup dan jalur pemulihan; tentukan apakah kegagalan memerlukan rollback, koreksi forward, atau restore.
- Pantau error dan perilaku aplikasi setelah deployment sebelum menghapus struktur lama.
