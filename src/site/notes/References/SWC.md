---
{"dg-publish":true,"dg-path":"SWC.md","permalink":"/swc/","title":"SWC (Speedy Web Compiler)","hideInFiletree":true,"tags":["references","programming","javascript","typescript","architecture","performance"],"dg-note-properties":{"title":"SWC (Speedy Web Compiler)","category":"references","tags":["references","programming","javascript","typescript","architecture","performance"],"sources":["_raw/articles/swc-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

SWC, singkatan dari Speedy Web Compiler, adalah platform berbasis Rust untuk memproses [[References/JavaScript\|JavaScript]] dan [[typescript\|TypeScript]]. Komponen utamanya dapat mengurai, mentransformasi, dan menghasilkan kode, sedangkan perangkat terkait menyediakan minifikasi, bundling, integrasi dengan *build tool*, serta sistem plugin.

SWC sering dibandingkan dengan Babel karena keduanya dapat mengubah sintaks JavaScript modern, JSX, TypeScript, dan TSX menjadi JavaScript yang sesuai dengan target eksekusi tertentu. Situs resmi SWC melaporkan kecepatan hingga 20 kali Babel pada satu *thread* dan 70 kali pada empat inti. Angka tersebut berasal dari tolok ukur proyek SWC, sehingga tidak dapat dianggap sebagai peningkatan tetap untuk setiap proyek. Waktu pembangunan nyata juga dipengaruhi konfigurasi, ukuran basis kode, transformasi yang digunakan, I/O, minifikasi, dan alat yang menjalankan SWC.

## Kompilasi dan transformasi

SWC dapat membaca JavaScript atau TypeScript yang memakai fitur bahasa modern, lalu menghasilkan JavaScript untuk target browser yang ditentukan. Parser-nya mendukung ECMAScript, JSX, TypeScript, dan TSX melalui konfigurasi yang sesuai. Target keluaran dapat diatur berdasarkan versi ECMAScript atau daftar browser, sementara transformasi tertentu dapat disertakan atau dikecualikan.

Konfigurasi proyek biasanya disimpan dalam `.swcrc`. Contoh berikut mengaktifkan parser TypeScript dengan TSX dan menetapkan target keluaran ES2020:

```json
{
  "jsc": {
    "parser": {
      "syntax": "typescript",
      "tsx": true
    },
    "target": "es2020"
  }
}
```

Paket `@swc/core` menyediakan API JavaScript seperti `transform`, `transformFile`, `parse`, dan `print` untuk memakai kemampuan SWC secara terprogram. Penggunaan melalui baris perintah tersedia lewat `@swc/cli`, sedangkan integrasi lain mencakup `swc-loader` untuk webpack dan Rspack serta `@swc/jest` untuk transformasi kode dalam Jest.

## Dukungan TypeScript bukan pemeriksaan tipe

SWC mentranspilasi TypeScript dengan menghapus sintaks tipe dan menghasilkan JavaScript, tetapi proses tersebut tidak menggantikan pemeriksaan tipe oleh TypeScript Compiler. SWC memproses berkas satu per satu, sehingga transformasi yang membutuhkan pemahaman atas keseluruhan sistem tipe tidak dapat dilakukan dengan cara yang sama seperti `tsc`.

Proyek TypeScript yang memakai SWC tetap perlu menjalankan `tsc --noEmit` atau pemeriksa tipe lain sebagai langkah terpisah. Opsi seperti `isolatedModules` atau `verbatimModuleSyntax` membantu mendeteksi pola lintas berkas yang tidak aman bagi transpiler per berkas.

## Minifikasi, bundling, dan peran dalam build tool

Selain kompilasi, SWC menyediakan minifikasi dan dapat menggabungkan beberapa berkas JavaScript atau TypeScript melalui konfigurasi bundling. SWC juga dapat dipakai sebagai komponen dalam alat tingkat tinggi yang menangani transformasi dan minifikasi kode.

[[References/Next.js\|Next.js]] merupakan contoh adopsi tersebut. Next.js Compiler ditulis dalam Rust dengan SWC untuk mentransformasi dan meminifikasi JavaScript, menggantikan Babel pada transformasi per berkas dan Terser pada minifikasi keluaran. Dokumentasi Next.js melaporkan kompilasi 17 kali lebih cepat daripada Babel, sekitar tiga kali lebih cepat untuk Fast Refresh, dan sekitar lima kali lebih cepat untuk proses pembangunan dalam pengujiannya sendiri. Hasil tersebut menggambarkan integrasi Next.js, bukan jaminan umum bagi setiap penggunaan SWC.

SWC juga digunakan oleh alat seperti Parcel dan Deno. Posisi ini membuatnya lebih tepat dipahami sebagai infrastruktur compiler yang dapat ditanamkan ke alat lain, bukan hanya pengganti langsung satu per satu untuk Babel atau bundler tertentu.

## Migrasi dan batas kompatibilitas

Migrasi dari Babel dapat sederhana ketika proyek hanya memakai transformasi ECMAScript standar, tetapi konfigurasi Babel tidak selalu memiliki padanan langsung di SWC. Proyek yang bergantung pada plugin Babel khusus perlu memeriksa ketersediaan transformasi atau plugin SWC yang setara sebelum berpindah. SWC memiliki sistem plugin, tetapi kompatibilitas plugin dan integrasinya ditentukan oleh versi SWC serta alat yang menjalankannya.

Pemilihan SWC paling masuk akal ketika waktu transformasi menjadi hambatan, proyek memakai JavaScript atau TypeScript dalam skala besar, atau alat utama sudah menyediakan integrasi SWC yang terpelihara. Keputusan migrasi sebaiknya didasarkan pada tolok ukur di basis kode sendiri, kesetaraan keluaran, dukungan plugin, pemeriksaan tipe terpisah, dan biaya pemeliharaan konfigurasi.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[typescript\|TypeScript]]
- [[References/Next.js\|Next.js]]
- [[References/Vite\|Vite]]
- [[References/esbuild\|esbuild]]
- [[References/Biome\|Biome]]
- [[References/Bun\|Bun]]

## Sumber

- [SWC](https://swc.rs): ikhtisar platform, kompilasi, bundling, adopsi, dan tolok ukur resmi.
- [@swc/core](https://swc.rs/docs/usage/core): API pemrograman dan opsi transformasi.
- [Compilation](https://swc.rs/docs/configuration/compilation): parser, target keluaran, JSX, TypeScript, dan konfigurasi transformasi.
- [Bundling Configuration](https://swc.rs/docs/configuration/bundling): penggabungan berkas dan konfigurasi bundler.
- [Migrating from Babel](https://swc.rs/docs/migrating-from-babel): migrasi, kesetaraan transformasi, dan batas plugin.
- [Migrating from tsc](https://swc.rs/docs/migrating-from-tsc): pemrosesan per berkas, keterbatasan sistem tipe, dan opsi TypeScript terkait.
- [Next.js Compiler](https://nextjs.org/docs/architecture/nextjs-compiler): penggunaan SWC untuk transformasi dan minifikasi dalam Next.js.
- [Repositori resmi SWC](https://github.com/swc-project/swc): kode sumber, lisensi, dan dokumentasi proyek.
