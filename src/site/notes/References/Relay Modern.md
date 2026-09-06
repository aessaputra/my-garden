---
{"dg-publish":true,"dg-path":"Relay Modern.md","permalink":"/relay-modern/","title":"Relay Modern","hideInFiletree":true,"tags":["react","architecture"],"noteIcon":"","dg-note-properties":{"title":"Relay Modern","categories":["Frameworks"],"tags":["react","architecture"],"created":"2026-09-06","updated":"2026-09-06","sources":["_raw/articles/relay-modern-research-packet.md"]}}
---

Relay adalah GraphQL client untuk aplikasi [[References/React\|React]], berasal dari Facebook. Fragment mendeklarasikan kebutuhan data komponen, lalu compiler dan runtime mengatur pengambilan serta pembaruannya. Lihat [Relay](https://relay.dev/).

Catatan ini membahas arsitektur Relay pada dokumentasi aktif, bukan tutorial API Relay Classic. [[References/GraphQL\|GraphQL]] menyediakan schema dan operasi; Relay mengatur penggunaannya di sisi client.

## Fragment dan compiler

Komponen membaca data lewat `useFragment`. Fragment tidak mengambil data sendiri: fragment harus masuk ke query induk, sehingga kebutuhan komponen dapat digabungkan. Lihat [fragment](https://relay.dev/).

`loadQuery` dapat memulai pengambilan saat navigasi; `usePreloadedQuery` membaca hasilnya. Pola ini memisahkan waktu memulai request dari pembacaan data saat rendering. Lihat [query](https://relay.dev/).

Compiler menghasilkan artifact runtime serta tipe TypeScript atau Flow dari operasi dan fragment. Konfigurasi menentukan lokasi source dan schema. Lihat [compiler](https://relay.dev/docs/guides/compiler/).

Biaya adopsinya mencakup integrasi compiler dan transformasi build. `relay-compiler --validate` memeriksa artifact yang kedaluwarsa tanpa menulis berkas. Lihat [validasi](https://relay.dev/docs/guides/compiler/).

## Cache dan pembaruan

Runtime memakai cache ternormalisasi di memori. `Environment` menggabungkan `Store` dan `Network`; hasil operasi menjadi record beridentitas. Lihat [arsitektur](https://relay.dev/docs/principles-and-architecture/runtime-architecture/).

Mutation response dengan ID yang cocok memperbarui field record yang ada. Perubahan relasi atau daftar dapat membutuhkan directive atau `updater`. Lihat [mutation](https://relay.dev/docs/guided-tour/updating-data/graphql-mutations/).

`store-or-network` memakai cache, lalu mengambil query jika data hilang atau stale. `store-and-network` selalu meminta jaringan sambil memakai cache. Lihat [policy](https://relay.dev/docs/guided-tour/reusing-cached-data/fetch-policies/).

`network-only` selalu meminta jaringan; `store-only` tidak melakukannya. Cache bukan jaminan bahwa client mengetahui setiap perubahan server. Lihat [kebijakan](https://relay.dev/docs/guided-tour/reusing-cached-data/fetch-policies/).

## Optimistic UI dan realtime

`optimisticResponse` atau `optimisticUpdater` menampilkan hasil perkiraan. Relay melakukan rollback saat gagal dan memakai hasil server saat sukses. Lihat [optimisme](https://relay.dev/docs/guided-tour/updating-data/graphql-mutations/).

Prediksi bertumpuk berbasis nilai store bisa keliru setelah rollback. Perubahan fragment bisa membuat payload optimistis tidak lengkap. Lihat [batas mutation](https://relay.dev/docs/guided-tour/updating-data/graphql-mutations/).

`useSubscription` dan `requestSubscription` menerima event server. Network layer perlu konfigurasi subscriptions, umumnya memakai WebSocket. Lihat [subscriptions](https://relay.dev/docs/guided-tour/updating-data/graphql-subscriptions/).

Realtime bukan sinkronisasi otomatis. Config `useSubscription` perlu dimemoisasi agar render tidak membuat ulang subscription. Lihat [panduan](https://relay.dev/docs/guided-tour/updating-data/graphql-subscriptions/).

## Kapan dipilih

Relay layak dievaluasi ketika banyak komponen berbagi entitas dan perubahan kebutuhan data sulit dikoordinasikan. Fragment dan compiler menukar pekerjaan manual dengan disiplin schema serta build. Lihat [adopsi](https://relay.dev/).

Dukungan pagination berbasis connections bergantung pada schema server. Identitas objek stabil membantu cache dan merge, bukan persyaratan bahwa setiap schema wajib mengikuti seluruh pola Relay. Lihat [Relay](https://relay.dev/).

Klaim performa di sini menjelaskan mekanisme, bukan hasil benchmark. Sumber seluruhnya dokumentasi proyek; tidak ada pengujian aplikasi atau pembandingan terukur dengan [[References/Apollo\|Apollo]].

## Lanjutan

- [[Colocated fragments make component data dependencies explicit\|Colocated fragments make component data dependencies explicit]]
- [[Normalized caches need deliberate update policies\|Normalized caches need deliberate update policies]]
- [[Optimistic updates predict success rather than confirm it\|Optimistic updates predict success rather than confirm it]]
