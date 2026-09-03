---
{"dg-publish":true,"dg-path":"Svelte.md","permalink":"/svelte/","title":"Svelte","hideInFiletree":true,"tags":["references","frameworks","javascript","typescript","ui","architecture","performance","ssr"],"noteIcon":"","dg-note-properties":{"title":"Svelte","category":"references","tags":["references","frameworks","javascript","typescript","ui","architecture","performance","ssr"],"sources":["_raw/articles/svelte-expanded.md"],"created":"2026-08-30","updated":"2026-08-30","confidence":"high"}}
---

Svelte adalah framework untuk membangun antarmuka pengguna web. Pengembang menulis komponen deklaratif dengan [[References/HTML\|HTML]], [[References/CSS\|CSS]], dan [[References/JavaScript\|JavaScript]] atau [[typescript\|TypeScript]], lalu compiler Svelte mengubahnya menjadi JavaScript yang teroptimasi. Svelte dapat digunakan untuk komponen mandiri maupun aplikasi web lengkap melalui SvelteKit.

## Pendekatan berbasis compiler

Perbedaan utama Svelte terletak pada waktu dan cara kerja framework. Banyak pekerjaan analisis dilakukan saat build, kemudian compiler menghasilkan kode imperatif yang memperbarui DOM sesuai perubahan state. Pendekatan ini berbeda dari framework yang mengandalkan proses rekonsiliasi virtual DOM di browser.

Ketiadaan virtual DOM tidak berarti setiap aplikasi Svelte selalu lebih cepat. Performa akhir tetap dipengaruhi oleh ukuran aplikasi, pola pembaruan state, jaringan, aset, rendering, dan kode yang ditulis pengembang. Klaim yang lebih tepat adalah bahwa Svelte memakai hasil kompilasi untuk menghasilkan operasi DOM yang terarah dan mengurangi kebutuhan kerja rekonsiliasi saat runtime.

## Komponen dan sintaks

Komponen Svelte ditulis dalam berkas `.svelte`, yaitu superset HTML yang dapat memuat blok `<script>`, markup, dan `<style>`. Ketiga bagian tersebut bersifat opsional. Blok `<script>` menerima JavaScript atau TypeScript melalui atribut `lang="ts"`, sedangkan variabel tingkat atas dapat digunakan langsung dari markup.

Model berkas tunggal ini menempatkan struktur, perilaku, dan style komponen dalam satu unit. Komponen menjadi blok penyusun aplikasi dan dapat dikomposisikan untuk membentuk antarmuka yang lebih besar. Sintaks template juga menyediakan blok kondisi, iterasi, penanganan promise, event, binding, serta directive untuk style, class, action, transisi, dan animasi.

## State dan reaktivitas

Svelte 5 menggunakan runes untuk menyatakan perilaku reaktif. Runes adalah bagian dari sintaks Svelte yang mengarahkan compiler dan dapat digunakan dalam berkas `.svelte`, `.svelte.js`, serta `.svelte.ts`. Contohnya, `$state` mendeklarasikan state reaktif, `$derived` membentuk nilai turunan, dan `$effect` menjalankan efek ketika dependensinya berubah.

Ketika `$state` menerima array atau objek sederhana, Svelte membuat proxy reaktif secara mendalam. Pembacaan dan perubahan properti dapat dilacak sehingga operasi seperti `array.push(...)` memicu pembaruan yang terperinci. Untuk state bersama, pengembang dapat memakai runes pada modul JavaScript atau TypeScript, context API, atau stores. Pilihan tersebut bergantung pada cakupan data dan kebutuhan interoperabilitas.

## Styling, transisi, dan animasi

CSS di dalam blok `<style>` dicakup ke komponen secara bawaan. Svelte menerapkannya dengan kelas berbasis hash pada elemen yang terpengaruh, termasuk penyesuaian terhadap specificity selector dan nama `@keyframes`. Style global tetap tersedia ketika suatu aturan memang perlu berlaku di luar batas komponen.

Directive `transition:` menjalankan transisi ketika elemen masuk atau keluar dari DOM akibat perubahan state. Transisi bersifat lokal secara bawaan, dapat dibalik ketika masih berlangsung, dan dapat memakai fungsi bawaan maupun fungsi khusus. Transisi Svelte menggunakan Web Animations API, sehingga aplikasi perlu menangani preferensi reduced motion melalui dukungan yang disediakan Svelte, bukan hanya mengandalkan perubahan durasi animasi CSS global.

## Svelte dan SvelteKit

Svelte berfokus pada komponen UI, sedangkan SvelteKit menyediakan lapisan aplikasi. SvelteKit memakai [[References/Vite\|Vite]] dan mencakup routing, pemuatan data, optimasi build, dukungan offline, preloading, optimasi gambar, dan pilihan rendering. Bagian aplikasi dapat dirender di server melalui server-side rendering, di browser melalui client-side rendering, atau saat build melalui prerendering.

Pemisahan ini penting ketika memilih teknologi. Svelte saja dapat mencukupi untuk widget atau komponen mandiri. Aplikasi dengan halaman, routing, endpoint server, pengambilan data, dan deployment biasanya lebih tepat dibangun dengan SvelteKit.

## Kelebihan dan batasan

Svelte mengurangi sebagian kode penghubung dengan menyatukan komponen, reaktivitas, dan scoped CSS dalam sintaks yang terintegrasi. Binding dan transisi juga tersedia sebagai bagian dari sintaks komponen. Hasil kompilasinya dirancang sebagai JavaScript yang ringkas dan teroptimasi, tetapi ukuran bundle dan kecepatan aplikasi tidak dapat disimpulkan hanya dari pilihan framework.

Pendekatan compiler juga membawa konsekuensi. Tim perlu memahami sintaks khusus seperti runes dan directive, batas reaktivitas proxy, perbedaan antara Svelte dan SvelteKit, serta perubahan dari sintaks legacy ketika memelihara proyek lama. Pemilihan Svelte sebaiknya mempertimbangkan kebutuhan rendering, ekosistem pustaka, pengalaman tim, integrasi yang diperlukan, dan biaya pemeliharaan, bukan hanya kesan bahwa sintaksnya singkat atau bahwa framework ini tidak memakai virtual DOM.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[typescript\|TypeScript]]
- [[References/HTML\|HTML]]
- [[References/CSS\|CSS]]
- [[References/React\|React]]
- [[References/Vue.js\|Vue.js]]
- [[References/Angular\|Angular]]
- [[References/Vite\|Vite]]
- [[References/Pick a Framework\|Pick a Framework]]

## Sumber

- [Overview](https://svelte.dev/docs/svelte/overview): compiler Svelte, komponen deklaratif, keluaran JavaScript, dan hubungan dengan SvelteKit.
- [.svelte files](https://svelte.dev/docs/svelte/svelte-files): struktur komponen, JavaScript atau TypeScript, markup, dan scoped CSS.
- [What are runes?](https://svelte.dev/docs/svelte/what-are-runes): runes sebagai sintaks yang mengarahkan compiler Svelte.
- [$state](https://svelte.dev/docs/svelte/$state): state reaktif, proxy mendalam, dan pembaruan terperinci.
- [Scoped styles](https://svelte.dev/docs/svelte/scoped-styles): pencakupan CSS, specificity, dan keyframes berbasis hash.
- [transition:](https://svelte.dev/docs/svelte/transition): transisi elemen, Web Animations API, dan reduced motion.
- [Svelte 3: Rethinking reactivity](https://svelte.dev/blog/svelte-3-rethinking-reactivity): pendekatan build time, kode imperatif, dan perbedaan dengan virtual DOM.
- [Introduction to SvelteKit](https://svelte.dev/docs/kit): routing, pemuatan data, rendering, optimasi build, dan deployment aplikasi.
