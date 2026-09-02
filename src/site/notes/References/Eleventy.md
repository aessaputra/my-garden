---
{"dg-publish":true,"dg-path":"Eleventy.md","permalink":"/eleventy/","title":"Eleventy","hideInFiletree":true,"tags":["references","programming","javascript","frameworks","deployment","performance"],"noteIcon":"","dg-note-properties":{"title":"Eleventy","category":"references","tags":["references","programming","javascript","frameworks","deployment","performance"],"sources":["_raw/articles/eleventy-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

Eleventy, juga disebut 11ty, adalah static site generator berbasis [[References/JavaScript\|JavaScript]] yang mengubah template dan data menjadi berkas statis. Ia mendukung beberapa bahasa template dalam satu proyek, termasuk [[References/HTML\|HTML]], Markdown, JavaScript, Liquid, Nunjucks, dan WebC. Eleventy memberi kontrol langsung atas keluaran dan tidak menambahkan client-side JavaScript secara bawaan.

Karakter tersebut membuat Eleventy sesuai untuk blog, dokumentasi, situs pemasaran, arsip, dan situs berbasis data yang dapat dibangun sebelum deployment. Eleventy bukan framework full-stack atau single-page application framework. Backend request-time, autentikasi server, dan personalisasi per pengguna membutuhkan layanan atau arsitektur tambahan.

## Status versi

Pada 31 Agustus 2026, versi stabil terbaru adalah Eleventy `3.1.6`, sedangkan Eleventy 4 masih tersedia sebagai canary prerelease. Eleventy 3 memerlukan Node.js 18 atau lebih baru dan menambahkan dukungan ESM sebagai pilihan utama tanpa menghentikan dukungan CommonJS.

Versi 3 juga memindahkan sejumlah template language yang lebih jarang digunakan dari core ke plugin dan menghapus plugin Serverless serta Edge dari distribusi inti. Proyek lama perlu memeriksa upgrade helper dan release notes sebelum berpindah versi mayor, terutama bila bergantung pada template engine atau plugin yang dipindahkan.

## Model build

Eleventy membaca berkas template dari input directory, menggabungkannya dengan data, menjalankan template engine yang sesuai, lalu menulis hasil ke output directory. Input default adalah direktori proyek saat ini dan output default adalah `_site`.

Satu template biasanya menghasilkan satu halaman, tetapi pagination dapat menghasilkan banyak halaman dari satu array, object, collection, atau sumber data lain. `permalink` mengendalikan lokasi output, sedangkan layout membungkus isi template dengan struktur bersama.

```text
project/
├── content/
│   ├── index.md
│   └── posts/
├── _data/
├── _includes/
├── eleventy.config.js
└── _site/
```

Perintah build standar sudah menghasilkan build yang dianggap siap produksi oleh dokumentasi Eleventy. Tidak ada mode optimasi produksi internal yang berbeda dari mode pengembangan; variasi proyek biasanya diatur melalui environment variable, plugin, atau pipeline aset sendiri.

## Template language

Eleventy dapat memakai beberapa template language dalam satu proyek. HTML, Markdown, JavaScript, Liquid, Nunjucks, dan WebC termasuk pilihan utama. Dukungan untuk JSX, TypeScript, MDX, Handlebars, Mustache, EJS, HAML, Pug, dan Sass tersedia melalui plugin atau integrasi terkait versi.

Fleksibilitas ini memudahkan migrasi konten dan pemilihan sintaks per kebutuhan. Trade-off-nya adalah konsistensi proyek dapat menurun bila terlalu banyak template engine dipakai sekaligus. Filter, shortcode, escaping, asynchronous rendering, dan aturan include dapat berbeda antar-engine. Tim sebaiknya menetapkan pilihan utama dan membatasi variasi yang tidak diperlukan.

## Data Cascade

Eleventy menggabungkan data dari beberapa tingkat melalui Data Cascade. Prioritas tertinggi dimiliki computed data, disusul frontmatter template, template data file, directory data file, frontmatter layout, global data dari configuration API, dan global data file.

Model tersebut memungkinkan metadata bersama diwariskan dari direktori atau layout lalu ditimpa pada halaman tertentu. Object dan array digabung secara mendalam secara bawaan. Prefix `override:` dapat digunakan untuk mengganti nilai tertentu tanpa deep merge.

Sumber data dapat berupa JSON, JavaScript, frontmatter, atau format kustom. JavaScript data file juga dapat mengambil data dari API ketika build. Request jaringan perlu di-cache dan diberi penanganan kegagalan karena layanan eksternal yang lambat atau tidak tersedia dapat memperlambat atau menggagalkan build.

## Collections dan pagination

Collections mengelompokkan konten untuk dipakai kembali dalam template. Nilai `tags` pada frontmatter memasukkan halaman ke satu atau beberapa collection. Eleventy juga menyediakan collection `all`, sementara configuration API dapat membuat collection dengan filtering dan sorting khusus.

Pagination mengiterasi data dan membuat beberapa output dari satu template. Mekanisme ini dapat dipakai untuk daftar artikel per halaman, halaman kategori, atau satu halaman detail untuk setiap record data. Collection mengelompokkan konten; pagination menghasilkan halaman dari kelompok atau data tersebut. Keduanya berhubungan tetapi tidak saling menggantikan.

## Aset dan client-side JavaScript

Eleventy tidak membundel CSS, JavaScript browser, font, atau aset lain secara otomatis. Passthrough Copy menyalin berkas atau direktori terpilih ke output. Proyek dapat memakai bundler terpisah, plugin Bundle, transform HTML, atau pipeline lain jika memerlukan minification, hashing, code splitting, maupun pemrosesan CSS.

Pernyataan bahwa Eleventy menghasilkan situs tanpa JavaScript perlu dibaca sebagai default, bukan larangan. Eleventy tidak menyisipkan runtime JavaScript sendiri, tetapi pengembang tetap dapat mengirim script apa pun. Plugin `<is-land>` juga menyediakan pola partial hydration untuk komponen sisi klien. Performa halaman tetap bergantung pada HTML, CSS, gambar, font, script pihak ketiga, dan kode aplikasi yang benar-benar dikirim.

## Plugin Image dan Navigation

Plugin Image melakukan transformasi saat build untuk gambar raster maupun vector. Plugin ini dapat menghasilkan beberapa ukuran dan format, membuat `srcset`, menambahkan dimensi intrinsik, menggunakan cache, serta menghasilkan markup `img` atau `picture`. Pemrosesan gambar dapat menambah biaya build, sehingga cache dan mode optimasi saat development penting untuk proyek dengan banyak aset.

Plugin Navigation membentuk navigasi hierarkis dengan kedalaman tak terbatas dan mendukung breadcrumb. Halaman didaftarkan melalui data `eleventyNavigation`, lalu filter plugin menghasilkan tree atau markup navigasi. Plugin ini berguna, tetapi navigasi bukan kemampuan yang otomatis muncul hanya dengan memasang Eleventy core.

## Development server dan incremental build

Opsi `--serve` menjalankan development server dengan hot reload. Opsi `--watch` membangun ulang ketika berkas berubah tanpa menjalankan server. Eleventy juga dapat mengawasi target tambahan dan dependensi JavaScript pada template, data file, serta configuration file.

Untuk proyek besar, incremental build dapat mengurangi pekerjaan saat development. Hasilnya bergantung pada deklarasi dependensi yang benar. Template yang membaca collection atau sumber eksternal tanpa hubungan yang diketahui Eleventy dapat membutuhkan konfigurasi tambahan agar perubahan tidak menghasilkan output usang.

## Deployment dan performa

Output `_site` dapat diterbitkan ke static hosting seperti [[References/GitHub Pages\|GitHub Pages]], Cloudflare Pages, Netlify, Vercel, atau server web biasa. Deployment pada subdirectory membutuhkan `pathPrefix` yang benar dan, untuk penulisan ulang URL dalam HTML, plugin HTML Base atau penanganan URL yang setara.

Tidak adanya runtime JavaScript bawaan memberi titik awal yang ringan, tetapi tidak menjamin situs cepat. Gambar yang tidak dioptimalkan, CSS besar, font, script pihak ketiga, dan markup yang buruk tetap dapat merusak performa. Build juga dapat melambat akibat transform mahal, pemrosesan gambar, collection besar, atau request API. Ukur output dengan [[References/Lighthouse\|Lighthouse]], profil build, cache operasi mahal, dan gunakan incremental build sesuai kebutuhan.

## Kapan Eleventy tepat digunakan

Eleventy tepat ketika proyek membutuhkan keluaran statis, kontrol HTML, pilihan template language, data cascade, collection, dan pipeline yang dapat dirakit sendiri. Ia sangat sesuai untuk tim yang menginginkan fondasi kecil tanpa runtime frontend wajib.

Gunakan alat lain ketika kebutuhan utama adalah state aplikasi browser yang kompleks, rendering dinamis per request, backend terpadu, atau konvensi full-stack yang lebih kuat. Dibanding [[References/Astro\|Astro]] atau [[References/VuePress\|VuePress]], Eleventy memberi kebebasan lebih besar atas template dan pipeline, tetapi lebih banyak keputusan mengenai theme, navigasi, search, aset, dan interaktivitas berada pada proyek.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/HTML\|HTML]]
- [[References/Astro\|Astro]]
- [[References/VuePress\|VuePress]]
- [[References/GitHub Pages\|GitHub Pages]]
- [[References/Lighthouse\|Lighthouse]]
- [[References/Web Hosting\|Web Hosting]]

## Sumber

- [Get Started](https://www.11ty.dev/docs/): instalasi, Node.js minimum, build dasar, output `_site`, development server, dan versi stabil.
- [Template Languages](https://www.11ty.dev/docs/languages/): bahasa template core dan bahasa yang memerlukan plugin.
- [Data Cascade](https://www.11ty.dev/docs/data-cascade/): sumber data, urutan prioritas, deep merge, dan override.
- [Collections](https://www.11ty.dev/docs/collections/): pengelompokan konten melalui tags dan collection khusus.
- [Pagination](https://www.11ty.dev/docs/pagination/): pembuatan beberapa halaman dari array, object, collection, dan data global.
- [Configuration](https://www.11ty.dev/docs/config/): direktori, template formats, path prefix, filter, shortcode, collection, dan plugin.
- [Passthrough File Copy](https://www.11ty.dev/docs/copy/): penyalinan aset ke output dan perilaku development server.
- [Watch and Serve](https://www.11ty.dev/docs/watch-serve/): hot reload, watch mode, target tambahan, dan dependensi JavaScript.
- [Plugins](https://www.11ty.dev/docs/plugins/): ekosistem plugin resmi, community plugin, dan plugin yang dihentikan.
- [Image](https://www.11ty.dev/docs/plugins/image/): transformasi gambar, format, ukuran, markup, dan cache.
- [Navigation](https://www.11ty.dev/docs/plugins/navigation/): navigasi hierarkis, breadcrumb, dan integrasi template.
- [Deployment](https://www.11ty.dev/docs/deployment/): build produksi, hosting statis, cache, dan GitHub Pages.
- [Eleventy v3.0.0 is now available](https://www.11ty.dev/blog/eleventy-v3/): status stabil Eleventy 3 dan arah ESM.
- [Eleventy v3.1.6](https://github.com/11ty/eleventy/releases/tag/v3.1.6): versi stabil terbaru, pemeliharaan dependensi, dan dukungan Node 26 pada CI.
