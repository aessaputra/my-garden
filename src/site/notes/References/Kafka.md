---
{"dg-publish":true,"dg-path":"Kafka.md","permalink":"/kafka/","title":"Kafka","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Kafka","aliases":["Apache Kafka"],"categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/kafka-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

Kafka (Apache Kafka) adalah platform event streaming terdistribusi untuk pemrosesan data throughput tinggi yang fault-tolerant. Berbeda dengan message broker tradisional yang menghapus pesan setelah dikirim, Kafka menyimpan event secara persisten dalam log sehingga consumer dapat membaca ulang data berkali-kali — cocok untuk real-time analytics dan integrasi data antar layanan. Untuk delivery tugas sekali-pakai dengan acknowledgment, lihat [[References/RabbitMQ\|RabbitMQ]]. Lihat [[References/Message Brokers\|Message Brokers]] untuk perbandingan umum antar broker.

Pola komunikasi Kafka adalah publish/subscribe yang asinkron: producer menulis event ke topic, consumer membaca dari topic yang dilanggannya. Ini memutus tight coupling antar layanan — pengirim dan penerima tidak perlu bertemu langsung, sehingga satu layanan yang lambat atau down tidak memblokir yang lain.

## Topics, partitions, replication

- **Topics**: kanal pengelompokan event berdasarkan jenisnya (mis. pesanan, pembayaran, inventori).
- **Partitions**: topic dibagi menjadi partisi agar dapat diproses paralel oleh banyak consumer; partisi adalah unit skalabilitas Kafka.
- **Replication**: setiap partisi direplikasi ke beberapa broker sehingga kegagalan satu broker tidak menghilangkan data maupun ketersediaan.

Berbeda dengan Pub/Sub [[References/Redis\|Redis]] yang tidak menyimpan riwayat untuk replay, Kafka mempertahankan log event — consumer group dapat memutar ulang dari offset lama untuk reprocessing atau analitik historis.

## Batas operasional

Throughput dan latency aktual bergantung pada ukuran pesan, jumlah partisi, faktor replikasi, jaringan, dan beban bersamaan — bukan klaim generik. Partisi yang terlalu sedikit membatasi paralelisme; replikasi menambah durability dengan biaya write latency dan storage. Rencanakan retensi log, monitoring consumer lag, dan keamanan akses broker (ACL, TLS, autentikasi) sebelum membuka cluster ke jaringan yang lebih luas.

## Sumber

- [Apache Kafka documentation](https://kafka.apache.org/documentation/)
