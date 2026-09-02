---
{"dg-publish":true,"dg-path":"React.md","permalink":"/react/","title":"React","hideInFiletree":true,"tags":["references","react","javascript","ui","architecture","performance","react-native","redux"],"noteIcon":"","dg-note-properties":{"title":"React","category":"references","tags":["references","react","javascript","ui","architecture","performance","react-native","redux"],"sources":["_raw/articles/react-expanded.md"],"created":"2026-08-25","updated":"2026-08-25"}}
---

React adalah pustaka [[References/JavaScript\|JavaScript]] dari Facebook, kini Meta, untuk membangun antarmuka pengguna, terutama aplikasi satu halaman atau *single-page app*. Berbeda dengan kerangka kerja penuh seperti [[References/Angular\|Angular]], React hanya menangani lapisan tampilan. Pengembang tetap bebas memilih perkakas lain untuk routing, pengambilan data, dan manajemen state. React gratis, *open source*, memakai lisensi MIT, serta dirawat oleh Meta bersama komunitas pengembang.

Jordan Walke mulai mengembangkan prototipe React bernama FaxJS di Facebook pada 2011. Prototipe ini dipakai pada fitur liking and commenting di News Feed. Pada 2012, tim Instagram membangun ulang situs mereka dengan React. Karena Instagram tidak memakai infrastruktur internal Facebook, React harus dilepaskan dari ketergantungan tersebut sebelum dapat dirilis ke publik. React resmi menjadi *open source* pada JSConf US bulan Mei 2013.

## Model komponen

Komponen adalah potongan antarmuka yang mandiri dan dapat digunakan kembali, mulai dari tombol dan formulir hingga seluruh halaman. Komponen modern umumnya berupa fungsi JavaScript yang menerima data lewat *props* dan mengembalikan JSX. JSX adalah sintaks mirip HTML yang dikompilasi menjadi panggilan fungsi JavaScript. Pendekatan ini membentuk pohon komponen yang dapat dibaca dan diurai berdasarkan tanggung jawab setiap bagian UI. Dasar penggunaannya juga dibahas dalam [[References/Fundamental React\|Fundamental React]].

Props bersifat read-only dan mengalir dari induk ke anak. Komponen juga dapat memiliki *state*, yakni memori internal untuk menyimpan nilai yang berubah akibat interaksi, misalnya isi kotak pencarian atau daftar barang di keranjang. Ketika state berubah, React menjalankan ulang fungsi komponen dan memperbarui tampilan sesuai nilai terbaru.

React menyediakan *hooks* seperti `useState` dan `useEffect` agar fungsi komponen dapat menyimpan state dan menjalankan efek samping tanpa memakai class. Dokumentasi "Thinking in React" merangkum proses perancangan UI menjadi lima langkah:

1. Uraikan mockup menjadi hierarki komponen.
2. Bangun versi statis.
3. Tentukan representasi state minimal.
4. Tempatkan state pada induk yang tepat.
5. Hubungkan aliran data.

## Virtual DOM dan rekonsiliasi

Ketika data berubah, React tidak langsung memanipulasi DOM peramban. React merender pohon UI secara virtual, membandingkan hasil baru dengan hasil sebelumnya melalui *diffing*, lalu menerapkan bagian yang berubah ke DOM asli. Pengembang cukup mendeskripsikan tampilan untuk setiap kondisi, sedangkan React menangani transisinya.

Sejak React 16 pada 2017, proses rekonsiliasi menggunakan arsitektur Fiber. Fiber memecah rendering menjadi unit kerja kecil yang dapat dijeda, dibatalkan, atau diberi prioritas. Arsitektur ini memungkinkan pembaruan yang kurang mendesak ditunda agar animasi dan respons input tetap lancar. Kemampuan tersebut menjadi dasar *concurrent rendering* pada React modern.

React Compiler memindahkan sebagian optimasi dari kode aplikasi ke tahap build. Saat versi beta diperkenalkan, tim React melaporkan bahwa hanya sekitar 8 persen pengembang yang aktif menerapkan memoisasi manual. Pada skenario uji mereka, memoisasi otomatis meningkatkan performa 31 sampai 46 persen. React Compiler 1.0 menganalisis aliran data komponen dan menerapkan memoisasi otomatis. `useMemo` dan `useCallback` tetap tersedia ketika pengembang memerlukan kontrol yang lebih presisi.

## Aliran data satu arah

React memakai *one-way data flow*. Data bergerak dari komponen induk ke anak melalui props, sedangkan perubahan dikomunikasikan kembali melalui fungsi callback. Setiap perubahan tampilan dapat ditelusuri ke sumber data tertentu, sehingga perilaku aplikasi lebih mudah diprediksi dan di-debug.

Jika dua komponen memerlukan state yang sama, state dipindahkan ke induk terdekat melalui teknik *lifting state up*, kemudian diturunkan kembali lewat props. Context dapat dipakai ketika data harus melewati banyak tingkat komponen tanpa *prop drilling*. Untuk logika state yang kompleks, reducer mengonsolidasikan pembaruan pada satu tempat. Konsep state yang lebih dasar tersedia di [[References/State\|State]].

## Ekosistem dan React 19

Redux lama menjadi pilihan umum untuk manajemen state global. Tim Redux kini merekomendasikan Redux Toolkit, kumpulan API resmi yang menyederhanakan penulisan store, action, dan reducer.

React 19 dirilis stabil pada Desember 2024. Versi ini memperkenalkan Actions untuk menangani pengiriman formulir, status pending, error, dan pembaruan optimistis. React 19 juga menstabilkan React Server Components, yang memungkinkan komponen dirender di server sebelum hasilnya dikirim ke peramban. Kerangka kerja berbasis React seperti Next.js memakai kemampuan tersebut untuk arsitektur aplikasi full-stack.

## React Native

Prinsip komponen React diterapkan pada aplikasi mobile melalui [[References/Tutorial React Native\|React Native]]. Pengembang dapat memakai JavaScript dan React untuk membangun aplikasi iOS dan Android, sambil tetap menggunakan kode native ketika API perangkat atau kebutuhan performa memerlukannya.

Semua aplikasi mobile Shopify dibangun dengan React Native. Microsoft memakainya pada Office dan Outlook, sedangkan Meta memakainya pada Facebook Marketplace dan Ads Manager. Setelah lima tahun bermigrasi, Shopify melaporkan waktu muat layar di bawah 500 milidetik pada persentil ke-75 dan tingkat sesi bebas crash di atas 99,9 persen. Microsoft, Amazon, Tesla, dan Coinbase juga menggunakan atau berkontribusi pada ekosistem React Native.

## Popularitas

Stack Overflow Developer Survey 2025 menempatkan React sebagai teknologi web kedua yang paling banyak dipakai, dengan 44,7 persen responden. React berada di bawah Node.js dan di atas Angular serta Vue.js. Di kalangan pengembang profesional, angka penggunaannya mencapai sekitar 46,9 persen. State of JavaScript 2024 mencatat React dipakai oleh lebih dari 80 persen responden dan menempati posisi teratas di antara kerangka frontend.

Basis pengguna yang besar memberi React dokumentasi luas, pustaka pihak ketiga yang matang, banyak diskusi teknis, dan pasar kerja yang mapan. Model komponen yang jelas, optimasi melalui Fiber dan React Compiler, serta dukungan lintas platform membuat React tetap relevan untuk proyek web dan mobile.

## Lihat juga

- [[References/JavaScript\|JavaScript]]: bahasa yang digunakan React.
- [[References/Fundamental React\|Fundamental React]]: dasar komponen, JSX, props, dan state.
- [[References/Tutorial React Native\|Tutorial React Native]]: penerapan konsep React pada aplikasi mobile.
- [[References/State\|State]]: pengelolaan data yang berubah seiring waktu.
- [[References/Performa di React Native\|Performa di React Native]]: karakteristik performa pada lingkungan mobile.

## Sumber

- [React Official Documentation](https://react.dev): pengantar resmi dan panduan memulai.
- [React Compiler Beta Release](https://react.dev/blog/2024/10/21/react-compiler-beta-release): data adopsi memoisasi manual dan hasil uji React Compiler.
- [React Compiler 1.0 di InfoQ](https://www.infoq.com/news/2025/12/react-compiler-meta): status stabil dan cara kerja compiler.
- [React Compiler Introduction](https://react.dev/learn/react-compiler/introduction): optimasi otomatis dan penggunaan `useMemo` serta `useCallback`.
- [Our First 50,000 Stars](https://legacy.reactjs.org/blog/2016/09/28/our-first-50000-stars.html): sejarah awal React, FaxJS, dan Instagram.
- [React di Wikipedia](https://en.wikipedia.org/wiki/React_(software)): riwayat rilis, lisensi, dan pengelolaan proyek.
- [React: Facebook's Functional Turn on Writing Javascript](https://cacm.acm.org/practice/react/): penggunaan awal React pada News Feed.
- [Stack Overflow Developer Survey 2025](https://survey.stackoverflow.co/2025/technology): statistik penggunaan React.
- [React Native Showcase](https://reactnative.dev/showcase): contoh penerapan React Native di perusahaan.
- [Your First Component](https://react.dev/learn/your-first-component): konsep komponen React.
- [State: A Component's Memory](https://react.dev/learn/state-a-components-memory): definisi state dan `useState`.
- [Managing State](https://react.dev/learn/managing-state): lifting state, Context, dan reducer.
- [Render and Commit](https://react.dev/learn/render-and-commit): tahapan pembaruan tampilan.
- [React v16.0](https://legacy.reactjs.org/blog/2017/09/26/react-v16.0.html): pengenalan arsitektur Fiber.
- [React v19](https://react.dev/blog/2024/12/05/react-19): Actions dan React Server Components.
- [Thinking in React](https://react.dev/learn/thinking-in-react): proses membangun hierarki komponen dan state.
- [Redux Toolkit](https://redux-toolkit.js.org): rekomendasi resmi untuk penggunaan Redux.
- [React Fiber Architecture](https://github.com/acdlite/react-fiber-architecture): penjelasan rekonsiliasi dan incremental rendering.
- [The State of JavaScript 2024](https://www.i-programmer.info/news/167-javascript/17709-the-state-of-javascript-2024.html): statistik penggunaan React di survei State of JS.
- [Five years of React Native at Shopify](https://shopify.engineering/five-years-of-react-native-at-shopify): hasil migrasi dan metrik aplikasi Shopify.
