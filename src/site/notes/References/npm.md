---
{"dg-publish":true,"dg-path":"npm.md","permalink":"/npm/","title":"npm","hideInFiletree":true,"tags":["javascript","programming","npm","nodejs","package-manager","guide"],"dg-note-properties":{"title":"npm","category":"references","tags":["javascript","programming","npm","nodejs","package-manager","guide"],"sources":["_raw/articles/npm-docs.md","_raw/articles/modern-js-dinosaurs-peterxjang.md","_raw/articles/npm-tutorial-beginners-youtube-transcript.md","https://docs.npmjs.com/","https://peterxjang.com/blog/modern-javascript-explained-for-dinosaurs.html","https://www.youtube.com/watch?v=2V1UUhBJ62Y"],"created":"2026-08-22","updated":"2026-08-22"}}
---

npm atau Node Package Manager adalah manajer paket utama untuk JavaScript. Pusatnya di registry npmjs.com tempat orang berbagi kode yang bisa dipakai ulang. Anda menemukan paket, menginstalnya, dan mengelola versi dependensi tanpa download manual satu per satu. Meski lahir untuk Node.js di server, npm kini menjadi standar untuk frontend juga dan tetap paling populer dibanding Yarn dan pnpm yang tetap memakai paket npm di bawahnya.

## Apa yang dipecahkan npm

Cara lama menambah pustaka adalah download file `moment.min.js`, letakkan di folder proyek, dan tambah tag `<script>` di HTML sesuai urutan. Itu mudah dipahami, tetapi merepotkan saat update dan rawan salah urutan. npm mengotomasi proses itu lewat command line dan file konfigurasi `package.json`.

## Pasang dan cek

npm ikut terinstal saat Anda memasang Node.js dari nodejs.org. Cek instalasi dengan:

```bash
node -v
npm -v
```

## Alur kerja inti

### 1. Inisialisasi proyek

Masuk ke folder proyek (`cd` ke path proyek, `ls` untuk memastikan file), lalu:

```bash
npm init
```

Perintah ini menanyakan nama, versi, deskripsi, author, dan license, lalu membuat `package.json`. Isi awal berisi field `name`, `version`, `main`, `scripts`, dan blok kosong untuk `dependencies`.

### 2. Pasang paket

Contoh memasang pustaka format tanggal moment:

```bash
npm install moment --save
# singkatan: npm i moment
```

Yang terjadi:

- Kode paket diunduh ke folder `node_modules` (misalnya `node_modules/moment/min/moment.min.js`).
- `package.json` diperbarui dengan `dependencies: { "moment": "^2.22.2" }`.
- `package-lock.json` dibuat berisi detail versi pasti dan struktur dependensi.

Contoh kedua dari video: deduplikasi array dengan paket `uniq` (`npm install uniq`, lalu `uniq([1,1,1,2,3,3,4,5,6,4,7])` menghasilkan `[1,2,3,4,5,6,7]`).

Untuk berbagi proyek, cukup kirim `package.json`. Rekan menjalankan `npm install` tanpa argumen dan npm akan memulihkan seluruh `node_modules` secara konsisten.

### 3. Memakai paket di kode

Cara lama: referensikan file di HTML:

```html
<script src="node_modules/moment/min/moment.min.js"></script>
<script src="index.js"></script>
```

Cara Node: impor langsung di JavaScript:

```js
var moment = require('moment');
console.log(moment().startOf('day').fromNow());
```

Atau dengan sintaks ES2015 setelah transpiler diatur:

```js
import moment from 'moment';
```

## Mengapa browser butuh bundler

Browser tidak mengenali `require` dan tidak punya akses file system seperti Node. Menjalankan `require('moment')` di browser menghasilkan `require is not defined`. Solusinya adalah module bundler yang dijalankan saat build dan mengganti pernyataan `require` dengan isi file yang dibutuhkan, lalu menghasilkan satu bundle yang bisa dimuat browser.

- Browserify (2011) mempopulerkan `require` gaya Node di frontend dan membuka jalan npm sebagai manajer paket frontend.
- webpack (menjadi dominan sekitar 2015, didorong React) melakukan hal yang sama dengan fitur lebih lengkap.

Contoh dengan Browserify:

```bash
npm install -g browserify  # atau sebagai devDependency
browserify main.js -o bundle.js
```

Lalu ganti di `index.html`:

```html
<script src="bundle.js"></script>
```

Dan contoh dengan webpack:

```bash
npm install webpack webpack-cli --save-dev
./node_modules/.bin/webpack index.js --mode=development
# hasil default: dist/main.js
```

Konfigurasi `webpack.config.js` menyederhanakan pemanggilan ini, dan script `npm run build` atau `npm run watch` bisa dipakai untuk minifikasi serta rebuild otomatis.

Video freeCodeCamp menekankan poin yang sama: setelah bundle dibuat, halaman menampilkan tanggal terformat dengan rapi, dan setiap perubahan di `main.js` perlu di-bundle ulang (bisa diotomasi).

## npm dalam ekosistem JavaScript modern

Artikel Peter Jang menempatkan npm sebagai langkah pertama dari empat pilar workflow modern:

1. **Package manager** (npm) untuk download dan update paket otomatis.
2. **Module bundler** (webpack/Browserify) untuk gabungkan modul menjadi satu file.
3. **Transpiler** (Babel/TypeScript) untuk tulis fitur JavaScript baru lalu ubah ke ES5 yang kompatibel.
4. **Task runner** (npm scripts, dulu Grunt/Gulp) untuk otomasi build, minify, dan dev server.

Instalasi webpack dan Babel sendiri dilakukan lewat npm sebagai `devDependencies`:

```bash
npm install webpack webpack-cli --save-dev
npm install @babel/core @babel/preset-env babel-loader --save-dev
```

Lalu `webpack.config.js` mengatur loader Babel, dan `package.json` menambahkan script seperti:

```json
{
  "scripts": {
    "build": "webpack --progress --mode=production",
    "watch": "webpack --progress --watch",
    "serve": "webpack-dev-server --open"
  }
}
```

## Build yang konsisten dan peran package.json

`package.json` mencatat semua kebutuhan proyek, `package-lock.json` mengunci versi pasti untuk build yang konsisten di setiap lingkungan. Folder `node_modules` sengaja tidak dibagikan karena ukurannya besar. Alternatif Yarn muncul akhir 2016 sebagai antarmuka lain di atas registry npm, tetapi npm tetap rujukan utama.

## Kapan tidak membuat halaman terpisah

Penyebutan npm di beberapa commit skripsi atau blueprint admin panel cukup sebagai referensi pada halaman ini, tidak perlu halaman baru.

## Lihat juga

- [[References/JavaScript\|JavaScript]]: bahasa yang di-package oleh npm, termasuk modul dan bundling
- [[References/Git\|Git]]: version control yang sering dipakai bersama `package.json`
- [[GitHub\|GitHub]]: hosting repo yang sering memicu `npm install` di CI/CD
- [[Node.js\|Node.js]]: runtime yang membawa npm secara bawaan
- [[References/HTML\|HTML]] dan [[References/CSS\|CSS]]: aset statis yang dimuat bersama bundle JavaScript

## Sumber

- Dokumentasi resmi npm: https://docs.npmjs.com/ (landing: About npm, Getting started, Packages and modules, npm CLI)
- Peter Jang - Modern JavaScript Explained For Dinosaurs (2017): https://peterxjang.com/blog/modern-javascript-explained-for-dinosaurs.html - sejarah HTML manual, Bower vs npm, package.json, Browserify dan webpack, Babel, npm scripts
- freeCodeCamp / iEatWebsites - npm Tutorial for Beginners (YouTube, 2018-11-16, 14:34): https://www.youtube.com/watch?v=2V1UUhBJ62Y - demo `npm init`, `npm i moment`, `package-lock.json`, error `require is not defined`, bundling dengan Browserify, contoh `uniq`
