---
{"dg-publish":true,"dg-path":"Flutter.md","permalink":"/flutter/","title":"Flutter","hideInFiletree":true,"tags":["references","flutter","frameworks","programming","ui","performance"],"dg-note-properties":{"title":"Flutter","category":"references","tags":["references","flutter","frameworks","programming","ui","performance"],"sources":["_raw/articles/flutter-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Flutter adalah toolkit UI *open source* dari Google untuk membangun aplikasi dari basis kode Dart yang dapat digunakan kembali pada [[Android\|Android]], iOS, web, Windows, macOS, dan Linux.

Tujuannya bukan menghapus seluruh perbedaan platform. Flutter berbagi sebanyak mungkin kode sambil tetap memberi akses ke layanan sistem dan ruang untuk menyesuaikan perilaku aplikasi pada setiap platform.

## Dart dan model eksekusi

Aplikasi Flutter ditulis dengan Dart, bahasa *type-safe* yang dirancang untuk pengembangan aplikasi lintas platform. Tipe wajib, tetapi anotasi sering dapat disimpulkan oleh *type inference*.

Pada mobile dan desktop, Dart memakai JIT dalam siklus pengembangan dan AOT untuk menghasilkan *machine code* pada rilis. Target web memakai jalur kompilasi web, termasuk JavaScript dan WebAssembly pada browser yang didukung.

Flutter SDK menyertakan Dart SDK, framework, engine rendering 2D, tool `flutter` dan `dart`, pustaka pengujian, serta DevTools. Paket tambahan diperoleh melalui ekosistem pub.dev.

## Arsitektur

Flutter memakai arsitektur berlapis. Embedder menghubungkan aplikasi dengan sistem operasi, engine C++ menyediakan primitive tingkat rendah, framework Dart membentuk UI, lalu kode aplikasi berada di lapisan teratas.

Engine menangani rasterisasi scene, layout teks, grafik, I/O, runtime Dart, dan toolchain kompilasi. Framework di atasnya menyediakan foundation, rendering, widgets, gestures, animation, painting, Material, dan Cupertino.

Struktur ini dapat diperluas. Aplikasi dapat memakai abstraksi tingkat tinggi untuk pekerjaan umum atau turun ke render object dan layanan platform ketika memerlukan kontrol lebih rendah.

## Widget dan UI reaktif

Widget adalah deklarasi *immutable* atas bagian UI. Widget disusun menjadi hierarki melalui komposisi, biasanya berakar pada `MaterialApp` atau `CupertinoApp`.

Ketika data berubah, aplikasi membangun deskripsi widget baru. Framework membandingkannya dengan struktur sebelumnya dan memperbarui elemen serta render object yang diperlukan, bukan menggambar ulang seluruh aplikasi tanpa seleksi.

`StatelessWidget` cocok untuk tampilan yang hanya bergantung pada input dan konteks. `StatefulWidget` memisahkan konfigurasi *immutable* dari objek `State` yang bertahan selama posisi widget tetap cocok dalam tree.

## Pustaka UI

Flutter menyediakan widget dasar untuk layout, teks, input, scrolling, animasi, aksesibilitas, dan interaksi. Pustaka Material dan Cupertino membangun kontrol lengkap di atas primitive komposisi tersebut.

Katalog yang kaya mempercepat pembangunan UI, tetapi tidak mencakup setiap kebutuhan produk. Kamera, webview, pembayaran, autentikasi, dan layanan lain sering memerlukan paket atau plugin tambahan.

## Rendering dan konsistensi visual

Pada platform native, Flutter umumnya memakai implementasi kontrolnya sendiri alih-alih menyerahkan setiap widget kepada pustaka UI sistem. Kode Dart untuk visual dikompilasi dan scene dirasterisasi oleh engine dengan renderer Impeller.

Pendekatan ini memberi kontrol atas setiap piksel dan membantu menjaga tampilan konsisten lintas perangkat. Ia juga membuat framework tidak sepenuhnya bergantung pada versi widget bawaan sistem operasi.

Konsistensi bukan berarti satu layout untuk semua layar. Aplikasi tetap perlu menyesuaikan ukuran jendela, metode input, navigasi, aksesibilitas, dan konvensi platform.

Panduan Flutter menyarankan tiga tahap: abstraksikan data UI, ukur ruang dengan `MediaQuery.sizeOf` atau `LayoutBuilder`, lalu pilih struktur yang sesuai berdasarkan ruang tersedia, bukan hanya nama perangkat.

## Hot reload

Hot reload menyuntikkan berkas Dart yang berubah ke runtime, memperbarui definisi class, lalu meminta framework membangun ulang widget tree. State aplikasi biasanya dipertahankan sehingga siklus edit dan inspeksi menjadi singkat.

Fitur ini hanya berlaku dalam *debug mode*. Perubahan tertentu, seperti modifikasi yang memengaruhi inisialisasi atau bentuk tipe, dapat memerlukan *hot restart* atau restart penuh.

Hot reload meningkatkan kecepatan iterasi, bukan performa aplikasi rilis. Pengukuran performa tetap harus dilakukan pada mode dan perangkat yang mewakili produksi.

## Dukungan platform

Flutter mendukung deployment ke Android dan iOS, desktop Windows, macOS, dan Linux, serta browser Chrome, Firefox, Safari, dan Edge. Arsitektur CPU dan versi minimum bergantung pada rilis Flutter.

Matriks dukungan membedakan platform yang didukung, diuji pada setiap commit, dan tidak didukung. Karena matriks berubah, persyaratan proyek perlu diperiksa pada dokumentasi versi Flutter yang digunakan.

Satu basis kode dapat berbagi model data, logika bisnis, state, jaringan, dan banyak komponen UI. Namun, izin, packaging, signing, distribusi, plugin, dan interaksi sistem tetap memiliki pekerjaan khusus platform.

## Integrasi kode platform

Plugin dan *platform channels* menghubungkan Dart dengan API yang ditulis dalam Kotlin atau Java, Swift atau Objective-C, C++, dan bahasa platform lain.

`MethodChannel` memodelkan pemanggilan metode melalui pesan asinkron. Untuk library native berbasis C, Flutter juga dapat memakai FFI. Jalur ini menjaga akses ke kemampuan sistem yang belum tersedia sebagai API Dart.

Integrasi tersebut bukan bebas biaya. Semakin besar bagian native, semakin banyak kode khusus platform, konfigurasi build, pengujian perangkat, dan pemeliharaan yang harus ditanggung tim.

## Performa dan ukuran aplikasi

Rendering engine memberi Flutter jalur grafis yang terkontrol, tetapi tidak menjamin setiap aplikasi mulus. Jank masih dapat muncul akibat pekerjaan mahal, rebuild berlebihan, shader, I/O, alokasi memori, atau plugin.

Diagnosis harus berbasis metrics. Dokumentasi Flutter membagi perhatian performa ke speed, memory, app size, dan energy, serta menyediakan DevTools untuk menelusuri masalah.

Ukuran aplikasi harus diperiksa pada artifact rilis. Dokumentasi Flutter memperlakukannya sebagai dimensi performa tersendiri karena ukuran unduhan memengaruhi waktu yang diperlukan pengguna untuk memperoleh aplikasi.

## Flutter untuk web

Flutter web cocok untuk SPA interaktif, visual kaya grafis, dan perluasan aplikasi Flutter yang sudah ada ke browser. Ia tidak otomatis menjadi pilihan terbaik bagi setiap situs web.

Dokumentasi Flutter menyatakan konten statis yang kaya teks dan mengikuti aliran dokumen, seperti blog, lebih cocok dengan model web *document-centric*. Flutter dapat tetap disematkan untuk bagian interaktif.

Pertimbangan web mencakup ukuran unduhan, waktu awal, integrasi DOM, aksesibilitas, SEO, dan dukungan browser. Nilainya paling kuat ketika pengalaman *app-centric* lebih penting daripada dokumen HTML tradisional.

## Kapan Flutter tepat digunakan

Flutter layak dipilih ketika satu tim perlu mengirim UI khusus ke beberapa platform, mengutamakan iterasi cepat, dan dapat menerima Dart serta toolchain Flutter sebagai fondasi produk.

Ia juga sesuai ketika konsistensi visual dan komponen bersama memberi nilai lebih besar daripada penggunaan kontrol native bawaan secara langsung.

Evaluasi lebih hati-hati diperlukan untuk situs statis kaya teks, aplikasi yang sangat bergantung pada UI native khusus, ukuran unduhan yang sangat ketat, atau integrasi platform yang dominan.

Perbandingan dengan [[References/React Native\|React Native]] harus berangkat dari model rendering, keahlian tim, ekosistem paket, kebutuhan native, target web, dan biaya pemeliharaan, bukan klaim bahwa salah satunya selalu lebih cepat.

## Lihat juga

- [[Dart\|Dart]]: bahasa dan runtime yang digunakan Flutter.
- [[Android\|Android]]: salah satu platform deployment utama Flutter.
- [[References/React Native\|React Native]]: alternatif lintas platform berbasis React dan komponen native.
- [[References/Tauri\|Tauri]]: pendekatan lintas platform berbasis webview dan backend Rust.
- [[References/Electron\|Electron]]: framework desktop dengan Chromium dan Node.js.

## Sumber

- [Flutter architectural overview](https://docs.flutter.dev/resources/architectural-overview): layer, widget, engine, rendering, kompilasi, embedder, dan integrasi.
- [Flutter SDK overview](https://docs.flutter.dev/tools/sdk): isi SDK dan toolchain.
- [Hot reload](https://docs.flutter.dev/tools/hot-reload): mekanisme, pemeliharaan state, dan batas debug mode.
- [Supported deployment platforms](https://docs.flutter.dev/reference/supported-platforms): matriks mobile, desktop, dan browser untuk Flutter 3.44.7.
- [Web support for Flutter](https://docs.flutter.dev/platform-integration/web): use case aplikasi web dan batas konten document-centric.
- [Performance](https://docs.flutter.dev/perf): metrics, DevTools, speed, memory, app size, dan energy.
- [Writing custom platform-specific code](https://docs.flutter.dev/platform-integration/platform-channels): plugin, FFI, dan platform channels.
- [Dart overview](https://dart.dev/overview): type safety, JIT, AOT, dan target web.
- [Widget catalog](https://docs.flutter.dev/ui/widgets): pustaka Material, Cupertino, dan widget dasar.
- [General approach to adaptive apps](https://docs.flutter.dev/ui/adaptive-responsive/general): abstraksi, pengukuran, dan percabangan UI adaptif.
