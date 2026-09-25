---
{"dg-publish":true,"dg-path":"Message Brokers.md","permalink":"/message-brokers/","title":"Message Brokers","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Message Brokers","aliases":["message broker"],"categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/message-brokers-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

Message broker memfasilitasi komunikasi antar sistem terdistribusi dengan me-routing dan mengantarkan pesan. Ia memungkinkan asynchronous messaging: producer dan consumer ter-decouple — pengirim tidak perlu menunggu penerima, sehingga satu layanan yang lambat atau down tidak memblokir yang lain.

Fitur umum broker: queuing (antrean pesan), load balancing antar consumer, persistence (pesan bertahan saat kegagalan), dan acknowledgment (pesan dianggap selesai hanya setelah consumer mengonfirmasi).

## Memilih implementasi

- [[References/Kafka\|Kafka]]: log event persisten untuk replay dan real-time analytics; cocok untuk stream data yang perlu dibaca ulang.
- [[References/RabbitMQ\|RabbitMQ]]: delivery tugas sekali-pakai via exchange/queue dengan acknowledgment; cocok untuk work queue dan pola request/reply.
- ActiveMQ: broker JMS/AMQP serbaguna untuk enterprise messaging — belum ada halaman khusus di vault ini.

"High-throughput" bukan sifat otomatis broker mana pun: throughput dan latency aktual bergantung pada ukuran pesan, durability, jumlah queue/partisi/consumer, jaringan, dan beban bersamaan. Ukur workload sendiri sebelum memilih.

## Sumber

- [Apache Kafka documentation](https://kafka.apache.org/documentation/)
- [RabbitMQ documentation](https://www.rabbitmq.com/docs)
