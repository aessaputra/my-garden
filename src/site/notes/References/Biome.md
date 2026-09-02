---
{"dg-publish":true,"dg-path":"Biome.md","permalink":"/biome/","title":"Biome","hideInFiletree":true,"tags":["references","programming","javascript","typescript","performance"],"noteIcon":"","dg-note-properties":{"title":"Biome","category":"references","tags":["references","programming","javascript","typescript","performance"],"sources":["_raw/articles/biome-expanded.md"],"created":"2026-08-26","updated":"2026-08-26","confidence":"medium"}}
---

Biome adalah perangkat kerja berperforma tinggi untuk proyek web yang menyatukan pemformatan, linting, dan pengorganisasian impor dalam satu alat. Implementasinya ditulis dalam Rust dan dirancang untuk mengurangi kebutuhan akan beberapa konfigurasi terpisah seperti Prettier dan ESLint.

## Fungsi utama

Menurut [situs resmi Biome](https://biomejs.dev/), formatter Biome mendukung JavaScript, TypeScript, JSX, TSX, JSON, HTML, CSS, dan GraphQL dengan tingkat kompatibilitas yang diklaim mencapai 97 persen terhadap Prettier. Linter-nya menganalisis kode secara statis dan pada dokumentasi saat ini menyediakan 526 aturan yang berasal dari ESLint, TypeScript ESLint, dan sumber lain. Perintah `check` menjalankan pemformatan, linting, dan pengorganisasian impor dalam satu proses.

Biome dapat dipasang sebagai dependensi pengembangan atau dijalankan sebagai berkas eksekusi mandiri tanpa Node.js. Konfigurasi tidak wajib untuk penggunaan awal, tetapi perintah `init` dapat membuat `biome.json` agar aturan dan gaya disesuaikan dengan proyek.

```bash
npm install --save-dev --save-exact @biomejs/biome
npx @biomejs/biome init
npx @biomejs/biome check --write
```

Perintah `format --write` memformat berkas. `lint --write` menjalankan linter dan menerapkan perbaikan aman, sedangkan `check --write` menjalankan seluruh pemeriksaan utama sekaligus. Untuk CI, [panduan memulai Biome](https://biomejs.dev/guides/getting-started/) menyediakan perintah `biome ci`, yang bekerja seperti `check` tetapi disesuaikan untuk lingkungan integrasi berkelanjutan.

## Arsitektur dan performa

Biome menggunakan Rust dan arsitektur yang terinspirasi oleh `rust-analyzer`. [Dokumentasi arsitekturnya](https://biomejs.dev/internals/architecture/) menjelaskan bahwa parser menyimpan kode sebagai CST yang mempertahankan spasi, tab, komentar, dan informasi sintaks lain. Parser juga memakai mekanisme pemulihan dan node `Bogus` ketika menemukan kode yang rusak. Daemon yang berjalan di latar belakang melayani permintaan dari editor dan CLI melalui arsitektur klien-server.

Benchmark resmi mengklaim formatter Biome sekitar 35 kali lebih cepat daripada Prettier ketika memproses 171.127 baris dalam 2.104 berkas pada Intel Core i7-1270P. [Artikel OpenReplay](https://blog.openreplay.com/biome-toolchain-modern-frontend-projects/) melaporkan angka berbeda, yaitu 25 kali lebih cepat untuk pemformatan dan 15 kali lebih cepat untuk linting dibandingkan Prettier dan ESLint. Metodologi dan beban kerja benchmark dapat berbeda, sehingga tim tetap perlu menguji repositorinya sendiri.

Sejak Biome v2, aturan yang membutuhkan informasi tingkat proyek dapat mengaktifkan Scanner untuk membangun graf modul dan menginferensi tipe. [Dokumentasi linter Biome](https://biomejs.dev/linter/) melaporkan bahwa pada pengujian internal sekitar 2.000 berkas, waktu meningkat dari kurang lebih 800 milidetik tanpa Scanner menjadi sekitar 2 detik dengan Scanner. Pada sekitar 5.000 berkas, waktunya meningkat dari sekitar 1 detik menjadi 8 detik. Scanner hanya aktif ketika aturan domain proyek memerlukannya.

## Linting dan perbaikan kode

Aturan linter dikelompokkan menurut tujuan, antara lain aksesibilitas, kompleksitas, kebenaran, performa, keamanan, gaya, dan pola mencurigakan. Aturan rekomendasi aktif secara bawaan ketika pengguna menjalankan `lint` atau `check` tanpa konfigurasi khusus. Biome memisahkan keputusan format dari aturan lint, sehingga linter tidak menyediakan aturan yang hanya memeriksa gaya pemformatan.

Perbaikan dibagi menjadi kategori aman dan tidak aman. Perbaikan aman dijamin tidak mengubah semantik dan dapat diterapkan dengan `--write`. Perbaikan tidak aman mungkin mengubah perilaku program, sehingga memerlukan `--write --unsafe` dan peninjauan manual. Integrasi editor berbasis LSP dapat menampilkan diagnostik, tindakan perbaikan, dan opsi penekanan aturan langsung di editor.

## Migrasi dari ESLint dan Prettier

[Panduan migrasi resmi](https://biomejs.dev/guides/migrate-eslint-prettier/) menyediakan `biome migrate eslint --write` dan `biome migrate prettier --write` untuk menerjemahkan konfigurasi yang kompatibel. Migrasi ESLint dapat membaca konfigurasi lama maupun *flat config*, menangani sejumlah konfigurasi bersama dan plugin, serta memindahkan `.eslintignore`. Hasilnya tidak selalu identik karena sebagian opsi aturan tidak tersedia atau sengaja diterapkan secara berbeda oleh Biome.

Migrasi Prettier membaca konfigurasi yang ada dan menerjemahkan pilihan seperti gaya indentasi, tanda kutip, koma akhir, dan *override* berkas. Konfigurasi Prettier berbasis JavaScript memerlukan Node.js saat migrasi, sementara format JSON5, TOML, dan YAML tidak didukung oleh subperintah tersebut.

Formatter Biome berusaha mendekati keluaran Prettier, tetapi beberapa perbedaan bersifat disengaja. [Dokumentasi perbedaannya dengan Prettier](https://biomejs.dev/formatter/differences-with-prettier/) menyebut parser Biome lebih ketat untuk sejumlah sintaks JavaScript dan TypeScript yang tidak valid. Dalam kondisi tersebut, node yang rusak dapat dicetak apa adanya untuk menghindari perubahan semantik yang keliru.

## Dukungan bahasa dan batasan

[Tabel dukungan bahasa Biome](https://biomejs.dev/internals/language-support/) mencantumkan dukungan penuh untuk parsing, pemformatan, dan linting pada JavaScript, TypeScript, JSX, TSX, JSON, JSONC, CSS, dan GraphQL. Dukungan HTML, SVG, Vue, Svelte, dan Astro masih ditandai eksperimental pada bagian tertentu. SCSS, YAML, dan Markdown masih dalam proses untuk parsing atau pemformatan dan belum memiliki linting. Dukungan Vue, Svelte, dan Astro tersedia sejak Biome 2.3, tetapi dukungan penuh perlu diaktifkan secara eksplisit dan masih dapat menghasilkan positif palsu.

Keterbatasan lain terletak pada ekosistem aturan dan plugin. OpenReplay menilai Biome memiliki ekosistem aturan yang lebih kecil, konfigurasi berbasis JSON, dan ekstensibilitas yang lebih terbatas daripada ESLint. Migrasi penuh paling sesuai untuk proyek JavaScript atau TypeScript yang kebutuhannya sudah tercakup oleh aturan dan bahasa Biome. Pendekatan hibrida tetap masuk akal jika proyek bergantung pada plugin ESLint khusus atau format berkas yang belum didukung.

## Integrasi editor, Git, dan CI

Biome menyediakan ekstensi pihak pertama untuk editor utama serta ekstensi komunitas untuk editor lain. Integrasi LSP dapat menjalankan perbaikan aman saat berkas disimpan melalui tindakan `source.fixAll.biome`. Untuk konsistensi tim, `biome ci` dapat ditambahkan ke GitHub Actions atau GitLab CI, sedangkan pemeriksaan berkas bertahap dapat dipasang pada *pre-commit hook*.

Video Better Stack ["The EASIEST Way To Switch From ESLint & Prettier to Biome"](https://www.youtube.com/watch?v=lEkXbneUnWg) membahas Ultracite sebagai lapisan konfigurasi yang menggunakan Biome untuk linting dan pemformatan. Ultracite menyediakan aturan berpendapat, konfigurasi editor, dukungan *pre-commit hook*, berkas aturan untuk asisten AI, dan server MCP. Fitur tersebut berasal dari Ultracite, bukan fitur inti Biome, sehingga keduanya perlu dibedakan ketika menilai kebutuhan proyek.

## Kapan Biome tepat digunakan

Biome paling sesuai untuk proyek JavaScript dan TypeScript yang mengutamakan waktu umpan balik singkat, konfigurasi terpadu, serta aturan bawaan yang memadai. Proyek baru dan monorepo dengan jenis berkas yang seragam dapat memperoleh manfaat tanpa membawa seluruh konfigurasi ESLint dan Prettier lama.

Tim perlu mempertahankan ESLint, Prettier, atau alat lain jika bergantung pada plugin khusus, konfigurasi JavaScript dinamis, atau bahasa yang belum didukung penuh. Migrasi sebaiknya dilakukan pada cabang terpisah, kemudian keluaran format, aturan yang tidak terpetakan, waktu CI, dan perilaku perbaikan otomatis diperiksa sebelum konfigurasi lama dihapus.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Vite\|Vite]]
- [[References/esbuild\|esbuild]]
- [[References/Package Managers\|Package Managers]]

## Sumber

- [Biome: The All-in-One Toolchain for Modern Frontend Projects](https://blog.openreplay.com/biome-toolchain-modern-frontend-projects/): perbandingan dengan ESLint dan Prettier, cara penggunaan, integrasi, serta batasan.
- [The EASIEST Way To Switch From ESLint & Prettier to Biome](https://www.youtube.com/watch?v=lEkXbneUnWg): demonstrasi Biome melalui Ultracite, integrasi editor, Git hooks, aturan AI, dan MCP.
- [Biome, toolchain of the web](https://biomejs.dev/): fitur utama, jumlah aturan, kompatibilitas formatter, dan benchmark resmi.
- [Getting Started](https://biomejs.dev/guides/getting-started/): instalasi, konfigurasi, perintah CLI, editor, dan CI.
- [Language support](https://biomejs.dev/internals/language-support/): status dukungan bahasa dan kerangka kerja.
- [Migrate from ESLint and Prettier](https://biomejs.dev/guides/migrate-eslint-prettier/): mekanisme dan keterbatasan migrasi konfigurasi.
- [Differences with Prettier](https://biomejs.dev/formatter/differences-with-prettier/): perbedaan keluaran dan perilaku parser.
- [Linter](https://biomejs.dev/linter/): aturan, perbaikan aman dan tidak aman, integrasi editor, domain, serta dampak Scanner.
- [Architecture](https://biomejs.dev/internals/architecture/): Scanner, CST, pemulihan parser, dan daemon.
