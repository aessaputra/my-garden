---
{"dg-publish":true,"dg-path":"MariaDB.md","permalink":"/maria-db/","title":"MariaDB","hideInFiletree":true,"tags":["references","database","sql"],"noteIcon":"","dg-note-properties":{"title":"MariaDB","categories":["Databases"],"tags":["references","database","sql"],"sources":["_raw/articles/mariadb-research-packet.md"],"created":"2026-09-10","updated":"2026-09-10","confidence":"high"}}
---

MariaDB adalah relational database server open-source yang berasal dari fork [[References/MySQL\|MySQL]] dan dikembangkan oleh pengembang asli MySQL bersama komunitas. MariaDB memakai SQL untuk mengelola data terstruktur.

Sebagai implementasi [[References/Relational Databases\|Relational Databases]], MariaDB menyediakan tabel, indeks, query, dan transaksi. Ia bukan sekadar nama distribusi MySQL: kedua proyek telah mengembangkan fitur dan perilaku yang berbeda.

## Pengembangan dan lisensi

MariaDB Foundation adalah organisasi nirlaba yang menjaga keterbukaan dan kontinuitas proyek. Foundation membantu kontribusi, dokumentasi, kualitas kode, dan distribusi; pengembangan tidak terbatas pada stafnya.

MariaDB Corporation merupakan kontributor kode utama sekaligus penyedia produk dan layanan komersial. Foundation dan perusahaan bukan organisasi yang sama.

MariaDB Server berlisensi GPLv2. “Lisensi lebih baik” bukan sifat teknis universal: kesesuaiannya bergantung pada kebutuhan penggunaan dan distribusi. Lisensi server juga tidak otomatis mewakili semua produk bernama MariaDB.

## Model dan kemampuan

Storage engine menentukan cara data tabel disimpan dan sebagian jaminan operasinya. InnoDB mendukung transaksi ACID, foreign keys, indeks, MVCC, dan row-level locking; sifat ini tidak boleh diasumsikan untuk semua engine.

[[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan alasan perubahan terkait perlu diselesaikan sebagai satu unit. Engine transaksional tetap memerlukan pemilihan batas transaksi yang tepat oleh aplikasi.

MariaDB juga menyediakan fitur di luar SQL dasar. Dokumentasi MariaDB 11.8 mencakup sequences, system-versioned tables untuk riwayat data, serta application-time periods. Ketersediaan fitur tetap harus diperiksa pada versi deployment.

## Batas pengganti MySQL

“Drop-in replacement” menggambarkan tujuan awal dan kompatibilitas beberapa versi historis. Dokumentasi membedakan pasangan versi dan mencatat bahwa perbedaan implementasi bertambah seiring perkembangan proyek.

Kompatibilitas protokol client membantu penggunaan ekosistem MySQL, tetapi koneksi yang berhasil belum membuktikan seluruh query, autentikasi, dan data aplikasi kompatibel.

Contoh berikut berasal dari perbandingan MariaDB 11.8.3 dengan MySQL 8.0.43; bukan penilaian MySQL 8.4 atau semua versi mendatang:

- JSON pada MariaDB merupakan alias LONGTEXT, berbeda dari format JSON biner MySQL. Penyimpanan dan semantik perbandingan perlu diperiksa saat migrasi.
- GTID MariaDB tidak kompatibel dengan GTID MySQL. Topologi replikasi membutuhkan perencanaan tersendiri.
- Dukungan autentikasi, character set, collation, fungsi, dan system variables memiliki perbedaan.
- Fitur khusus salah satu server dapat membuat perpindahan kembali lebih sulit.

Karena itu, jangan mengganti binary atau memakai ulang data directory produksi hanya berdasarkan label kompatibel. Gunakan panduan untuk versi asal dan tujuan serta uji salinan data lebih dahulu.

## Operasi dan pemulihan

Untuk koneksi jaringan, periksa TLS serta verifikasi identitas server pada client. Dokumentasi membedakan perilaku sebelum MariaDB 11.4; dukungan TLS pada binary bukan bukti bahwa setiap koneksi terenkripsi dan terverifikasi.

Backup fisik dengan mariadb-backup harus melalui tahap prepare agar salinan file konsisten sebelum restore. Versi tool harus kompatibel dengan server sumber.

Restore fisik membutuhkan server tujuan berhenti, data directory kosong, serta ownership yang sesuai. Opsi copy-back mempertahankan backup asli, sedangkan move-back memindahkannya. Jangan mengosongkan data produksi tanpa rencana pemulihan.

Checklist migrasi yang disarankan:

1. Inventarisasi versi server, engine, connector, akun, collation, dan fitur khusus.
2. Uji query penting, hasil JSON, transaksi, dan perilaku aplikasi pada staging.
3. Ukur latensi serta penggunaan sumber daya dengan data dan beban yang representatif.
4. Uji backup dan restore; tetapkan cara menangani penulisan baru bila cutover harus dibatalkan.

[[References/Database Migrations\|Database Migrations]] membahas kompatibilitas schema dan aplikasi selama perubahan. Memindahkan server tidak menggantikan pengelolaan riwayat perubahan tersebut.

MariaDB masuk akal ketika fitur dan ekosistemnya cocok dengan aplikasi serta kemampuan operasional tim. Klaim lebih cepat, lebih stabil, atau lebih murah daripada MySQL perlu bukti workload dan biaya nyata, bukan daftar fitur saja.

## Sumber

- [About MariaDB Server](https://mariadb.org/about/)
- [MariaDB versus MySQL — Compatibility](https://mariadb.com/docs/release-notes/community-server/about/compatibility-and-differences/mariadb-vs-mysql-compatibility.md)
- [MariaDB 11.8 versus MySQL 8.0](https://mariadb.com/docs/release-notes/community-server/about/compatibility-and-differences/incompatibilities-and-feature-differences-between-mariadb-11-8-and-mysql-8.md)
- [InnoDB](https://mariadb.com/docs/server/storage-engines/innodb)
- [Secure Connections Overview](https://mariadb.com/docs/server/security/encryption/data-in-transit-encryption/secure-connections-overview)
- [Full Backup and Restore](https://mariadb.com/docs/server/server-usage/backup-and-restore/mariadb-backup/full-backup-and-restore-with-mariadb-backup.md)
