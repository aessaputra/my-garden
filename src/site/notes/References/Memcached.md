---
{"dg-publish":true,"dg-path":"Memcached.md","permalink":"/memcached/","title":"Memcached","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Memcached","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/memcached-user-summary-2026-09-11.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Memcached adalah sistem cache key-value yang terutama menyimpan data di RAM untuk mengurangi pengambilan atau komputasi berulang. Aplikasi dapat menyimpan hasil query, potongan halaman, atau objek yang sudah diserialisasi sebagai nilai cache.

## Distribusi dan alur akses

Pada arsitektur umumnya, client memilih server berdasarkan key, misalnya melalui hashing. Beberapa server menyediakan kapasitas cache gabungan, tetapi masing-masing server tidak otomatis mengetahui isi server lain atau mereplikasi data antarnode. Istilah distributed hash table di sini tidak berarti cluster dengan koordinasi dan replication bawaan.

Dalam pola cache-aside, aplikasi memeriksa Memcached terlebih dahulu. Jika terjadi cache miss, aplikasi mengambil data dari database atau sumber lain, kemudian dapat menyimpan hasilnya ke cache. Memcached tidak otomatis menjalankan query ke backing store. [[References/Caching\|Caching]] menjelaskan hit, miss, cache key, dan freshness.

## Expiry dan eviction

Item dapat diberi expiration. Saat ruang dibutuhkan, Memcached memakai mekanisme eviction berbasis LRU; implementasi modern tidak harus berupa satu antrean LRU global yang persis mengikuti urutan seluruh akses.

Expiration dan eviction berbeda: item dapat dikeluarkan karena tekanan kapasitas sebelum masa berlakunya habis. Aplikasi harus tetap benar ketika item tidak tersedia dan menentukan kapan data lama perlu dibatalkan atau diperbarui.

## Penggunaan dan batas

Memcached cocok untuk cache bersama yang datanya dapat diperoleh kembali. Jangan mengandalkannya sebagai satu-satunya salinan data penting; kehilangan node atau entri cache harus memiliki jalur pemulihan dari sumber utama.

Dibanding [[References/Redis\|Redis]], Memcached berfokus pada key-value caching sederhana, bukan operasi atas banyak tipe struktur data. Pemilihan keduanya bergantung pada kebutuhan operasi, kapasitas, dan pengelolaan, bukan asumsi bahwa salah satunya selalu lebih cepat.

Ukur hit rate, latency, eviction, serta beban backing store. Cache miss serentak dapat membebani sumber, sehingga aplikasi mungkin membutuhkan pembatasan request atau penggabungan pengambilan data untuk key yang sama.

Batasi akses jaringan ke client tepercaya dan jangan membuka Memcached langsung ke Internet. Key serta isi cache tetap perlu mengikuti batas akses pengguna dan tenant.

## Sumber

- [Memcached documentation](https://docs.memcached.org/): rujukan arsitektur dan pengoperasian untuk pemeriksaan lanjutan.
- [[References/Caching\|Caching]]: konsep cache dan kebijakan penggunaan ulang data.
