---
{"dg-publish":true,"dg-path":"Vue.js.md","permalink":"/vue-js/","title":"Vue.js","hideInFiletree":true,"tags":["references","vue","javascript","ui","frameworks","architecture","performance","ssr"],"noteIcon":"","dg-note-properties":{"title":"Vue.js","category":"references","tags":["references","vue","javascript","ui","frameworks","architecture","performance","ssr"],"sources":["_raw/articles/vuejs-expanded.md"],"created":"2026-08-25","updated":"2026-08-25","confidence":"medium"}}
---

Vue.js adalah framework [[References/JavaScript\|JavaScript]] untuk membangun antarmuka pengguna dengan HTML, CSS, dan JavaScript standar. Vue menyediakan model pemrograman deklaratif berbasis komponen, sehingga pengembang dapat mendeskripsikan tampilan berdasarkan *state* aplikasi dan membagi antarmuka menjadi bagian yang mandiri.

## Framework yang dapat diadopsi bertahap

Sebutan "progresif" pada Vue merujuk pada cara penggunaannya yang dapat disesuaikan dengan kebutuhan proyek. Vue dapat dipasang untuk menambahkan interaksi pada HTML statis tanpa proses *build*, tetapi juga dapat digunakan untuk aplikasi satu halaman, *server-side rendering*, dan *static site generation*. Pola ini memungkinkan tim memperkenalkan Vue pada bagian tertentu dari sistem yang sudah ada sebelum memutuskan apakah seluruh antarmuka perlu dibangun dengannya.

Vue mendukung dua gaya API. Options API mengelompokkan logika melalui opsi seperti `data`, `methods`, dan `mounted`, sedangkan Composition API menyusun logika melalui fungsi seperti `ref` dan *lifecycle hooks*. Dokumentasi Vue menyatakan bahwa keduanya menggunakan sistem dasar yang sama. Options API disarankan untuk skenario sederhana atau tanpa alat *build*, sementara Composition API bersama Single-File Components direkomendasikan untuk aplikasi lengkap.

## Template, reaktivitas, dan proses render

Template Vue memperluas HTML dengan sintaks deklaratif yang menghubungkan tampilan ke *state* JavaScript. Sistem reaktivitas melacak ketergantungan yang dibaca saat proses render, kemudian menjalankan pembaruan saat data terkait berubah. Pada Vue 3, objek reaktif menggunakan `Proxy`, sedangkan `ref` menggunakan *getter* dan *setter* untuk melacak pembacaan serta memicu efek ketika nilainya berubah.

Virtual DOM bukan salinan lengkap halaman, melainkan representasi antarmuka dalam bentuk objek JavaScript yang disimpan di memori. Ketika komponen pertama kali ditampilkan, Vue mengompilasi template menjadi fungsi render, membuat pohon virtual DOM, lalu memasangnya ke DOM nyata. Setelah *state* berubah, Vue membuat representasi baru, membandingkannya dengan versi sebelumnya melalui proses *diffing*, dan menerapkan perubahan yang diperlukan pada DOM.

Kinerja pembaruan Vue tidak hanya bergantung pada virtual DOM. Karena Vue mengendalikan compiler dan runtime, compiler dapat menandai bagian statis serta jenis pembaruan yang dibutuhkan, sehingga runtime dapat melewati pekerjaan yang tidak relevan. Teknik seperti *patch flags*, penyimpanan node statis, dan *tree flattening* mengurangi jumlah node yang perlu diperiksa ketika komponen dirender ulang.

## Komponen dan Single-File Components

Komponen memecah antarmuka menjadi unit mandiri yang dapat digunakan kembali. Setiap penggunaan komponen membuat instans tersendiri dengan *state* lokalnya sendiri. Komponen induk dapat mengirim data ke komponen anak melalui *props*, sedangkan komponen anak dapat memberi tahu induknya melalui peristiwa khusus. Pola ini dapat dibandingkan dengan model komponen pada [[References/React\|React]].

Dalam proyek yang memakai alat *build*, komponen biasanya ditulis sebagai Single-File Component dengan ekstensi `.vue`. Format ini menempatkan template, logika, dan gaya komponen dalam satu berkas melalui blok `<template>`, `<script>`, dan `<style>`. Sebelum dijalankan di peramban, berkas tersebut dikompilasi menjadi JavaScript dan CSS standar.

Single-File Components membutuhkan proses *build*, tetapi memberi beberapa manfaat: template dapat dikompilasi lebih awal, CSS dapat dibatasi ke komponen tertentu, pemeriksaan tipe dan pelengkapan otomatis dapat didukung oleh IDE, serta pembaruan modul saat pengembangan tersedia melalui *Hot Module Replacement*. Untuk interaksi ringan pada halaman yang sebagian besar statis, dokumentasi Vue tetap menyediakan penggunaan melalui JavaScript biasa tanpa Single-File Components.

## Navigasi dan pengelolaan state

Vue berfokus pada lapisan antarmuka, sedangkan kebutuhan aplikasi yang lebih luas ditangani oleh pustaka dalam ekosistemnya. Vue Router adalah router resmi untuk Vue dan menyediakan rute bertingkat, parameter rute, kontrol navigasi, mode riwayat HTML5 atau hash, serta pengaturan perilaku gulir.

Pinia digunakan untuk berbagi *state* antarkomponen atau halaman. Konsep dasarnya berkaitan dengan [[References/State\|State]], tetapi Pinia mengaturnya pada lingkup aplikasi. Pustaka ini menyediakan dukungan TypeScript, alat pengujian, integrasi DevTools, *Hot Module Replacement*, serta dukungan untuk *server-side rendering*. Pinia kini menjadi solusi pengelolaan *state* bawaan yang direkomendasikan dalam ekosistem Vue, menggantikan arah pengembangan Vuex 5 yang sebelumnya direncanakan.

Tidak setiap aplikasi memerlukan router atau penyimpanan global. Komponen dengan alur sederhana dapat mempertahankan *state* lokal dan memakai *props* serta peristiwa, sedangkan Vue Router dan Pinia lebih berguna ketika navigasi atau data bersama mulai melintasi banyak bagian aplikasi.

## Kinerja dan pertimbangan arsitektur

Vue dirancang agar cukup cepat untuk penggunaan umum, tetapi hasil akhirnya tetap dipengaruhi oleh arsitektur, ukuran paket, pola komponen, dan jumlah node DOM. Dokumentasi Vue membedakan kinerja pemuatan awal dari kinerja pembaruan setelah interaksi, karena keputusan yang menguntungkan satu aspek belum tentu optimal untuk aspek lainnya.

Aplikasi yang sensitif terhadap waktu tampil konten sebaiknya tidak selalu dikirim sebagai aplikasi satu halaman yang sepenuhnya dirender di sisi klien. Dokumentasi Vue menyarankan *server-side rendering*, *static site generation*, atau HTML dari server dengan peningkatan interaksi di sisi klien untuk kebutuhan semacam itu. Pengembang juga dapat mengurangi JavaScript yang dikirim dengan kompilasi template saat proses *build*, *tree shaking*, pemisahan kode, dan pemuatan lambat untuk fitur yang belum diperlukan.

Virtual DOM juga tidak menghapus biaya DOM nyata. Daftar berisi ribuan item tetap lambat bila seluruh elemennya dirender, sehingga dokumentasi Vue menyarankan virtualisasi daftar untuk hanya menampilkan item di dalam atau dekat area pandang. Data yang sangat besar dan bertingkat dalam juga dapat menambah biaya pelacakan reaktivitas, yang dapat dikurangi dengan `shallowRef()` atau `shallowReactive()` bila struktur bersarang diperlakukan secara imutabel.

## Popularitas dan pemilihan Vue

Survei State of JavaScript 2024 menempatkan Vue di posisi kedua untuk penggunaan framework antarmuka di antara respondennya. Dalam konteks profesional, 3.976 dari 12.802 responden pertanyaan tersebut menyatakan menggunakan Vue di tempat kerja. Angka ini menunjukkan adopsi yang luas dalam sampel survei, tetapi tidak dapat diperlakukan sebagai pangsa pasar global karena hasilnya terbatas pada peserta yang memilih mengikuti survei.

Vue sesuai untuk tim yang menginginkan sintaks dekat dengan HTML, adopsi bertahap, dan pilihan antara struktur Options API atau Composition API. Keputusan akhir tetap perlu mempertimbangkan kemampuan tim, kebutuhan render awal, ukuran dan umur aplikasi, integrasi dengan sistem yang ada, serta ketersediaan pustaka untuk kebutuhan khusus. Dengan batasan tersebut, Vue dapat digunakan mulai dari peningkatan kecil pada halaman statis hingga aplikasi antarmuka yang dikelola sebagai kumpulan komponen. Untuk kebutuhan yang lebih kompleks, Vue Router dapat menangani rute dan Pinia dapat mengelola *state* bersama.


## Lihat juga

- [[References/JavaScript\|JavaScript]]: bahasa utama yang digunakan Vue.
- [[References/React\|React]]: pustaka antarmuka berbasis komponen dengan mekanisme render yang berbeda.
- [[References/State\|State]]: konsep data yang berubah dan memengaruhi tampilan.
- [[References/HTML\|HTML]]: struktur markup yang diperluas oleh template Vue.
- [[References/CSS\|CSS]]: bahasa gaya yang dapat ditempatkan dalam Single-File Components.

## Sumber

- [Introduction | Vue.js](https://vuejs.org/guide/introduction.html): definisi, pola adopsi, gaya API, dan rekomendasi penggunaan.
- [Single-File Components | Vue.js](https://vuejs.org/guide/scaling-up/sfc.html): format `.vue`, proses kompilasi, dan manfaat Single-File Components.
- [Rendering Mechanism | Vue.js](https://vuejs.org/guide/extras/rendering-mechanism.html): virtual DOM, pipeline render, serta optimasi compiler.
- [Reactivity in Depth | Vue.js](https://vuejs.org/guide/extras/reactivity-in-depth.html): cara kerja reaktivitas melalui `Proxy`, *getter*, dan *setter*.
- [Components Basics | Vue.js](https://vuejs.org/guide/essentials/component-basics.html): komponen, *props*, peristiwa, dan *state* lokal.
- [Introduction | Vue Router](https://router.vuejs.org/introduction.html): fitur router resmi untuk Vue.
- [Introduction | Pinia](https://pinia.vuejs.org/introduction.html): pengelolaan *state* bersama dan posisinya dalam ekosistem Vue.
- [Performance | Vue.js](https://vuejs.org/guide/best-practices/performance): pertimbangan arsitektur dan optimasi kinerja.
- [State of JavaScript 2024: Front-end Frameworks](https://2024.stateofjs.com/en-US/libraries/front-end-frameworks/): data penggunaan Vue di antara responden survei.
