---
{"dg-publish":true,"dg-path":"MySQL.md","permalink":"/my-sql/","title":"MySQL","hideInFiletree":true,"tags":["references","database","sql"],"noteIcon":"","dg-note-properties":{"title":"MySQL","categories":["Databases"],"tags":["references","database","sql"],"sources":["_raw/articles/mysql-user-summary-2026-09-09.md","_raw/articles/mysql-documentation-2026-09-09.md"],"created":"2026-09-09","updated":"2026-09-09","confidence":"medium"}}
---

MySQL adalah relational database management system (RDBMS) open-source yang dikembangkan dan dipelihara Oracle. Sebagai implementasi [[References/Relational Databases\|Relational Databases]], MySQL memakai SQL untuk mendefinisikan struktur, membaca, dan mengubah data.

## Kemampuan utama

- **SQL:** query, join, dan pengelolaan tabel untuk data terstruktur.
- **Transaksi:** mengelompokkan perubahan terkait; jaminannya perlu dilihat berdasarkan storage engine dan konfigurasi, bukan nama produk saja.
- **Indexing:** menyediakan struktur akses untuk membantu pencarian data. Keberadaan index tidak otomatis membuat semua query lebih cepat.
- **Stored procedures:** menyimpan rangkaian operasi di server database untuk dipanggil kembali.

Untuk perubahan yang harus berhasil atau gagal bersama, lihat [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]]. Untuk visibilitas perubahan konkuren, [[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] membahas default `REPEATABLE READ` pada MySQL 8.4 InnoDB dan perbedaannya dari PostgreSQL.

## Penggunaan dan ekosistem

MySQL banyak digunakan dalam aplikasi web dan merupakan komponen database dalam LAMP: Linux, Apache, MySQL, dan PHP, Perl, atau Python. Dalam [[References/Backend Development\|Backend Development]], database ini menyimpan data persisten sementara layanan aplikasi menangani kontrak API dan aturan akses.

[Portal dokumentasi MySQL](https://dev.mysql.com/doc/) mencantumkan Connector/J, Connector/Python, Connector/NET, Connector/C++, ODBC, dan integrasi PHP. Portal yang sama menyediakan dokumentasi Workbench, Shell, replikasi, InnoDB Cluster, serta administrasi dan deployment.
## Sumber
- [MySQL Documentation](https://dev.mysql.com/doc/),
