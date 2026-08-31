---
{"dg-publish":true,"dg-path":"Jest.md","permalink":"/jest/","title":"Jest","hideInFiletree":true,"tags":["references","programming","javascript","testing"],"dg-note-properties":{"title":"Jest","category":"references","tags":["references","programming","javascript","testing"],"sources":["_raw/articles/jest-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

Jest adalah framework pengujian [[References/JavaScript\|JavaScript]] open source yang menyediakan test runner, assertion API, mocking, snapshot testing, code coverage, dan watch mode dalam satu perangkat. Jest awalnya dikembangkan di Facebook, lalu Meta memindahkan kepemilikannya kepada OpenJS Foundation pada 2022.

## Menulis dan menjalankan pengujian

Pengujian Jest biasanya disimpan dalam berkas berekstensi `.test.js` atau `.spec.js`, atau di dalam direktori `__tests__`. Fungsi `test` atau `it` mendefinisikan kasus pengujian, sedangkan `expect` dan matcher seperti `toBe`, `toEqual`, atau `toThrow` memeriksa hasilnya.

```js
function sum(a, b) {
  return a + b;
}

test('menjumlahkan dua angka', () => {
  expect(sum(1, 2)).toBe(3);
});
```

Jest dapat menjalankan seluruh test suite, satu berkas tertentu, pengujian dengan nama tertentu, atau hanya pengujian yang berkaitan dengan berkas yang berubah. Watch mode memantau perubahan proyek dan menjalankan kembali pengujian yang relevan selama pengembangan.

## Mocking

Mock function menggantikan implementasi suatu fungsi selama pengujian. Jest dapat merekam jumlah pemanggilan, argumen, nilai kembalian, instance, dan konteks `this`. Fitur ini berguna untuk mengisolasi unit yang diuji dari API, basis data, timer, atau dependensi lain.

```js
const callback = jest.fn();

callback('data');

expect(callback).toHaveBeenCalledWith('data');
expect(callback).toHaveBeenCalledTimes(1);
```

Jest mendukung mock function, manual mock, module mock, partial mock, serta nilai kembalian atau implementasi yang dapat dikonfigurasi per pengujian. Istilah "auto mocking" perlu diberi konteks: Jest menyediakan opsi `automock`, tetapi opsi tersebut tidak aktif secara bawaan. Pengembang juga dapat mengaktifkan mock secara eksplisit melalui `jest.mock()`.

Mocking yang berlebihan dapat membuat pengujian terlalu terikat pada detail implementasi. Mock sebaiknya digunakan untuk mengendalikan batas eksternal atau dependensi yang sulit dijalankan, bukan untuk mengganti seluruh perilaku internal aplikasi.

## Snapshot testing

Snapshot testing menyimpan representasi serialisasi dari suatu nilai, struktur data, atau hasil render komponen. Pada eksekusi berikutnya, Jest membandingkan hasil terbaru dengan snapshot yang tersimpan. Perbedaan menyebabkan pengujian gagal sampai implementasi diperbaiki atau snapshot diperbarui secara sengaja.

Snapshot bukan pengganti assertion yang spesifik. Snapshot berukuran besar sulit ditinjau dan mudah diperbarui tanpa memahami perubahannya. Dokumentasi Jest menyarankan snapshot dibuat singkat, deterministik, disimpan bersama kode, dan diperiksa melalui code review.

## Code coverage

Jest dapat menghasilkan laporan code coverage dengan opsi `--coverage`. Informasi yang dilaporkan dapat mencakup statement, branch, function, dan line coverage. Jest menyediakan provider `babel` dan `v8`, serta dapat menerapkan ambang minimum melalui `coverageThreshold`. Proses pengujian dapat dibuat gagal ketika cakupan berada di bawah batas tersebut.

Coverage menunjukkan bagian kode yang dijalankan oleh pengujian, bukan kualitas assertion atau ketepatan perilaku yang diuji. Angka coverage yang tinggi tetap dapat menyembunyikan pengujian lemah jika test hanya mengeksekusi kode tanpa memeriksa hasil penting.

## Paralelisme dan isolasi

Jest menjalankan test suite melalui worker pool agar beberapa berkas pengujian dapat diproses secara paralel. Jumlah worker dapat dikendalikan dengan `--maxWorkers`, sedangkan `--runInBand` menjalankan semuanya secara serial dalam satu proses. Eksekusi serial berguna untuk debugging atau lingkungan dengan sumber daya terbatas, tetapi biasanya lebih lambat untuk test suite besar.

Paralelisme mengharuskan pengujian tidak bergantung pada state global bersama, urutan eksekusi antarberkas, port yang sama, atau basis data yang tidak diisolasi. Pengujian dengan dependensi semacam itu dapat menghasilkan kegagalan tidak konsisten.

## Dukungan ekosistem

Jest dapat dipakai untuk kode JavaScript umum dan proyek berbasis [[References/React\|React]]. Framework lain, termasuk [[References/Angular\|Angular]] dan [[References/Vue.js\|Vue.js]], juga dapat menggunakannya melalui transform, preset, test environment, atau integrasi komunitas yang sesuai. Jest lebih tepat disebut framework pengujian JavaScript yang dapat diintegrasikan dengan berbagai framework, bukan alat yang bekerja identik pada semuanya tanpa konfigurasi.

Dukungan TypeScript tersedia melalui Babel atau `ts-jest`. Babel hanya mentranspilasi TypeScript dan tidak menjalankan pemeriksaan tipe, sehingga `tsc` tetap perlu dijalankan secara terpisah jika proyek memerlukan type checking.

Ada dua batas integrasi yang perlu diperhatikan. Dokumentasi Jest menyatakan dukungan native ECMAScript Modules masih bersifat eksperimental. Dokumentasi yang sama juga mencatat bahwa Jest tidak terintegrasi langsung dengan sistem plugin [[References/Vite\|Vite]]; proyek berbasis Vite dapat mempertimbangkan [[References/Vitest\|Vitest]], yang menyediakan API kompatibel Jest dan menggunakan pipeline Vite.

## Kapan Jest tepat digunakan

Jest sesuai untuk proyek yang membutuhkan perangkat pengujian terpadu dengan assertion, mocking, snapshot, coverage, watch mode, dan eksekusi paralel. Ia terutama berguna bagi basis kode yang telah memakai Jest, aplikasi React, monorepo dengan konfigurasi beberapa proyek, atau tim yang menginginkan satu toolchain pengujian yang matang.

Klaim bahwa Jest merupakan "pilihan utama" bagi semua pengembang JavaScript terlalu mutlak. Pilihan test runner perlu mempertimbangkan build tool, penggunaan ESM, ukuran test suite, kebutuhan browser, framework, kompatibilitas plugin, kecepatan startup, dan pengalaman tim. Proyek Vite mungkin lebih sesuai dengan Vitest, sedangkan pengujian browser end-to-end memerlukan alat seperti [[References/Playwright\|Playwright]].

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/React\|React]]
- [[References/Vitest\|Vitest]]
- [[References/Vite\|Vite]]
- [[References/Playwright\|Playwright]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [Meta Open Source is transferring Jest to the OpenJS Foundation](https://engineering.fb.com/2022/05/11/open-source/jest-openjs-foundation/) — Meta Engineering: sejarah proyek dan pemindahan kepemilikan.
- [Getting Started](https://jestjs.io/docs/getting-started) — Jest Documentation: instalasi, contoh dasar, TypeScript, bundler, Vite, dan konfigurasi awal.
- [Expect](https://jestjs.io/docs/expect) — Jest Documentation: assertion API dan matcher bawaan.
- [Jest CLI Options](https://jestjs.io/docs/cli) — Jest Documentation: watch mode, coverage, worker, pemilihan test, dan eksekusi serial.
- [Mock Functions](https://jestjs.io/docs/mock-functions) — Jest Documentation: fungsi, modul, implementasi, nilai kembalian, dan pencatatan mock.
- [Configuring Jest](https://jestjs.io/docs/configuration) — Jest Documentation: automock, coverage provider, threshold, test environment, worker, dan ESM.
- [Snapshot Testing](https://jestjs.io/docs/snapshot-testing) — Jest Documentation: snapshot eksternal dan inline, pembaruan, determinisme, serta praktik peninjauan.
