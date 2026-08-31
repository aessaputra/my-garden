---
{"dg-publish":true,"dg-path":"React Native.md","permalink":"/react-native/","title":"React Native","hideInFiletree":true,"tags":["references","react-native","react","javascript","architecture","performance"],"dg-note-properties":{"title":"React Native","category":"references","tags":["references","react-native","react","javascript","architecture","performance"],"sources":["_raw/articles/react-native-expanded.md"],"created":"2026-08-30","updated":"2026-08-30","confidence":"high"}}
---

React Native adalah kerangka kerja *open source* untuk membangun aplikasi Android dan iOS dengan [[References/React\|React]] dan [[References/JavaScript\|JavaScript]]. Kode React mendeskripsikan tampilan serta perilaku antarmuka, sementara React Native menyediakan akses ke kapabilitas asli tiap platform. Pola komponen, *props*, *state*, dan Hooks tetap digunakan, tetapi hasil akhirnya berupa aplikasi seluler yang memakai elemen antarmuka platform, bukan halaman web di dalam *WebView*.

## Komponen native dan arsitektur

Komponen seperti `View`, `Text`, `Image`, dan `TextInput` dipetakan ke tampilan Android dan iOS yang sesuai saat aplikasi berjalan. Karena komponen tersebut ditopang oleh tampilan platform, aplikasi dapat mengikuti perilaku interaksi dan karakter visual sistem operasi secara lebih alami. Namun, frasa "dikompilasi menjadi komponen native" perlu dipahami secara tepat: JavaScript tidak seluruhnya diubah menjadi Swift atau Kotlin. React Native menjalankan logika React melalui *runtime* JavaScript, lalu mengoordinasikan pembuatan dan pembaruan tampilan native.

Sejak React Native 0.76, Arsitektur Baru diaktifkan secara bawaan. Arsitektur ini mengganti *bridge* asinkron lama dengan JavaScript Interface atau JSI, sehingga JavaScript dan lapisan native dapat berkomunikasi langsung tanpa biaya serialisasi yang sebelumnya diperlukan. Perubahan tersebut juga mendukung *concurrent rendering*, pembacaan tata letak secara sinkron, *automatic batching*, Suspense, dan Transitions. Meski demikian, mengaktifkan Arsitektur Baru tidak otomatis mempercepat setiap aplikasi karena sumber hambatan dapat berada pada logika aplikasi, daftar yang besar, animasi, atau pekerjaan berat di *thread* JavaScript.

## Berbagi kode tanpa menghapus perbedaan platform

Sebagian besar logika bisnis, pengelolaan data, validasi, dan komponen antarmuka dapat digunakan bersama pada Android dan iOS. Untuk perbedaan kecil, modul `Platform` memungkinkan kode memeriksa sistem operasi yang sedang berjalan. Untuk implementasi yang lebih kompleks, React Native memilih berkas berekstensi `.ios.*` atau `.android.*` secara otomatis, sedangkan ekstensi `.native.*` dapat memisahkan implementasi React Native dari kode yang juga dipakai di web atau Node.js. Rincian pola ini tersedia di [[References/Kode Spesifik Platform\|Kode Spesifik Platform]].

*Code sharing* tidak berarti satu basis kode selalu identik untuk seluruh platform. Tim tetap perlu menyesuaikan navigasi, izin, pola interaksi, dan integrasi layanan sistem ketika perilaku Android dan iOS berbeda. Mekanisme pemisahan bawaan membuat penyesuaian tersebut eksplisit tanpa memaksa duplikasi seluruh aplikasi.

## Siklus pengembangan dengan Fast Refresh

[[References/Fast Refresh\|Fast Refresh]] memberi umpan balik hampir seketika ketika komponen React diubah. Pada kebanyakan penyuntingan, perubahan terlihat dalam satu atau dua detik dan *state* lokal pada komponen fungsi serta Hooks dapat dipertahankan. Jika sebuah berkas juga diekspor ke modul di luar pohon React, sistem dapat beralih ke pemuatan ulang penuh. *State* pada komponen kelas juga tidak dipertahankan, sementara Hooks seperti `useEffect`, `useMemo`, dan `useCallback` dapat dijalankan kembali selama proses penyegaran.

Karakteristik ini memperpendek siklus edit dan uji, tetapi Fast Refresh bukan ukuran performa produksi. Mode pengembangan menambah pekerjaan untuk peringatan dan pelaporan galat, sehingga evaluasi performa harus dilakukan pada *release build*.

## Akses API perangkat dan kode native

React Native menyediakan API inti dan ekosistem pustaka untuk kebutuhan umum. Jika API platform yang diperlukan belum tersedia, pengembang dapat membuat [[References/Turbo Modul Native\|Turbo Modul Native]]. Prosesnya dimulai dengan spesifikasi bertipe dalam TypeScript atau Flow, lalu [[References/Apa Itu Codegen\|Codegen]] menghasilkan antarmuka untuk Android dan iOS sebelum implementasi native dihubungkan ke *runtime* React Native. Jalur ini memungkinkan integrasi dengan kemampuan seperti penyimpanan platform, sensor, kamera, Bluetooth, atau SDK vendor, tetapi implementasi tertentu tetap memerlukan Swift, Objective-C, Kotlin, Java, atau C++ sesuai kebutuhan pustaka dan platform.

## Performa dan batas praktis

React Native menargetkan pengalaman dengan tampilan native dan setidaknya 60 *frame* per detik, tetapi kelancaran tetap bergantung pada cara aplikasi dibangun. Logika bisnis, panggilan API, dan pemrosesan peristiwa sentuh umumnya berjalan pada *thread* JavaScript. Pekerjaan berat pada *thread* tersebut dapat menjatuhkan *frame rate* dan menunda respons antarmuka. Sebaliknya, pengguliran `ScrollView` dan animasi navigator native dapat tetap berjalan pada *thread* UI meskipun *thread* JavaScript sedang sibuk.

Tim perlu menguji aplikasi pada perangkat sasaran dan *release build*, mengoptimalkan daftar besar, serta memindahkan pekerjaan yang sensitif terhadap latensi ke mekanisme native bila diperlukan. Pembahasan teknis lebih lanjut tersedia di [[References/Performa di React Native\|Performa di React Native]]. React Native paling sesuai ketika manfaat berbagi kode dan keahlian React lebih besar daripada kebutuhan akan kontrol native penuh pada setiap bagian aplikasi.

## Kapan React Native tepat digunakan

React Native cocok untuk tim yang sudah menguasai React dan ingin mengembangkan aplikasi Android serta iOS dari basis kode bersama. Kerangka kerja ini efektif untuk aplikasi produk, layanan transaksi, konten, dan alat internal yang memakai pola antarmuka standar serta integrasi perangkat yang telah didukung pustaka.

Pilihan ini perlu dievaluasi lebih hati-hati untuk aplikasi dengan pemrosesan grafis berat, aliran data berlatensi sangat rendah, atau ketergantungan besar pada API platform khusus. Dalam kasus tersebut, modul native masih tersedia, tetapi semakin banyak kode native yang dibutuhkan, semakin kecil keuntungan pemeliharaan dari satu basis kode bersama.

## Lihat juga

- [[References/React\|React]]: model komponen, state, Hooks, dan rekonsiliasi.
- [[References/JavaScript\|JavaScript]]: bahasa utama untuk logika aplikasi React Native.
- [[References/Tutorial React Native\|Tutorial React Native]]: pengantar praktik membangun aplikasi.
- [[References/Kode Spesifik Platform\|Kode Spesifik Platform]]: pemisahan implementasi Android dan iOS.
- [[References/Turbo Modul Native\|Turbo Modul Native]]: integrasi API platform pada Arsitektur Baru.
- [[References/Performa di React Native\|Performa di React Native]]: *thread*, *frame rate*, dan optimasi.

## Sumber

- [Core Components and Native Components](https://reactnative.dev/docs/intro-react-native-components): hubungan komponen React Native dengan tampilan Android dan iOS.
- [About the New Architecture](https://reactnative.dev/architecture/landing-page): JSI, rendering konkuren, tata letak sinkron, dan batas peningkatan performa.
- [New Architecture is here](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here): Arsitektur Baru sebagai bawaan sejak React Native 0.76.
- [Fast Refresh](https://reactnative.dev/docs/fast-refresh): cara kerja, pemeliharaan state, dan kondisi pemuatan ulang penuh.
- [Native Modules: Introduction](https://reactnative.dev/docs/turbo-native-modules-introduction): Turbo Native Modules, spesifikasi bertipe, dan Codegen.
- [Platform-Specific Code](https://reactnative.dev/docs/platform-specific-code): modul `Platform` serta ekstensi `.ios.*`, `.android.*`, dan `.native.*`.
- [Performance Overview](https://reactnative.dev/docs/performance): *thread* JavaScript, *thread* UI, target 60 *frame* per detik, dan pengujian *release build*.
