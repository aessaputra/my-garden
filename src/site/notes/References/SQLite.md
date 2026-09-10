---
{"dg-publish":true,"dg-path":"SQLite.md","permalink":"/sq-lite/","title":"SQLite","hideInFiletree":true,"tags":["references","database","sql"],"noteIcon":"","dg-note-properties":{"title":"SQLite","categories":["Databases"],"tags":["references","database","sql"],"sources":["_raw/articles/sqlite-research-packet.md"],"created":"2026-09-10","updated":"2026-09-10","confidence":"high"}}
---

SQLite adalah library database SQL yang berjalan di dalam proses aplikasi. Ia membaca dan menulis file secara langsung, tanpa proses server database terpisah. Kode sumbernya berada di public domain.

SQLite termasuk [[References/Relational Databases\|Relational Databases]], tetapi model operasinya berbeda dari [[References/MySQL\|MySQL]] atau [[References/PostgreSQL\|PostgreSQL]] yang memakai arsitektur client/server. Fokus SQLite adalah penyimpanan lokal yang sederhana dan mandiri.

## Serverless dan single file

Serverless di sini berarti aplikasi memanggil fungsi library, bukan mengirim permintaan ke server database. Istilah ini tidak berarti layanan cloud, autoscaling, atau database tanpa batas kapasitas.

Database persisten menyimpan tabel, indeks, trigger, dan view dalam satu file utama dengan format lintas platform. Namun, selama operasi dapat muncul rollback journal atau file pendamping WAL dan shared memory.

Karena itu, single file bukan izin untuk menyalin hanya file utama ketika database sedang aktif. Perubahan yang sudah committed dapat masih berada di WAL dan belum dipindahkan ke file utama.

## SQL dan integritas data

SQLite menyediakan SQL, tetapi perilakunya tidak identik dengan semua engine lain. Tabel biasa memakai flexible typing. STRICT tables, tersedia sejak SQLite 3.37.0, menawarkan pemeriksaan tipe yang lebih ketat.

SQLite tidak memiliki tipe BOOLEAN atau DATETIME terpisah seperti sebagian database lain. Representasi data dan konversinya harus konsisten di aplikasi; deklarasi tipe saja bukan bukti portabilitas schema.

Foreign key enforcement secara historis nonaktif secara default dan dapat dipengaruhi opsi kompilasi. Aplikasi sebaiknya menetapkan serta memeriksa `PRAGMA foreign_keys = ON` pada setiap koneksi, di luar transaksi aktif.

SQLite mendukung transaksi ACID. [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan cara transaksi mencegah sebagian perubahan bisnis tersimpan sementara sisanya gagal.

Namun, durability bergantung pada journal mode, pengaturan synchronous, dan perilaku penyimpanan. Pada WAL dengan `synchronous=NORMAL`, transaksi yang committed dapat hilang setelah power loss atau system crash meski konsistensi tetap terjaga.

Dalam WAL, `synchronous=FULL` menambahkan sinkronisasi pada setiap commit untuk durability. Menonaktifkan sinkronisasi bukan optimasi gratis: crash sistem atau kehilangan daya dapat merusak database.

## Concurrency dan WAL

SQLite mengizinkan banyak reader, tetapi hanya satu writer pada satu waktu untuk satu database. Banyak koneksi tidak membuat transaksi tulis berjalan paralel.

WAL memungkinkan reader dan writer berjalan bersamaan. Checkpoint memindahkan perubahan dari WAL ke file utama. Transaksi baca yang lama dapat menghambat checkpoint dan membuat WAL membesar.

WAL bukan solusi berbagi file database lintas mesin lewat network filesystem. Dokumentasi mensyaratkan proses yang menggunakan WAL berada pada host yang sama. `SQLITE_BUSY` juga tetap mungkin terjadi.

Jaga transaksi tulis singkat, tangani konflik lock secara terukur, dan ukur antrean penulisan. Jika banyak writer harus bekerja serentak atau data dipakai langsung dari banyak mesin, pertimbangkan database client/server.

## Kapan digunakan

SQLite cocok untuk penyimpanan lokal aplikasi mobile, aplikasi desktop, perangkat embedded, cache persisten, dan format dokumen aplikasi. Tidak membutuhkan administrasi server bukan berarti tidak membutuhkan backup atau migrasi schema.

Website dengan beban rendah sampai menengah juga dapat memakai SQLite. Kecocokannya lebih ditentukan oleh pola akses, durasi transaksi, dan concurrency penulisan daripada jumlah pengunjung saja.

Karena panggilan database berada dalam proses aplikasi, query kecil tidak menanggung round trip jaringan seperti client/server. [[Relationship loading should minimize total work rather than query count\|Relationship loading should minimize total work rather than query count]] menjelaskan mengapa jumlah query bukan satu-satunya ukuran biaya.

Indeks, query plan, volume data, dan I/O tetap menentukan performa. Sebutan lightweight atau high-performance tidak menjamin SQLite paling cepat untuk semua workload.

## Backup dan perubahan schema

Gunakan Online Backup API untuk membuat snapshot database aktif dengan koordinasi oleh engine. API ini dapat menyalin secara bertahap sehingga database sumber tidak perlu terkunci sepanjang seluruh proses backup.

Jangan menghapus WAL secara manual atau menganggap file utama yang disalin saat aplikasi berjalan pasti lengkap. Uji hasil restore, bukan hanya keberhasilan membuat file backup.

[[References/Database Migrations\|Database Migrations]] tetap diperlukan untuk melacak evolusi schema. Uji perubahan pada versi SQLite yang benar-benar dibundel aplikasi; versi library platform tidak harus sama dengan versi terbaru proyek.

## Sumber
- [About SQLite](https://www.sqlite.org/about.html)
- [Appropriate Uses For SQLite](https://www.sqlite.org/whentouse.html)
- [Quirks, Caveats, and Gotchas](https://www.sqlite.org/quirks.html)
- [Write-Ahead Logging](https://www.sqlite.org/wal.html)
- [SQLite Is Transactional](https://www.sqlite.org/transactional.html)
- [PRAGMA synchronous](https://www.sqlite.org/pragma.html#pragma_synchronous)
- [Online Backup API](https://www.sqlite.org/backup.html)
