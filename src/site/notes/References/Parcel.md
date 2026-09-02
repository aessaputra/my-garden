---
{"dg-publish":true,"dg-path":"Parcel.md","permalink":"/parcel/","title":"Parcel","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance"],"noteIcon":"","dg-note-properties":{"title":"Parcel","category":"references","tags":["references","programming","javascript","architecture","performance"],"sources":["_raw/articles/parcel-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

Parcel adalah *build tool* untuk web yang menggabungkan pemrosesan dependensi, transformasi kode, bundling, server pengembangan, dan optimasi produksi. Proyek dapat dimulai dari berkas HTML tanpa menulis konfigurasi bundler terlebih dahulu. Parcel mengikuti referensi [[References/JavaScript\|JavaScript]], CSS, gambar, font, dan aset lain untuk membangun grafik dependensi serta keluaran yang diperlukan.

Istilah "zero configuration" menggambarkan pengalaman awal Parcel, bukan berarti setiap proyek selamanya bebas konfigurasi. Target browser, plugin, transformasi khusus, penamaan keluaran, dan kebutuhan aplikasi yang lebih kompleks tetap dapat memerlukan pengaturan.

## Memulai proyek

Parcel dapat memakai HTML sebagai titik masuk dan menemukan aset yang dirujuk dari sana. Instalasi dasar dan server pengembangan dapat dijalankan dengan perintah berikut.

```sh
npm install --save-dev parcel
npx parcel src/index.html
```

Untuk keluaran produksi, gunakan:

```sh
npx parcel build src/index.html
```

Perintah pengembangan menjalankan server bawaan, sedangkan `parcel build` mengaktifkan mode produksi beserta optimasinya.

## Pemrosesan JavaScript dan aset

Parcel mendukung ES modules dan CommonJS, berbagai bentuk dependensi, transpilation berdasarkan target browser, JSX, serta penghapusan sintaks TypeScript dan Flow. Dukungan TypeScript ini berfokus pada transformasi menjadi JavaScript, bukan pemeriksaan tipe. Dokumentasi Parcel menyarankan TypeScript Compiler atau alat lain untuk pemeriksaan tipe secara terpisah.

Resolver Parcel mengubah *dependency specifier* menjadi lokasi berkas yang dapat dimuat. Proses tersebut berlaku untuk `import` dan `require` di JavaScript, serta referensi seperti `@import` dan `url()` di CSS. Parcel juga memahami fitur paket seperti `exports`, `imports`, alias, dan kondisi target lingkungan.

Dukungan bawaan mencakup HTML, CSS, JavaScript, gambar, font, video, dan jenis aset lain. Jenis berkas tertentu dapat memicu pemasangan plugin selama pengembangan. Sebagai contoh, penggunaan Sass dapat membuat Parcel memasang `@parcel/transformer-sass` secara otomatis. Perilaku ini praktis untuk memulai, tetapi perubahan dependensi otomatis tetap perlu diperiksa sebelum dikomit atau dipakai dalam CI.

## Pengembangan, cache, dan HMR

Server pengembangan Parcel menyediakan *hot reloading*, HTTPS, serta dukungan proksi API. Ketika kode berubah, Parcel membangun ulang berkas yang terdampak dan memperbarui aplikasi di browser. Pembaruan biasanya memuat ulang halaman, tetapi Hot Module Replacement dapat mengganti modul saat runtime tanpa kehilangan seluruh keadaan aplikasi. Perubahan CSS diterapkan melalui HMR tanpa memuat ulang halaman, dan framework seperti [[References/React\|React]] serta [[References/Vue.js\|Vue]] dapat memakai integrasi HMR masing-masing.

Parcel menyimpan hasil pembangunan ke `.parcel-cache`. Cache dilacak berdasarkan berkas sumber, konfigurasi, plugin, dan dependensi pengembangan yang terlibat. Saat Parcel dijalankan kembali, hanya bagian yang berubah atau terinvalidasi yang dibangun ulang. Dokumentasi menyarankan agar folder tersebut tidak dimasukkan ke repositori.

Kinerja nyata tetap bergantung pada ukuran grafik dependensi, jenis transformasi, plugin, media penyimpanan, jumlah inti CPU, dan pola perubahan kode. Sebutan "cepat" tidak menggantikan pengukuran pada proyek sendiri.

## Optimasi produksi

Mode produksi mengaktifkan minifikasi dan optimasi lain secara otomatis. Parcel menyediakan minifier untuk JavaScript, CSS, HTML, dan SVG. Dokumentasi saat ini menyebut [[References/SWC\|SWC]] untuk JavaScript, Lightning CSS untuk CSS, Oxvg untuk SVG, dan minifier bawaan untuk HTML.

Parcel juga menjalankan *tree-shaking* dengan menganalisis impor dan ekspor secara statis, lalu menghapus bagian yang tidak digunakan. Dukungan ini mencakup ES modules, CommonJS, `import()` dinamis, dan CSS modules. Parcel dapat melakukan *scope hoisting* dengan menggabungkan modul ke satu lingkup ketika memungkinkan, sehingga minifikasi lebih efektif dan akses antar-modul tidak selalu memerlukan pencarian objek saat runtime.

Hasil *tree-shaking* tidak selalu maksimal. Akses ekspor yang dinamis, penggunaan `eval`, pengubahan `module` atau `exports`, `return` tingkat atas pada CommonJS, dan efek samping modul dapat mencegah analisis statis atau memaksa Parcel mempertahankan lebih banyak kode. Opsi `--log-level verbose` dapat menampilkan diagnostik ketika *scope hoisting* atau *tree-shaking* dibatalkan.

Parcel menambahkan *content hash* pada sebagian besar nama berkas keluaran agar perubahan konten menghasilkan URL baru untuk cache browser dan CDN. Beberapa jenis keluaran membutuhkan nama stabil, misalnya *service worker*, sehingga tidak selalu memakai perilaku yang sama.

## Code splitting

Parcel mendukung *code splitting* melalui `import()` dinamis. Modul dapat dimuat ketika dibutuhkan, bukan dimasukkan seluruhnya ke paket awal.

```js
button.addEventListener("click", async () => {
  const {openDialog} = await import("./dialog.js");
  openDialog();
});
```

Dependensi yang dipakai oleh beberapa bagian aplikasi dapat dipisahkan ke *bundle* bersama agar tidak diduplikasi dan dapat disimpan terpisah oleh browser. Namun, `import()` dinamis merupakan petunjuk asinkron, bukan jaminan bahwa Parcel selalu membuat berkas baru. Parcel dapat mempertahankan modul di *bundle* yang sama untuk menghindari duplikasi atau berdasarkan strategi bundling yang dipakai.

## Target keluaran

Parcel dapat membangun beberapa target dari sumber yang sama, misalnya keluaran modern dan lama untuk browser, atau target terpisah untuk frontend dan backend. Target menentukan lokasi keluaran dan lingkungan kompilasi. Pengaturan `engines` dan Browserslist membantu Parcel memilih transformasi yang sesuai dengan runtime tujuan.

*Source map* aktif secara bawaan dan dapat dikendalikan per target atau melalui opsi CLI. Optimasi produksi juga dapat disesuaikan, tetapi perilaku tepatnya bergantung pada target dan plugin. Target pustaka, misalnya, tidak selalu memakai pengaturan optimasi yang sama seperti aplikasi.

## Kapan Parcel tepat digunakan

Parcel sesuai ketika proyek memerlukan alur awal yang singkat, dukungan aset terpadu, dan server pengembangan tanpa merakit banyak plugin lebih dahulu. Penggunaannya tidak terbatas pada prototipe. Sistem konfigurasi dan plugin memungkinkan Parcel dipakai pada aplikasi produksi yang lebih besar.

Keputusan memilih Parcel sebaiknya mempertimbangkan kebutuhan integrasi framework, kontrol atas keluaran, kompatibilitas plugin, waktu pembangunan, ukuran *bundle*, serta kemudahan diagnosis. Untuk prototipe, minimnya konfigurasi awal dapat mempercepat pekerjaan. Untuk proyek kompleks, manfaat tersebut perlu dibandingkan dengan kebutuhan konfigurasi khusus dan perilaku otomatis yang harus dipahami tim.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Vite\|Vite]]
- [[References/Rollup\|Rollup]]
- [[References/esbuild\|esbuild]]
- [[References/SWC\|SWC]]
- [[References/npm\|npm]]

## Sumber

- [Parcel documentation](https://parceljs.org/docs): instalasi, titik masuk HTML, alur pengembangan, dan struktur dokumentasi.
- [JavaScript](https://parceljs.org/languages/javascript): ES modules, CommonJS, JSX, TypeScript, Flow, target browser, dan minifikasi JavaScript.
- [Development](https://parceljs.org/features/development): server pengembangan, HMR, cache, HTTPS, proksi API, dan pemasangan plugin otomatis.
- [Production](https://parceljs.org/features/production): minifikasi, tree-shaking, optimasi gambar, content hashing, dan bundling bersama.
- [Dependency resolution](https://parceljs.org/features/dependency-resolution): resolusi import, require, CSS, package exports, alias, dan kondisi lingkungan.
- [Code splitting](https://parceljs.org/features/code-splitting): import dinamis, bundle bersama, dan batas pembentukan chunk.
- [Targets](https://parceljs.org/features/targets): target keluaran, lingkungan kompilasi, Browserslist, source map, dan optimasi.
- [Scope hoisting](https://parceljs.org/features/scope-hoisting): analisis statis, tree-shaking, bailout, efek samping, dan CommonJS.
- [Repositori resmi Parcel](https://github.com/parcel-bundler/parcel): kode sumber, arsitektur, kemampuan bawaan, plugin, dan rilis proyek.
