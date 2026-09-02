---
{"dg-publish":true,"dg-path":"Vite.md","permalink":"/vite/","title":"Vite","hideInFiletree":true,"tags":["references","programming","javascript","typescript","performance"],"dg-note-properties":{"title":"Vite","category":"references","tags":["references","programming","javascript","typescript","performance"],"sources":["_raw/articles/vite-expanded.md"],"created":"2026-08-26","updated":"2026-08-26","confidence":"medium"}}
---

Vite adalah alat pengembangan dan *build* untuk proyek web modern. Namanya berasal dari kata bahasa Prancis yang berarti "cepat" dan dilafalkan `/viːt/`. Vite memisahkan kebutuhan pengembangan dari pembuatan aset produksi: *development server* menyajikan modul sesuai permintaan melalui ES Modules (ESM), sedangkan perintah `vite build` menghasilkan aset statis yang telah dioptimalkan. ^[_raw/articles/vite-expanded.md]

Dukungan Vite tidak terbatas pada satu pustaka antarmuka. `create-vite` menyediakan templat untuk JavaScript dan TypeScript biasa, Vue, React, Preact, Lit, Svelte, Solid, dan Qwik. Integrasi kerangka kerja dijalankan melalui plugin, termasuk plugin resmi untuk Vue dan React. Pola ini menjadikan Vite lapisan perkakas yang dapat dipakai oleh proyek kecil, aplikasi multipage, pustaka, maupun kerangka kerja yang membangun infrastrukturnya sendiri di atas API Vite. ^[_raw/articles/vite-expanded.md]

## Mengapa Vite terasa cepat

*Bundler* tradisional perlu memproses sebagian besar atau seluruh graf modul sebelum *development server* siap. Waktu tunggu bertambah ketika aplikasi dan jumlah dependensinya membesar. Vite mengambil pendekatan berbeda dengan membagi kode menjadi dua kelompok: dependensi yang relatif jarang berubah diprapaketkan, sedangkan kode sumber aplikasi disajikan sesuai permintaan melalui ESM bawaan peramban. ^[_raw/articles/vite-expanded.md]

Ketika peramban meminta sebuah modul, Vite mentransformasi dan mengirim berkas tersebut tanpa lebih dahulu membangun satu paket lengkap untuk seluruh aplikasi. Impor paket seperti `import { x } from 'package'`, yang tidak dapat dipahami langsung oleh peramban, diubah menjadi URL yang valid setelah dependensinya diprapaketkan. Permintaan dependensi juga disimpan melalui *cache* HTTP agar pemuatan berikutnya tidak mengulang pekerjaan yang sama. ^[_raw/articles/vite-expanded.md]

Pendekatan ini membuat waktu penyalaan server tidak lagi bergantung langsung pada keharusan membundel seluruh aplikasi di awal. Video pengantar Vite memperlihatkan perbedaan praktis tersebut melalui perbandingan proyek React berbasis Vite dan Create React App, tetapi angka waktu dan ukuran hasil dalam demonstrasi itu berasal dari proyek kecil serta versi perkakas saat video dibuat, sehingga tidak layak diperlakukan sebagai tolok ukur universal. ^[_raw/articles/vite-expanded.md]

## Hot Module Replacement

Vite menyediakan API Hot Module Replacement (HMR) di atas ESM bawaan. Saat berkas berubah, Vite dapat mengganti modul yang terdampak tanpa memuat ulang seluruh halaman. Integrasi kerangka kerja dapat mempertahankan *state* aplikasi selama perubahan aman dilakukan, seperti melalui Vue Single File Components atau React Fast Refresh. ^[_raw/articles/vite-expanded.md]

Dampaknya terlihat pada pekerjaan berulang seperti menyesuaikan CSS atau komponen modal. Pengembang dapat menyimpan perubahan beberapa kali tanpa harus kembali ke kondisi antarmuka yang sama setelah setiap pemuatan ulang. Keuntungan ini mengurangi jeda antara penyuntingan dan umpan balik, terutama ketika pengembang sedang memeriksa perubahan visual atau perilaku komponen. ^[_raw/articles/vite-expanded.md]

## Dari Rollup dan esbuild ke Rolldown

Artikel dan video lama sering menjelaskan bahwa Vite memakai esbuild untuk prapembundelan dan transformasi selama pengembangan, lalu Rollup untuk *build* produksi. Penjelasan tersebut benar secara historis, tetapi sudah tidak menggambarkan arsitektur Vite terbaru. ^[_raw/articles/vite-expanded.md]

Vite 8, yang dirilis pada 12 Maret 2026, mengganti susunan dua *bundler* tersebut dengan Rolldown sebagai *bundler* terpadu berbasis Rust. Oxc menangani pekerjaan seperti penguraian, transformasi, dan minifikasi, sementara kompatibilitas dengan konvensi plugin Rollup dipertahankan agar sebagian besar plugin Vite lama tetap dapat digunakan. Perubahan ini dimaksudkan untuk mengurangi perbedaan perilaku antara jalur pengembangan dan produksi yang sebelumnya memakai sistem transformasi terpisah. ^[_raw/articles/vite-expanded.md]

Menurut pengumuman Vite 8, Rolldown mencapai peningkatan *build* 10 sampai 30 kali dibanding Rollup dalam tolok ukur tim Vite. Angka tersebut bukan jaminan untuk setiap proyek. Waktu aktual tetap dipengaruhi ukuran graf modul, plugin, transformasi khusus, pembagian *chunk*, dan lingkungan perangkat keras. ^[_raw/articles/vite-expanded.md]

## Fitur bawaan

Vite mendukung impor berkas TypeScript, JSX, CSS, JSON, aset statis, Web Workers, dan WebAssembly. Dukungan TypeScript hanya melakukan transpilasi, bukan pemeriksaan tipe. Pemeriksaan tipe perlu dijalankan terpisah melalui IDE, `tsc --noEmit`, proses *watch*, atau plugin pemeriksa agar jalur transformasi Vite tetap cepat. ^[_raw/articles/vite-expanded.md]

Untuk CSS, Vite mendukung CSS Modules, PostCSS, dan praprosesor `.scss`, `.sass`, `.less`, `.styl`, serta `.stylus`. Praprosesor terkait tetap harus dipasang sebagai dependensi proyek, misalnya `sass-embedded` untuk Sass atau `less` untuk Less. Impor CSS menerima HMR, sedangkan *build* produksi dapat memisahkan CSS per *chunk* asinkron dan memuatnya bersama modul terkait. ^[_raw/articles/vite-expanded.md]

Vite juga memproses aset yang direferensikan dari HTML, CSS, dan JavaScript. Impor aset menghasilkan URL yang telah diselesaikan, sementara sufiks seperti `?raw`, `?url`, dan `?worker` mengubah cara berkas dimuat. Fungsi khusus `import.meta.glob` dapat membentuk peta beberapa modul dari pola berkas, tetapi fitur ini khusus Vite dan bukan standar ESM. ^[_raw/articles/vite-expanded.md]

## Struktur proyek dan alur dasar

Dalam proyek Vite, `index.html` berada di akar proyek dan diperlakukan sebagai kode sumber serta bagian dari graf modul. Berkas tersebut menjadi titik masuk aplikasi selama pengembangan, sehingga referensi `<script type="module">`, CSS, dan aset dapat diproses langsung oleh Vite. Susunan ini juga mendukung aplikasi multipage dengan beberapa berkas HTML sebagai titik masuk. ^[_raw/articles/vite-expanded.md]

Proyek baru dapat dibuat dengan perintah berikut:

```bash
npm create vite@latest
```

Setelah dependensi dipasang, alur utamanya terdiri dari tiga perintah:

```bash
npm run dev
npm run build
npm run preview
```

`dev` menjalankan *development server*, `build` membuat aset produksi, dan `preview` menyajikan hasil *build* secara lokal untuk pemeriksaan. Secara bawaan, server pengembangan tersedia di [localhost port 5173](http://localhost:5173), sedangkan hasil produksi ditulis ke direktori `dist`. `vite preview` hanya ditujukan untuk memeriksa hasil secara lokal, bukan sebagai server produksi.

Dokumentasi Vite saat ini mensyaratkan Node.js 20.19+ atau 22.12+, dan templat tertentu dapat menetapkan versi yang lebih tinggi. Persyaratan Node.js 14.18 yang disebut dalam salah satu video merupakan informasi dari masa Vite 3 dan tidak berlaku untuk Vite 8. ^[_raw/articles/vite-expanded.md]

## Build produksi dan kompatibilitas

`vite build` memakai `index.html` sebagai titik masuk bawaan dan menghasilkan paket yang dapat disajikan melalui layanan *static hosting*. Proses ini mencakup pemisahan kode, *tree-shaking*, minifikasi, pengolahan aset, pemisahan CSS, serta optimasi pemuatan *chunk* asinkron. ^[_raw/articles/vite-expanded.md]

Vite 8 menargetkan peramban modern. Untuk produksi, rentang bawaan pada rilis utama saat ini mencakup Chrome 111+, Edge 111+, Firefox 114+, dan Safari 16.4+. Target dapat diubah melalui `build.target`, sedangkan `@vitejs/plugin-legacy` dapat menghasilkan *chunk* dan *polyfill* tambahan untuk peramban tanpa dukungan ESM bawaan. ^[_raw/articles/vite-expanded.md]

Jika aplikasi ditempatkan pada subdirektori, opsi `base` perlu disesuaikan agar URL aset ditulis ulang dengan benar. Vite juga menyediakan mode pustaka, dukungan beberapa titik masuk HTML, pengaturan strategi pembagian *chunk*, dan opsi Rolldown tingkat lanjut untuk kebutuhan yang melampaui konfigurasi standar. ^[_raw/articles/vite-expanded.md]

## Kapan Vite tepat digunakan

Vite cocok untuk proyek frontend modern yang membutuhkan siklus umpan balik singkat, dukungan ESM, integrasi TypeScript atau CSS, dan hasil produksi yang teroptimasi. Pilihan templatnya mencakup Vue, React, Svelte, serta beberapa kerangka kerja lain, sementara sistem plugin memungkinkan integrasi yang lebih khusus. ^[_raw/articles/vite-expanded.md]

Namun, Vite bukan pengganti pemeriksaan tipe, pengujian, *linting*, atau rancangan penerapan. Kecepatan server pengembangan juga tidak berarti setiap aplikasi besar akan selalu bebas hambatan. Dokumentasi Vite mencatat bahwa proyek yang sangat besar dapat mengalami banyak permintaan jaringan karena modul disajikan tanpa bundel selama pengembangan, sehingga Vite sedang mengembangkan *full bundle mode* untuk kasus tersebut. ^[_raw/articles/vite-expanded.md]

Nilai utama Vite terletak pada pemisahan pekerjaan yang jelas. Selama pengembangan, kode disajikan sesuai kebutuhan agar perubahan cepat terlihat. Saat produksi, modul dan aset diproses menjadi keluaran yang lebih efisien untuk didistribusikan. Arsitektur ini memperpendek siklus penyuntingan tanpa mengabaikan kebutuhan optimasi produksi. ^[_raw/articles/vite-expanded.md]

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/React\|React]]
- [[References/Vue.js\|Vue.js]]
- [[References/CSS\|CSS]]
- [[References/Package Managers\|Package Managers]]

## Sumber

- [Vite: The Build Tool for the Web](https://vite.dev/): ringkasan fitur utama dan posisi Vite sebagai alat *build* web.
- [Getting Started](https://vite.dev/guide/): arsitektur dasar, templat, persyaratan Node.js, struktur proyek, dan perintah CLI.
- [Why Vite](https://vite.dev/guide/why.html): alasan penggunaan ESM sesuai permintaan dan evolusi perkakas Vite.
- [Features](https://vite.dev/guide/features.html): HMR, TypeScript, CSS, aset, dan integrasi kerangka kerja.
- [Building for Production](https://vite.dev/guide/build.html): target peramban, *base path*, aplikasi multipage, dan mode pustaka.
- [Vite 8.0 is out](https://vite.dev/blog/announcing-vite8): migrasi dari Rollup dan esbuild ke Rolldown serta perubahan Vite 8.
- [Vite Crash Course](https://youtu.be/LQQ3CR2JTX8): demonstrasi alur proyek, HMR, *build*, dan penerapan pada versi Vite terdahulu.
- [Vite Full Course](https://www.youtube.com/watch?v=VAeRhmpcWEQ): tutorial konfigurasi, templat, variabel lingkungan, dan penerapan pada Vite 3.
