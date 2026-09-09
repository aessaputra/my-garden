---
{"dg-publish":true,"dg-path":"PostgreSQL.md","permalink":"/postgre-sql/","title":"PostgreSQL","hideInFiletree":true,"tags":["references","database","postgres"],"noteIcon":"","dg-note-properties":{"title":"PostgreSQL","categories":["Databases"],"tags":["references","database","postgres"],"sources":["_raw/articles/postgresql-research-packet.md"],"created":"2026-09-09","updated":"2026-09-09"}}
---

PostgreSQL, juga disebut Postgres, adalah open-source object-relational database management system yang menggunakan dan memperluas SQL. Ia termasuk [[References/Relational Databases\|Relational Databases]], dengan dukungan tipe data, transaksi, dan extensibility yang luas. [Ikhtisar resmi PostgreSQL](https://www.postgresql.org/about/) menjelaskan fitur dan orientasi sistem ini.

## Query, tipe data, dan extensibility

PostgreSQL menyediakan query planner, beragam index, parallel read queries, dan table partitioning. Tipe datanya mencakup numeric, text, date/time, UUID, array, range, serta JSON/JSONB. Fitur tersebut mendukung pengolahan data terstruktur maupun dokumen tanpa mengharuskan semua data berbentuk string, menurut [daftar fitur resmi](https://www.postgresql.org/about/).

Extensibility berasal dari arsitektur berbasis system catalogs yang juga mendeskripsikan tipe, fungsi, dan access methods. PostgreSQL dapat memuat kode untuk tipe atau fungsi tambahan melalui dynamic loading, sebagaimana dijelaskan dalam [How Extensibility Works](https://www.postgresql.org/docs/18/extend-how.html). [[References/Ekstensi Postgres di Supabase\|Ekstensi Postgres di Supabase]] memberi konteks penerapannya pada layanan terkelola, bukan daftar kemampuan yang otomatis tersedia pada setiap instalasi PostgreSQL.

## Transaksi dan concurrency

PostgreSQL mendukung ACID serta mekanisme integritas seperti primary key, foreign key, unique, dan exclusion constraints. Ia juga menyediakan WAL, replikasi, serta point-in-time recovery dalam [fitur reliability](https://www.postgresql.org/about/). [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] menjelaskan mengapa perubahan yang saling bergantung perlu dikelompokkan sebagai transaksi.

[MVCC](https://www.postgresql.org/docs/18/mvcc-intro.html) memberikan snapshot data dan mengurangi konflik antara pembacaan biasa dan penulisan. Ini bukan janji bebas blocking: explicit locking dan penulisan yang bersaing tetap perlu diperhitungkan.

Pada [PostgreSQL 18](https://www.postgresql.org/docs/18/transaction-iso.html), default isolation adalah `READ COMMITTED`. Dua `SELECT` berturut-turut dalam transaksi yang sama dapat melihat perubahan commit yang berbeda. `READ UNCOMMITTED` berperilaku sebagai `READ COMMITTED`, sedangkan `SERIALIZABLE` dapat membatalkan transaksi sehingga aplikasi harus mengulanginya. [[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] menjelaskan konsekuensi desainnya, terpisah dari atomicity.

## Full-text search

[Full-text search bawaan](https://www.postgresql.org/docs/18/textsearch-intro.html) memproses dokumen menjadi lexemes pada `tsvector`, merepresentasikan query dengan `tsquery`, lalu mencocokkan keduanya melalui `@@`. Normalisasi, stop words, dan variasi kata bergantung pada konfigurasi parser serta dictionaries; ranking dapat mengurutkan hasil berdasarkan relevansi.

[[References/Full Text Search di Postgres\|Full Text Search di Postgres]] menyediakan contoh implementasi. Pencocokan bentuk kata melalui normalisasi tidak sama dengan menjamin semua salah ketik akan ditemukan.

## Standards dan pemilihan workload

PostgreSQL mengupayakan kesesuaian dengan SQL standard, tetapi tidak mengklaim implementasi penuh. [Pernyataan resmi](https://www.postgresql.org/about/) menyebut adanya perbedaan sintaks atau fungsi pada sebagian fitur. Karena itu, standards compliance tidak berarti semua query dapat dipindahkan antar-RDBMS tanpa penyesuaian.

Sebagai pertimbangan desain, kombinasi transaksi, integritas data, query, dan extensibility membuat PostgreSQL layak dievaluasi untuk aplikasi web maupun pelaporan analitis. Kemampuan parallel queries dan partitioning bukan bukti bahwa ia selalu menjadi pilihan terbaik untuk data warehouse. Evaluasi tetap mengikuti volume data, pola query, concurrency, dan target latency; [[References/Optimasi Query di Postgres\|Optimasi Query di Postgres]] mengarahkan pemeriksaan ke query plan dan index, bukan reputasi produk saja.
