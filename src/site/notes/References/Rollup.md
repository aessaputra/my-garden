---
{"dg-publish":true,"dg-path":"Rollup.md","permalink":"/rollup/","title":"Rollup","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance"],"noteIcon":"","dg-note-properties":{"title":"Rollup","category":"references","tags":["references","programming","javascript","architecture","performance"],"sources":["_raw/articles/rollup-expanded.md"],"created":"2026-08-31","updated":"2026-09-03","confidence":"high"}}
---

Rollup adalah *module bundler* untuk [[References/JavaScript\|JavaScript]]. Alat ini membaca modul beserta hubungan `import` dan `export`, membangun grafik modul, lalu menghasilkan satu atau beberapa berkas untuk pustaka maupun aplikasi. Rollup dirancang di sekitar ES modules, yang struktur statisnya memungkinkan analisis dan optimasi sebelum kode dijalankan.

Rollup dikenal karena *tree-shaking*, yaitu proses menghilangkan bagian kode yang tidak diperlukan oleh hasil akhir. Namun, Rollup tidak selalu menghasilkan *bundle* yang lebih kecil daripada setiap alternatif. Ukuran keluaran tetap dipengaruhi bentuk modul, efek samping, dependensi, plugin, target format, pemisahan kode, minifikasi, dan konfigurasi proyek.

## ES modules dan tree-shaking

ES modules memakai deklarasi statis seperti `import` dan `export`. Struktur ini membantu Rollup mengetahui hubungan antar-*binding* tanpa menjalankan program, sehingga optimasi seperti *tree-shaking* dan *scope hoisting* dapat dilakukan dengan lebih efektif.

Rollup menyebut algoritme *tree-shaking*-nya sebagai *live code inclusion*. Proses tersebut menandai pernyataan yang relevan, mengikuti dependensinya, dan mengecualikan kode yang tidak dibutuhkan dari keluaran. Pernyataan yang dinilai memiliki efek samping tetap dipertahankan agar perilaku program tidak berubah.

```js
// math.js
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;

// main.js
import { add } from "./math.js";

console.log(add(2, 3));
```

Jika `multiply` tidak digunakan dan tidak menimbulkan efek samping yang perlu dipertahankan, Rollup dapat mengeluarkannya dari *bundle*. Hasil aktual tetap bergantung pada kemampuan Rollup dan plugin untuk menganalisis modul yang terlibat.

## Format keluaran

Opsi `output.format` mendukung ES modules (`es`), CommonJS (`cjs`), AMD, IIFE, UMD, dan SystemJS (`system`). Pilihan tersebut dapat dipakai untuk menyediakan keluaran bagi *bundler*, lingkungan CommonJS, atau skrip mandiri di browser.

```js
// rollup.config.js
export default {
  input: "src/index.js",
  output: [
    {
      file: "dist/index.js",
      format: "es",
      sourcemap: true
    },
    {
      file: "dist/index.cjs",
      format: "cjs",
      sourcemap: true
    }
  ]
};
```

Rollup mendukung banyak titik masuk dan *code splitting*. Ketika satu proses menghasilkan lebih dari satu *chunk*, `output.dir` digunakan sebagai direktori keluaran. `output.file` sesuai ketika hasilnya hanya satu berkas.

## Dependensi eksternal dan pustaka

Opsi `external` mempertahankan dependensi tertentu sebagai `import` atau `require` di keluaran, alih-alih memasukkannya ke dalam *bundle*. Pengaturan ini berguna ketika sebuah pustaka ingin membiarkan aplikasi pemakai menyediakan dependensinya sendiri.

Dukungan banyak format keluaran, dependensi eksternal, dan optimasi berbasis ES modules membuat Rollup sesuai untuk alur penerbitan pustaka. Kebutuhan aplikasi seperti transformasi aset dan integrasi khusus dapat ditambahkan melalui plugin atau alat tingkat lebih tinggi.

## Plugin dan dukungan format lain

Inti Rollup berfokus pada ES modules. Resolusi paket seperti Node.js dan pemrosesan CommonJS disediakan melalui plugin opsional, termasuk `@rollup/plugin-node-resolve` dan `@rollup/plugin-commonjs`. Repositori plugin resmi juga menyediakan integrasi untuk JSON, TypeScript, Babel, [[References/SWC\|SWC]], Terser, WebAssembly, dan kebutuhan transformasi lainnya.

Plugin Rollup adalah objek yang menyediakan satu atau beberapa *hook*. `resolveId` dapat mengubah cara modul ditemukan, `load` dapat menyediakan isi modul, `transform` dapat memodifikasi kode, dan `generateBundle` dapat memeriksa atau mengubah hasil akhir. Kemampuan proyek tetap bergantung pada kualitas dan kompatibilitas plugin yang dipilih.

## CLI, konfigurasi, dan API JavaScript

Rollup dapat dijalankan melalui CLI dengan konfigurasi opsional atau melalui API JavaScript. Berkas `rollup.config.js` atau variannya dapat mendefinisikan titik masuk, keluaran, dependensi eksternal, dan plugin.

```sh
rollup src/index.js --file dist/bundle.js --format es
```

Fungsi `rollup.rollup()` membangun grafik modul dan menjalankan *tree-shaking*. Objek `bundle` kemudian dapat menghasilkan beberapa format melalui `bundle.generate()` atau menulisnya dengan `bundle.write()`. Rollup juga menyediakan `rollup.watch()` untuk membangun ulang ketika berkas yang dipantau berubah.

## Batasan dan pemilihan

*Tree-shaking* tidak menjamin seluruh kode yang tampak tidak digunakan akan hilang. Efek samping pada tingkat modul, CommonJS, transformasi plugin, dan konfigurasi `treeshake` dapat membatasi optimasi. Konfigurasi asumsi efek samping yang terlalu agresif juga dapat menghapus perilaku yang sebenarnya dibutuhkan.

Rollup berfokus pada bundling dan tidak menangani setiap bahasa atau jenis aset secara bawaan. TypeScript, CommonJS, transformasi tertentu, minifikasi, dan jenis aset lain dapat memerlukan plugin. Pemilihan Rollup perlu mempertimbangkan format keluaran, ekosistem plugin, waktu pembangunan, kompatibilitas target, dan kemudahan pemeliharaan, bukan ukuran *bundle* semata.

Rollup tepat digunakan ketika proyek memerlukan kendali rinci atas grafik modul dan keluaran, terutama untuk pustaka dengan beberapa format distribusi. Keunggulannya perlu dibuktikan melalui pengukuran pada proyek sendiri.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Module Bundlers\|Module Bundlers]]
- TypeScript
- [[References/Vite\|Vite]]
- [[References/esbuild\|esbuild]]
- [[References/SWC\|SWC]]
- [[References/Bun\|Bun]]

## Sumber

- [Introduction: Rollup](https://rollupjs.org/introduction): konsep dasar, ES modules, tree-shaking, format keluaran, dan penggunaan Rollup.
- [Frequently Asked Questions: Rollup](https://rollupjs.org/faqs): algoritme tree-shaking, efek samping, CommonJS, dan ruang lingkup inti Rollup.
- [Configuration Options: Rollup](https://rollupjs.org/configuration-options): opsi masukan, keluaran, external, code splitting, sourcemap, dan treeshake.
- [Plugin Development: Rollup](https://rollupjs.org/plugin-development): model plugin dan hook pembangunan.
- [JavaScript API: Rollup](https://rollupjs.org/javascript-api): API rollup, generate, write, dan watch.
- [Repositori resmi Rollup](https://github.com/rollup/rollup): kode sumber, dokumentasi, lisensi, dan rilis proyek.
- [Repositori plugin resmi Rollup](https://github.com/rollup/plugins): plugin resmi untuk resolusi modul, CommonJS, TypeScript, SWC, dan transformasi lain.
