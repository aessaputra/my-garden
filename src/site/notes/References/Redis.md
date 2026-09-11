---
{"dg-publish":true,"dg-path":"Redis.md","permalink":"/redis/","title":"Redis","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Redis","categories":["Databases"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/redis-user-summary-2026-09-11.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Redis adalah data structure store yang terutama mengolah data di memori. Redis mendukung strings, lists, sets, hashes, dan sorted sets, sehingga aplikasi dapat memilih struktur sesuai operasi yang dibutuhkan, bukan hanya menyimpan blob data.

## Penggunaan umum

- **Caching**: menyimpan data atau hasil komputasi yang dapat digunakan kembali; [[References/Caching\|Caching]] menjelaskan freshness dan invalidation.
- **Session management**: menyimpan state session dengan masa berlaku. [[References/Cookie-Based Authentication\|Cookie-Based Authentication]] menjelaskan bagaimana browser dapat membawa ID untuk mencari session tersebut.
- **Real-time analytics**: counters, agregasi, atau leaderboard, misalnya menggunakan sorted sets.
- **Messaging**: mendukung pola pertukaran pesan, tetapi jaminan delivery bergantung pada mekanisme yang dipilih.

Pub/Sub mengirim pesan kepada subscriber yang sedang terhubung tanpa menyimpan riwayat untuk replay. Redis Streams menyediakan log pesan dan consumer groups, tetapi durability serta pemrosesan ulang tetap memerlukan konfigurasi dan penanganan aplikasi. Keduanya tidak boleh dianggap memiliki jaminan delivery yang sama.

## Persistence dan distribusi

Redis menawarkan snapshot RDB dan append-only file (AOF) untuk persistence. Data yang bertahan setelah kegagalan bergantung pada konfigurasi penyimpanan dan sinkronisasi; penggunaan memori tidak berarti selalu tanpa persistence, dan mengaktifkan persistence bukan jaminan tanpa kehilangan data.

Replication menyalin data ke replica, sedangkan Redis Cluster membagi key ke beberapa node dan mendukung failover. Replication umumnya asynchronous, sehingga write yang sudah diakui masih dapat hilang dalam skenario kegagalan tertentu. Replica bukan pengganti backup yang dapat dipulihkan.

## Batas operasional

Akses memori dapat memberi latency rendah, tetapi throughput dan latency aktual bergantung pada ukuran data, kompleksitas command, jaringan, persistence, serta beban bersamaan. Ukur workload sendiri sebelum menganggap Redis menyelesaikan bottleneck.

Rencanakan kapasitas memori, TTL, dan eviction. Cache yang boleh dibuang memiliki kebutuhan berbeda dari session atau data yang tidak boleh hilang. Batasi akses jaringan, gunakan ACL dan TLS sesuai deployment, serta hindari membuka instance langsung ke Internet.

Lisensi Redis berubah antarversi. Jangan memperlakukan label "open-source" sebagai keterangan universal untuk semua versi atau produk Redis; periksa lisensi distribusi yang digunakan sebelum adopsi atau redistribusi.

## Sumber

Ringkasan ini berasal dari materi pengguna, dengan penjelasan teknis tambahan. Dokumentasi berikut menjadi rujukan pemeriksaan implementasi, bukan snapshot yang diverifikasi pada ingest ini:

- [Redis documentation](https://redis.io/docs/latest/): tipe data dan pengoperasian Redis.
- [Redis persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/): RDB dan AOF.
- [Redis licensing](https://redis.io/legal/licenses/): ketentuan lisensi menurut versi dan produk.
