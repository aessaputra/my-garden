---
{"dg-publish":true,"dg-path":"N plus one problem.md","permalink":"/n-plus-one-problem/","title":"N plus one problem","hideInFiletree":true,"tags":["references","database","programming"],"noteIcon":"","dg-note-properties":{"title":"N plus one problem","categories":["Databases"],"tags":["references","database","programming"],"sources":["_raw/articles/n-plus-one-problem-research-packet.md"],"created":"2026-09-09","updated":"2026-09-09"}}
---

N+1 problem adalah pola ketika satu query mengambil daftar berisi N item, lalu aplikasi menjalankan query tambahan untuk relasi setiap item. Untuk satu relasi yang selalu di-fetch terpisah tanpa cache, totalnya menjadi `1 + N` query. [SQLAlchemy](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html) menjelaskan pola ini pada lazy loading ORM; [GraphQL](https://graphql.org/learn/performance/) menunjukkan pola serupa pada resolver yang memanggil database atau microservice.

Masalahnya bukan keberadaan loop, melainkan akses backend yang bertambah per item. Satu request HTTP dapat menyembunyikan banyak query database.

## Bagaimana terjadi

Contoh hipotetis: aplikasi mengambil 20 pesanan, kemudian mengambil pelanggan untuk setiap pesanan dengan query terpisah. Tanpa pemakaian ulang hasil, jumlahnya 21 query. Jika pelanggan yang sama sudah ada dalam cache atau identity map, jumlah aktual dapat lebih kecil. [SQLAlchemy](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html) secara khusus menjelaskan pengecualian ini pada simple many-to-one relationship ketika objek target sudah berada dalam Session.

Lazy loading memudahkan akses seperti `order.customer`, tetapi operasi yang terlihat seperti pembacaan atribut dapat memicu SQL. Eager loading menentukan relasi yang dibutuhkan sebelum hasil dipakai. ORM dapat menyebabkan atau membantu menghindari N+1, tergantung loading strategy.

## Memilih perbaikan

| Pendekatan | Cara kerja | Batas penting |
|---|---|---|
| Join atau joined eager loading | Mengambil parent dan relasi dalam query yang sama | Collection join dapat menggandakan baris hasil. |
| Select-in loading atau prefetch | Mengambil parent, kemudian relasi untuk sekumpulan key | Batas ukuran batch dan kemampuan database dapat memerlukan beberapa query. |
| DataLoader | Mengumpulkan permintaan key pada satu jendela eksekusi dan menyerahkannya ke batch function | Batch function harus benar-benar memakai operasi backend berkelompok. |

[SQLAlchemy relationship loading](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html) mendokumentasikan dua strategi pertama. [DataLoader](https://raw.githubusercontent.com/graphql/dataloader/main/README.md) menyediakan batching dan memoization untuk data source yang dapat dimuat berdasarkan key. [[Relationship loading should minimize total work rather than query count\|Relationship loading should minimize total work rather than query count]] menjelaskan mengapa targetnya bukan selalu satu query.

Untuk `joinedload()` pada collection, SQLAlchemy 2.0 mewajibkan `Result.unique()` agar objek parent dideduplikasi. Untuk select-in loading, key dapat dipecah menjadi batch; jadi ungkapan “N+1 menjadi dua query” merupakan contoh sederhana, bukan jaminan universal. Pilih hanya relasi yang memang digunakan, bukan eager-load seluruh graph.

## DataLoader pada GraphQL

[[References/GraphQL\|GraphQL]] memungkinkan resolver field tetap terpisah sambil memakai loader bersama dalam satu request. Menurut [DataLoader](https://raw.githubusercontent.com/graphql/dataloader/main/README.md), load yang masuk dalam satu frame eksekusi digabungkan sebelum batch function dipanggil. Implikasinya, menunggu setiap load selesai secara berurutan dapat memisahkan pekerjaan ke batch berbeda; kebutuhan yang independen perlu dijadwalkan bersama.

Hasil batch harus sama panjang dan urutannya dengan daftar input key. Record yang tidak ditemukan tetap memerlukan posisi hasil berupa nilai kosong atau error. SQL `IN` tidak boleh dianggap mengembalikan record sesuai urutan input.

[[DataLoader caches belong to the request that defines access\|DataLoader caches belong to the request that defines access]] menjelaskan batas keamanan: loader yang menyimpan data sesuai izin pengguna tidak dibagi begitu saja antar-request. Cache juga tidak menggantikan authorization pada pengambilan data.

## Mendeteksi dan memverifikasi

Sebagai prosedur diagnosis yang disintesis dari [GraphQL monitoring](https://graphql.org/learn/performance/) dan [SQLAlchemy loading controls](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html):

- Telusuri query atau backend spans untuk satu operasi aplikasi, bukan hanya jumlah HTTP request.
- Cari query berulang dengan bentuk sama dan key berbeda setelah query daftar.
- Bandingkan jumlah query ketika ukuran daftar bertambah, dengan keadaan cache yang jelas.
- Uji ulang kesamaan hasil, relasi kosong, urutan, dan batas akses setelah optimasi.
- Ukur latency dan volume hasil, bukan hanya query count.

SQLAlchemy menyediakan `raiseload()` untuk menolak lazy relationship access yang tidak direncanakan. Ini membantu menemukan akses tersembunyi, tetapi bukan pengganti pengukuran seluruh operasi.

## Dampak bergantung pada arsitektur

Pada database client/server, query berulang menambah message round trips. Dampaknya bisa besar ketika latency dan jumlah item meningkat. Namun, [SQLite](https://www.sqlite.org/np1queryprob.html) berjalan dalam proses aplikasi dan tidak membayar overhead komunikasi tersebut; banyak query kecil dapat tetap efisien.

Karena itu, N+1 adalah petunjuk untuk memeriksa access pattern, bukan angka slowdown yang berlaku universal. Perbaikan loading juga tidak menghapus kebutuhan index dan query plan yang sesuai sebagaimana dibahas dalam [[References/Relational Databases\|Relational Databases]].
