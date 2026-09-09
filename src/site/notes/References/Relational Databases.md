---
{"dg-publish":true,"dg-path":"Relational Databases.md","permalink":"/relational-databases/","title":"Relational Databases","hideInFiletree":true,"tags":["references","database","programming"],"noteIcon":"","dg-note-properties":{"title":"Relational Databases","categories":["Databases"],"tags":["references","database","programming"],"sources":["_raw/articles/relational-databases-research-packet.md"],"created":"2026-09-09","updated":"2026-09-09"}}
---

Relational database adalah database yang merepresentasikan data sebagai tabel berisi baris dan kolom. RDBMS adalah perangkat lunak untuk mengelolanya. Dalam [definisi PostgreSQL](https://www.postgresql.org/docs/18/tutorial-concepts.html), *relation* merupakan istilah matematis untuk tabel, bukan sekadar hubungan antartabel.

SQL digunakan untuk mendefinisikan, membaca, dan mengubah data pada sistem relasional. [[References/PostgreSQL\|PostgreSQL]], MySQL, dan Oracle Database merupakan contoh produk yang dibahas di sini; model relasional tidak menyamakan fitur maupun konfigurasi ketiganya.

## Struktur dan hubungan

Kolom mempunyai tipe data, sedangkan baris menyimpan record. `PRIMARY KEY` mengidentifikasi baris secara unik dan tidak boleh null; kunci dapat terdiri atas beberapa kolom. `FOREIGN KEY` menjaga agar nilai referensi yang diwajibkan cocok dengan baris yang dirujuk. Rincian ini dijelaskan dalam [dokumentasi constraints PostgreSQL](https://www.postgresql.org/docs/18/ddl-constraints.html).

`JOIN` menggabungkan baris berdasarkan kondisi yang ditulis dalam query. Foreign key menjaga integritas, bukan prasyarat sintaks join. [Tutorial joins](https://www.postgresql.org/docs/18/tutorial-join.html) menunjukkan pemilihan pasangan baris melalui ekspresi `ON`.

Contoh hipotetis: tabel pelanggan menyimpan identitas pelanggan, tabel pesanan menyimpan referensi pelanggan, dan tabel item pesanan menghubungkan pesanan dengan produk. Struktur ini memungkinkan detail pelanggan dan item diperoleh bersama tanpa menyalin semua atribut pelanggan ke setiap baris item.

## Integritas bukan sekadar tipe data

`NOT NULL`, `UNIQUE`, `CHECK`, dan `FOREIGN KEY` menyatakan aturan yang diperiksa database. Pada PostgreSQL, `CHECK (quantity > 0)` tidak menolak null; tambahkan `NOT NULL` jika nilainya wajib. Row-level `CHECK` juga bukan mekanisme umum untuk memeriksa data di baris lain, menurut [batas constraints](https://www.postgresql.org/docs/18/ddl-constraints.html).

Implikasi desainnya dirangkum dalam [[Database constraints enforce shared invariants at the write boundary\|Database constraints enforce shared invariants at the write boundary]]: aturan yang dapat dideklarasikan di database tidak perlu bergantung pada satu jalur validasi aplikasi saja.

## Transaksi dan ACID

ACID mencakup atomicity, consistency, isolation, dan durability. Secara ringkas: perubahan transaksi diperlakukan sebagai satu unit, keadaan data mengikuti aturan yang ditetapkan, interaksi transaksi konkuren dibatasi, dan hasil commit dipertahankan. [Oracle menjelaskan kerangka ACID](https://www.oracle.com/database/what-is-a-relational-database/), sementara [tutorial transaksi PostgreSQL](https://www.postgresql.org/docs/18/tutorial-transactions.html) memperlihatkan `BEGIN`, `COMMIT`, dan `ROLLBACK`.

Untuk perubahan yang saling bergantung, [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan batas all-or-nothing. Transaksi database lokal bukan jaminan atomisitas pembayaran di layanan eksternal.

Consistency dalam ACID tidak boleh dibaca sebagai janji bahwa semua pembaca dan semua replica selalu melihat nilai paling baru. [[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] memisahkan aturan visibilitas dari atomicity.

| Implementasi | Default isolation yang didokumentasikan | Implikasi |
|---|---|---|
| [PostgreSQL 18](https://www.postgresql.org/docs/18/transaction-iso.html) | `READ COMMITTED` | Dua query dalam satu transaksi dapat melihat snapshot berbeda. |
| [MySQL 8.4 InnoDB](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html) | `REPEATABLE READ` | Ordinary consistent reads menggunakan snapshot dari pembacaan pertama; locking reads mempunyai perilaku berbeda. |

PostgreSQL mendukung replikasi asynchronous. Pada file-based log shipping, WAL dikirim setelah commit; replica dapat tertinggal dan failover dapat kehilangan perubahan yang belum terkirim. [Dokumentasi standby](https://www.postgresql.org/docs/18/warm-standby.html) membatasi klaim umum pada halaman Oracle bahwa salinan database selalu sama. Jaminan pembacaan perlu ditentukan dari isolation, konfigurasi replikasi, dan tujuan koneksi.

## Query dan performa

Sistem relasional mendukung query yang menggabungkan data lintas tabel, tetapi kemampuan tersebut tidak menjamin query selalu efisien. [PostgreSQL](https://www.postgresql.org/docs/18/indexes-intro.html) memilih index hanya ketika dianggap lebih efisien daripada sequential scan; pemeliharaan index menambah overhead operasi perubahan data.

[[References/Index di Postgres\|Index di Postgres]] memberi panduan index dan pemeriksaan rencana query. Keputusan optimasi tetap perlu melihat workload nyata, bukan hanya memilih produk berlabel relational.

## Kapan digunakan dan batas riset

Inventaris, pesanan, dan pencatatan pelanggan merupakan penggunaan yang dicontohkan dalam [ikhtisar Oracle](https://www.oracle.com/database/what-is-a-relational-database/). Sebagai rekomendasi desain, database relasional layak dipertimbangkan ketika data memiliki struktur yang jelas, referensi perlu dijaga, dan perubahan terkait membutuhkan transaksi. [[References/Backend Development\|Backend Development]] menempatkan penyimpanan ini dalam alur layanan, bukan sebagai keseluruhan arsitektur backend.