---
{"dg-publish":true,"dg-path":"Oracle Database.md","permalink":"/oracle-database/","title":"Oracle Database","hideInFiletree":true,"tags":["references","database","sql"],"noteIcon":"","dg-note-properties":{"title":"Oracle Database","aliases":["Oracle RDBMS"],"categories":["Databases"],"tags":["references","database","sql"],"sources":["_raw/articles/oracle-database-research-packet.md"],"created":"2026-09-10","updated":"2026-09-10","confidence":"high"}}
---

Oracle Database adalah relational database management system dari Oracle dengan kemampuan object-relational. Produk ini menyediakan SQL, transaksi, serta pengelolaan data untuk aplikasi multiuser. Dokumentasi generasi 26 memakai nama Oracle AI Database.

Oracle Database bukan nama lain untuk Oracle Cloud atau seluruh produk perusahaan Oracle. Sebagai implementasi [[References/Relational Databases\|Relational Databases]], ia tetap membutuhkan desain schema, constraints, query, dan pengelolaan operasional yang sesuai.

## Database, instance, dan schema

Database menyimpan data secara persisten dalam file. Instance adalah struktur memori dan background processes yang mengelola akses ke file tersebut. Keduanya berkaitan, tetapi bukan objek yang sama.

Dalam arsitektur multitenant, container database (CDB) menampung pluggable databases (PDB). PDB berisi schema dan objek terkait serta tampak sebagai database tersendiri bagi aplikasi, sementara pengelolaan tertentu tetap berada pada CDB.

Schema merupakan kumpulan struktur logis milik user, seperti tabel dan indeks. Pemisahan logis ini berbeda dari susunan fisik data files, control files, dan redo logs.

SQL digunakan untuk mendefinisikan dan memanipulasi data. PL/SQL menambahkan struktur prosedural untuk menempatkan logika di database. Memakai SQL tidak berarti semua sintaks atau stored program dapat dipindahkan tanpa perubahan ke vendor lain.

## Transaksi dan read consistency

[[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan mengapa perubahan yang saling bergantung harus dikelompokkan. Dukungan transaksi tidak menggantikan keputusan aplikasi tentang batas unit kerja.

Oracle menyediakan multiversion read consistency. Undo membantu merekonstruksi data pada titik waktu yang sesuai, sehingga query tidak perlu membaca perubahan transaksi lain yang belum committed.

Pada Read Committed, query memperoleh konsistensi tingkat statement. Serializable dan read-only transactions menggunakan titik konsistensi tingkat transaksi. Dua query dalam transaksi Read Committed tidak harus melihat snapshot yang sama.

[[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] membantu membedakan jaminan tersebut. Multiversion bukan berarti tidak ada lock: penulisan pada data yang sama tetap dapat menunggu, dan deadlock harus ditangani.

## Clustering dan skalabilitas

Oracle Real Application Clusters (RAC) memungkinkan beberapa instance pada server berbeda mengakses satu database. Oracle Clusterware mengelola infrastruktur cluster, sementara Cache Fusion mengoordinasikan akses lintas instance.

RAC berbeda dari sharding: menambah instance yang mengakses satu database bukan membagi data ke sejumlah database independen. Pilihan arsitektur harus mengikuti pola akses dan kebutuhan isolasi kegagalan.

RAC membantu availability dan kapasitas pemrosesan, tetapi tidak membuktikan semua query menjadi lebih cepat. Interconnect, contention, storage, dan distribusi workload tetap perlu diukur pada sistem yang nyata.

RAC juga bukan pengganti backup atau disaster recovery. Bahkan extended cluster memiliki batas terhadap bencana regional dan corruption; dokumentasi menyarankan Oracle Data Guard bersama RAC untuk perlindungan yang lebih menyeluruh.

## Relational, spatial, dan graph

Dukungan beberapa model memungkinkan data dan operasi khusus dipakai bersama fondasi relasional, tetapi bentuk penyimpanan serta jalur eksekusinya tetap perlu dipahami.

- Relational: tabel, constraints, indeks, dan SQL menjadi fondasi pengelolaan data.
- Spatial: Oracle Spatial menyediakan representasi dan operasi lokasi/geometri; model object-relational menggunakan tipe `SDO_GEOMETRY` untuk geometri vektor.
- Property graph: entitas dimodelkan sebagai vertices dan hubungan sebagai edges, masing-masing dapat memiliki properties.

SQL property graph dapat dibentuk di atas schema database dan ditanya melalui SQL graph queries. Analitik graph khusus juga dapat memakai graph server PGX dengan pemrosesan in-memory; jangan menganggap semua operasi graph berjalan dalam engine yang sama.

Model graph yang dimuat ke PGX juga memiliki urusan sinkronisasi sendiri. Dokumentasi menyatakan perubahan graph di memori tidak otomatis ditulis kembali ke tabel relasional.

## Recovery, keamanan, dan lisensi

Recovery Manager (RMAN) adalah alat Oracle untuk pekerjaan backup dan recovery. Strategi pemulihan mencakup penjadwalan backup, pemantauan, serta pengujian respons terhadap jenis kegagalan yang berbeda.

Availability, backup, dan recovery menyelesaikan masalah yang berbeda. Sistem yang tetap melayani setelah satu instance gagal belum tentu dapat memulihkan data yang dihapus pengguna atau media yang rusak.

Untuk keamanan, tetapkan izin minimum, perlindungan data sensitif, audit, dan patch sebagai persyaratan deployment yang harus diverifikasi. Label enterprise-grade bukan bukti bahwa konfigurasi aplikasi sudah aman.

Ketersediaan teknis fitur tidak sama dengan hak penggunaan. Periksa offering, versi, deployment, options, dan management packs pada Licensing Information serta kontrak organisasi sebelum mengaktifkan fitur atau tooling.

Jangan menganggap RAC, seluruh fitur keamanan, atau semua alat diagnosis otomatis termasuk dalam lisensi dasar. Sebaliknya, jangan menyimpulkan semua fitur spatial dan graph selalu berbayar terpisah hanya dari nama fiturnya.

Oracle Database masuk akal ketika kebutuhan transaksi, multi-model, availability, dan operasional sesuai dengan kemampuan produk serta biaya tim. Klaim high-performance atau scalable harus dibuktikan melalui workload representatif.

[[References/Database Migrations\|Database Migrations]] tetap diperlukan untuk mengelola perubahan schema dan kompatibilitas aplikasi. Skalabilitas infrastruktur tidak menghilangkan risiko perubahan data atau kebutuhan uji pemulihan.

## Sumber
- [Introduction to Oracle AI Database](https://docs.oracle.com/en/database/oracle/oracle-database/26/cncpt/introduction-to-oracle-database.html)
- [Data Concurrency and Consistency](https://docs.oracle.com/en/database/oracle/oracle-database/26/cncpt/data-concurrency-and-consistency.html)
- [Introduction to Oracle RAC](https://docs.oracle.com/en/database/oracle/oracle-database/26/racad/introduction-to-oracle-rac.html)
- [Licensing Information](https://docs.oracle.com/en/database/oracle/oracle-database/26/dblic/Licensing-Information.html)
- [Introduction to Backup and Recovery](https://docs.oracle.com/en/database/oracle/oracle-database/26/bradv/introduction-backup-recovery.html)
- [Spatial Concepts](https://docs.oracle.com/en/database/oracle/oracle-database/26/spatl/spatial-concepts.html)
- [Introduction to Property Graphs](https://docs.oracle.com/en/database/oracle/property-graph/26.1/spgdg/introduction-property-graphs.html)
