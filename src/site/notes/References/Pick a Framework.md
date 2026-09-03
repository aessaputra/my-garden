---
{"dg-publish":true,"dg-path":"Pick a Framework.md","permalink":"/pick-a-framework/","title":"Pick a Framework","hideInFiletree":true,"tags":["references","javascript","frameworks","architecture","performance","ui"],"noteIcon":"","dg-note-properties":{"title":"Pick a Framework","category":"synthesis","tags":["references","javascript","frameworks","architecture","performance","ui"],"sources":["_raw/articles/javascript-framework-comparison-fireship-transcript.md","_raw/articles/javascript-framework-updates-2024-fireship-transcript.md"],"created":"2026-08-25","updated":"2026-08-30","confidence":"medium"}}
---

Framework web adalah perangkat pengembangan yang menyediakan pustaka, struktur, aturan, dan alat untuk membangun aplikasi. Setiap framework menawarkan fitur yang berbeda sesuai kebutuhan proyek. Contoh yang populer meliputi [[References/React\|React]], [[References/Angular\|Angular]], dan [[References/Vue.js\|Vue.js]], masing-masing dengan keunggulan dan kemampuan tersendiri.

Framework frontend menghubungkan *state* aplikasi dengan tampilan secara deklaratif. Tanpa framework, pengembang harus memilih elemen DOM, memasang event listener, membuat atau menghapus node, dan menjaga data tetap sinkron dengan UI secara manual. Pendekatan tersebut cukup untuk interaksi kecil, tetapi kompleksitasnya meningkat saat aplikasi membutuhkan komponen, routing, animasi, lifecycle, dan state bersama. Konsep dasarnya tetap berakar pada [[References/JavaScript\|JavaScript]], [[References/HTML\|HTML]], dan [[References/State\|State]].

Dua video Fireship memperlihatkan perubahan sudut pandang dalam beberapa tahun. Video perbandingan lebih lama menilai pengalaman membangun aplikasi todo yang sama dengan sepuluh pendekatan. Video tahun 2024 menunjukkan arah evolusi berikutnya: compiler, signals, rendering parsial, pembaruan DOM yang lebih terarah, dan lebih sedikit JavaScript di sisi klien.

## Masalah yang diselesaikan framework

Aplikasi todo sederhana sudah memerlukan beberapa hal yang saling berkaitan:

- *State* untuk menyimpan daftar item.
- Data binding agar perubahan state terlihat pada UI.
- Event handling untuk pengiriman formulir.
- Lifecycle untuk memuat data dari `localStorage`.
- Struktur komponen agar kode dapat dikembangkan tanpa menjadi kumpulan manipulasi DOM imperatif.

Vanilla JavaScript tidak salah, dan tetap wajib dipahami. Kelemahannya muncul ketika tim harus membangun sendiri pola sinkronisasi state dan UI yang sudah disediakan framework.

## Perbandingan pendekatan

| Pendekatan         | Model UI dan reaktivitas                                          | Kekuatan utama                                                    | Pertimbangan                                                                                 |
| ------------------ | ----------------------------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Vanilla JavaScript | Manipulasi DOM imperatif                                          | Tanpa dependensi dan cocok untuk interaksi kecil                  | Sinkronisasi state, UI, lifecycle, dan routing harus dirancang sendiri                       |
| [[References/React\|React]]          | JSX, komponen fungsi, hooks, rekonsiliasi                         | Ekosistem besar, pasar kerja luas, fleksibel                      | Banyak keputusan arsitektur diserahkan kepada tim; hooks tertentu dapat membingungkan pemula |
| Angular            | Template, class TypeScript, dependency injection, tooling terpadu | Konvensi kuat dan cocok untuk tim besar                           | Learning curve dan ukuran konsep awal lebih besar                                            |
| [[References/Vue.js\|Vue.js]]         | Template deklaratif, directives, Single-File Components           | Mudah diadopsi bertahap dan ekosistem resmi cukup lengkap         | Tetap perlu keputusan tentang skala, rendering, dan state global                             |
| [[References/Svelte\|Svelte]]         | Compiler dan sintaks reaktif dalam komponen `.svelte`             | Boilerplate kecil dan dekat dengan JavaScript biasa               | Ekosistem dan peluang kerja lebih kecil dibanding React                                      |
| SolidJS            | JSX dengan fine-grained signals, tanpa virtual DOM                | Model mirip React dengan pembaruan DOM terarah                    | Komunitas lebih kecil                                                                        |
| Lit                | Class dan template literal yang menghasilkan Web Components       | Komponen standar lintas framework                                 | Lebih khusus untuk custom elements daripada aplikasi penuh                                   |
| Stencil            | TypeScript, decorators, JSX, dikompilasi menjadi Web Components   | Cocok untuk design system lintas framework                        | Abstraksinya menggabungkan beberapa gaya dan dapat terasa berat                              |
| Alpine.js          | Atribut reaktif langsung pada HTML                                | Ringan untuk menambah interaktivitas pada halaman server-rendered | Bukan pengganti ideal untuk SPA kompleks                                                     |
| Mithril            | Virtual DOM dengan UI yang ditulis melalui fungsi JavaScript      | Ringan dan cepat                                                  | Sintaks UI murni JavaScript dapat terasa kurang natural                                      |

Tidak ada framework yang mutlak terbaik. Pilihan bergantung pada kebutuhan aplikasi, kemampuan tim, ekosistem, strategi rendering, target performa, dan biaya pemeliharaan.

## Arah evolusi pada 2024

### Optimasi berpindah ke compiler

React Compiler mengotomatisasi sebagian memoisasi yang sebelumnya sering ditulis memakai `useMemo` dan `useCallback`. Svelte dan SolidJS lebih dahulu menunjukkan pendekatan compile-time atau fine-grained reactivity. Vue Vapor Mode juga mengeksplorasi kompilasi yang menghindari virtual DOM dan memperbarui DOM nyata secara terarah.

Arah ini tidak berarti virtual DOM langsung usang. Intinya, framework semakin banyak menganalisis kode saat build agar runtime melakukan lebih sedikit pekerjaan.

### Signals menjadi pola lintas framework

SolidJS memakai signals sebagai dasar reaktivitas. Svelte 5 memperkenalkan runes yang didukung signals, sedangkan Angular 18 menambahkan dukungan zoneless change detection dengan signals. Pola ini memungkinkan ketergantungan state dilacak lebih presisi daripada merender ulang area UI yang luas.

### Rendering menjadi campuran

Next.js 15 memperkenalkan Partial Prerendering untuk menggabungkan konten statis dan dinamis dalam satu halaman serta satu permintaan HTTP. TanStack Start, SolidStart, Astro Actions, dan HonoX memperlihatkan bahwa batas antara framework frontend, server rendering, form actions, dan backend semakin tipis.

### Ekosistem bergerak ke interoperabilitas

Mitosis menulis komponen dalam subset JSX lalu mengompilasinya ke beberapa framework. Lit dan Stencil menggunakan standar Web Components untuk tujuan serupa. JSR dari tim Deno menambahkan registry yang dapat mentranspilasi TypeScript dan menghasilkan dokumentasi API, melengkapi peran [[References/Package Managers\|package manager]] dan registry dalam ekosistem JavaScript.

### Minimalisme tetap relevan

htmx 2.0 dan jQuery 4.0 mewakili arah berbeda dari framework full-stack. htmx mempertahankan pendekatan HTML-first, sedangkan jQuery 4.0 lebih banyak membuang kode lama dan bloat daripada menambah abstraksi baru. Untuk halaman yang tidak membutuhkan SPA, Alpine.js dan pendekatan server-rendered juga tetap masuk akal.

## Panduan memilih

1. Gunakan Vanilla JavaScript atau Alpine.js untuk interaktivitas kecil pada halaman yang sudah ada.
2. Pilih React ketika ekosistem, tenaga kerja, dan fleksibilitas menjadi prioritas.
3. Pilih Angular ketika tim membutuhkan struktur seragam dan perangkat resmi yang lengkap.
4. Pilih Vue ketika ingin adopsi bertahap, template dekat dengan HTML, dan ekosistem resmi yang seimbang.
5. Pertimbangkan Svelte atau SolidJS ketika compiler, signals, ukuran runtime, dan kesederhanaan reaktivitas lebih penting daripada ukuran komunitas.
6. Gunakan Lit atau Stencil ketika keluaran utamanya harus berupa Web Components lintas framework.
7. Evaluasi meta-framework dan strategi rendering secara terpisah dari library komponen. Next.js, SolidStart, TanStack Start, Astro, dan HonoX menyelesaikan kebutuhan aplikasi yang lebih luas daripada UI saja.

## Batasan sumber

Video perbandingan memakai statistik dan tooling sekitar 2021 sampai 2022. Create React App, angka unduhan, jumlah GitHub stars, dan posisi komunitas pada masa itu tidak boleh dianggap sebagai kondisi terkini. Video 2024 juga membahas beberapa fitur yang saat itu masih release candidate, eksperimental, atau baru diumumkan. Klaim bahwa framework lain menjadi “obsolete” merupakan humor presenter, bukan rekomendasi migrasi.

## Lihat juga

- [[References/JavaScript\|JavaScript]]: fondasi bahasa dan API browser.
- [[References/React\|React]]: pustaka UI berbasis komponen dan React Compiler.
- [[References/Vue.js\|Vue.js]]: framework progresif dengan template dan sistem reaktivitas.
- [[References/State\|State]]: data yang berubah dan memengaruhi tampilan.
- [[References/HTML\|HTML]]: markup yang menjadi basis template dan pendekatan HTML-first.
- [[References/Package Managers\|Package Managers]]: instalasi dependensi dan registry paket.

## Sumber

- [15 JavaScript Framework Updates Every Developer Should Know](https://www.youtube.com/watch?v=466U-2D86bc): Code Report Fireship, 28 Mei 2024, tentang React Compiler, Partial Prerendering, signals, compiler, dan meta-framework baru.
- [I built the same app 10 times](https://www.youtube.com/watch?v=cuHDQhDhvPE): perbandingan implementasi aplikasi todo dengan Vanilla JavaScript, React, Angular, Vue, Svelte, Lit, Stencil, SolidJS, Alpine.js, dan Mithril.
