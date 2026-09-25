---
{"dg-publish":true,"dg-path":"RabbitMQ.md","permalink":"/rabbit-mq/","title":"RabbitMQ","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"RabbitMQ","aliases":[],"categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/rabbitmq-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

RabbitMQ adalah message broker open-source berbasis AMQP untuk komunikasi asinkron antar sistem terdistribusi. Ia menyediakan queuing, routing, durability, dan acknowledgment — pesan tidak dianggap selesai sampai consumer mengonfirmasi, sehingga kegagalan consumer tidak menghilangkan pekerjaan.

Berbeda dengan [[References/Kafka\|Kafka]] yang menyimpan event dalam log persisten untuk replay dan analitik, RabbitMQ berorientasi pada delivery tugas: pesan di-routing lewat exchange ke queue, dikonsumsi sekali, lalu dihapus setelah acknowledgment. Lihat [[References/Message Brokers\|Message Brokers]] untuk perbandingan umum. Pilih RabbitMQ untuk work queue dan pola request/reply; pilih Kafka untuk stream event yang perlu dibaca ulang.

## Pola messaging

- **Point-to-point**: producer → queue → satu consumer; cocok untuk distribusi tugas.
- **Pub/sub**: exchange fanout mengirim salinan ke banyak queue yang terikat.
- **Request/reply**: reply queue + correlation ID memasangkan respons dengan permintaan.

## Batas operasional

Throughput aktual bergantung pada ukuran pesan, durability (persistent vs transient), jumlah queue/consumer, jaringan, dan beban bersamaan. Pesan persistent + acknowledgment menambah jaminan dengan biaya latency dan disk I/O. Rencanakan monitoring panjang queue dan consumer yang macet, serta keamanan akses broker (TLS, autentikasi, vhost) sebelum membuka ke jaringan yang lebih luas.

## Sumber

- [RabbitMQ documentation](https://www.rabbitmq.com/docs)
