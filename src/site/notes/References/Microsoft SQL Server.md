---
{"dg-publish":true,"dg-path":"Microsoft SQL Server.md","permalink":"/microsoft-sql-server/","title":"Microsoft SQL Server","hideInFiletree":true,"tags":["references","database","sql"],"noteIcon":"","dg-note-properties":{"title":"Microsoft SQL Server","aliases":["MS SQL","MSSQL","SQL Server"],"categories":["Databases"],"tags":["references","database","sql"],"sources":["_raw/articles/microsoft-sql-server-research-packet.md"],"created":"2026-09-10","updated":"2026-09-10","confidence":"high"}}
---

Microsoft SQL Server adalah relational database management system (RDBMS) dari Microsoft. Aplikasi terhubung ke instance atau database dan memakai Transact-SQL (T-SQL) untuk berinteraksi dengan data.

Sebagai implementasi [[References/Relational Databases\|Relational Databases]], SQL Server menyediakan penyimpanan terstruktur dan transaksi. T-SQL adalah dialek SQL dalam ekosistem Microsoft, bukan nama terpisah untuk database atau jaminan portabilitas query lintas vendor.

## Database Engine dan deployment

Database Engine merupakan layanan inti untuk menyimpan, memproses, dan mengamankan data. SQL Server dapat dipasang pada Windows, Linux, Linux container, atau virtual machine sesuai dukungan versi dan platform.

Berbagi engine tidak membuat SQL Server identik dengan Azure SQL Database atau Azure SQL Managed Instance. Model pengelolaan dan ketersediaan fiturnya berbeda; panduan deployment harus sesuai produk yang dipakai.

Dibanding [[References/SQLite\|SQLite]] yang berjalan di proses aplikasi, SQL Server menyediakan layanan database yang diakses aplikasi. Administrasi instance, kapasitas, izin, patch, dan pemulihan menjadi bagian dari operasi sistem.

## Transaksi dan concurrency

Transaksi mengelompokkan perubahan sebagai satu unit kerja. [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan alasan operasi bisnis yang saling bergantung perlu berhasil atau gagal bersama.

SQL Server memakai locking dan, pada konfigurasi tertentu, row versioning. Isolation menentukan perubahan konkuren yang dapat diamati; dukungan transaksi tidak berarti semua query bebas blocking atau deadlock.

Dengan `READ_COMMITTED_SNAPSHOT` aktif, READ COMMITTED menggunakan snapshot tingkat statement. Jika nonaktif, pembacaan memakai shared locks. Panduan mencatat default opsi ini berbeda antara SQL Server dan Azure SQL Database.

[[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] memberi konteks mengapa nama isolation dan konfigurasi aktual perlu diperiksa bersama. Jangan mengasumsikan default satu produk berlaku pada produk lain.

Fully durable transaction mempertahankan perubahan setelah commit. SQL Server juga memiliki delayed durability: commit dapat terjadi sebelum transaction log persisten. Pilihan ini menukar sebagian perlindungan kehilangan data dengan karakteristik performa.

## Integrasi, analitik, dan reporting

Kemampuan business intelligence merupakan beberapa komponen dengan tugas berbeda, bukan semuanya pekerjaan Database Engine atau satu instalasi otomatis.

| Komponen | Peran |
|---|---|
| Database Engine | Penyimpanan relasional, query, transaksi, serta akses data; dapat menjadi penyimpanan data warehouse. |
| SSIS — Integration Services | Extract, transform, load (ETL), perpindahan data, dan workflow melalui packages, tasks, serta transformations. |
| SSAS — Analysis Services | Semantic models tabular atau multidimensional untuk analitik dan aplikasi seperti Excel atau Power BI. |
| SSRS — Reporting Services | Pembuatan, deployment, dan pengelolaan paginated reports, termasuk laporan berformat tetap untuk pencetakan. |

Dalam alur warehouse, SSIS dapat memasukkan data ke penyimpanan relasional, SSAS menyediakan model analitik, lalu alat reporting menampilkan hasil. Ini contoh arsitektur, bukan rangkaian wajib untuk setiap aplikasi.

Mulai SQL Server 2025, Microsoft mengonsolidasikan reporting on-premises ke Power BI Report Server (PBIRS) dan tidak merilis versi SSRS baru. PBIRS mendukung paginated RDL serta interactive PBIX reports.

Perubahan tersebut tidak berarti instalasi SSRS lama langsung berhenti bekerja. Dukungan mengikuti lifecycle versi masing-masing; migrasi dan hak penggunaan PBIRS perlu diperiksa terhadap versi serta lisensi organisasi.

## Recovery dan keandalan

Recovery model adalah properti database yang mengatur pemeliharaan transaction log dan pilihan restore. Model Simple, Full, dan Bulk-logged tidak saling menggantikan tanpa konsekuensi operasional.

- Simple tidak mendukung transaction log backup atau point-in-time restore; pemulihan terbatas pada backup yang tersedia.
- Full membutuhkan log backup. Point-in-time recovery bergantung pada kelengkapan backup dan log hingga titik yang dituju, bukan sekadar memilih mode Full.
- Bulk-logged mengurangi logging untuk operasi bulk tertentu, tetapi memiliki batas point-in-time recovery ketika log backup mencakup operasi minimally logged.

Tetapkan toleransi kehilangan data dan waktu pemulihan, jadwalkan backup yang sesuai, lalu uji restore. Klaim reliable storage tidak membuktikan sistem dapat pulih dari kesalahan pengguna atau kehilangan media tanpa prosedur ini.

## Edition dan batas penggunaan

SQL Server 2025 memiliki Enterprise, Standard, Developer, Evaluation, dan Express dengan hak penggunaan serta kemampuan berbeda. Developer ditujukan untuk pengembangan dan pengujian, bukan production server.

Express adalah edition gratis dengan batas sumber daya dan fitur. Jangan menyalin angka batas atau asumsi ketersediaan komponen dari versi lama; periksa matriks edition yang tepat sebelum deployment.

SQL Server cocok ketika kebutuhan transaksi, integrasi, analitik, dan pengelolaan organisasi sesuai dengan fiturnya. Performa dan biaya tetap bergantung pada query, indeks, concurrency, infrastruktur, edition, serta pekerjaan administrasi.

[[References/Database Migrations\|Database Migrations]] tetap diperlukan untuk mengelola perubahan schema dan kompatibilitas aplikasi. Pemilihan produk enterprise tidak menghapus kebutuhan pengujian migrasi, izin akses, atau rencana pemulihan.

## Sumber
- [What is SQL Server](https://learn.microsoft.com/en-us/sql/sql-server/what-is-sql-server?view=sql-server-ver17)
- [SQL Server Integration Services](https://learn.microsoft.com/en-us/sql/integration-services/sql-server-integration-services?view=sql-server-ver17)
- [SQL Server Analysis Services overview](https://learn.microsoft.com/en-us/analysis-services/ssas-overview?view=sql-analysis-services-2025)
- [What is SQL Server Reporting Services](https://learn.microsoft.com/en-us/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports?view=sql-server-ver17)
- [Reporting Services consolidation FAQ](https://learn.microsoft.com/en-us/sql/reporting-services/reporting-services-consolidation-faq?view=sql-server-ver17)
- [Transaction locking and row versioning guide](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver17)
- [Recovery models](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-ver17)
- [SQL Server 2025 editions and supported features](https://learn.microsoft.com/en-us/sql/sql-server/editions-and-components-of-sql-server-2025?view=sql-server-ver17)
