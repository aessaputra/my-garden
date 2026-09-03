---
{"dg-publish":true,"dg-path":"Prettier.md","permalink":"/prettier/","title":"Prettier","hideInFiletree":true,"tags":["references","programming","javascript","typescript"],"noteIcon":"","dg-note-properties":{"title":"Prettier","category":"references","tags":["references","programming","javascript","typescript"],"sources":["_raw/articles/prettier-expanded.md"],"created":"2026-08-26","updated":"2026-09-03","confidence":"high"}}
---

Prettier adalah formatter kode berpendapat yang menerapkan tampilan konsisten pada banyak bahasa dan terintegrasi dengan sebagian besar editor. Tujuan utamanya adalah memindahkan keputusan format dari manusia ke alat otomatis, sehingga pembahasan gaya dalam tinjauan kode dapat dikurangi.

## Cara kerja

Prettier mengurai kode menjadi AST, mengabaikan sebagian besar gaya asli, lalu mencetak ulang struktur tersebut memakai aturannya sendiri. Proses pencetakan mempertimbangkan `printWidth` untuk menentukan kapan ekspresi perlu dibungkus, tetapi nilai tersebut merupakan sasaran perkiraan, bukan batas keras panjang baris.

Arsitektur plugin memisahkan parser dan printer. Parser mengubah teks menjadi AST, sedangkan printer mengubah AST menjadi representasi perantara bernama Doc sebelum dirender menjadi string sesuai opsi seperti `printWidth`. Mekanisme yang sama dapat memformat bahasa tertanam, misalnya CSS dalam JavaScript atau blok kode dalam Markdown.

Prettier mempertahankan struktur program dan berfokus pada perubahan yang tidak memengaruhi AST. Namun, Prettier bukan alat untuk menemukan variabel tidak terpakai, global implisit, atau masalah kualitas kode lain. Dokumentasi resmi menyarankan penggunaan Prettier untuk format dan linter untuk menemukan bug.

## Dukungan bahasa

Paket inti menyediakan parser untuk JavaScript, Flow, TypeScript, CSS, SCSS, Less, JSON, JSON5, JSONC, GraphQL, Markdown, MDX, HTML, Vue, Angular, Lightning Web Components, MJML, Handlebars, dan YAML. Prettier menginferensi parser dari ekstensi atau nama berkas, sehingga pengguna biasanya tidak perlu menetapkan parser secara manual.

Bahasa tambahan dapat ditambahkan melalui plugin. Plugin dapat menyediakan definisi bahasa, parser, printer, opsi, dan nilai bawaan, serta dimuat melalui CLI, API, atau berkas konfigurasi.

## Instalasi dan alur dasar

Dokumentasi menyarankan instalasi lokal sebagai dependensi pengembangan dengan versi tepat. Penguncian versi penting karena setiap rilis, termasuk rilis perbaikan, dapat menghasilkan perbedaan format yang memicu perubahan bolak-balik antaranggota tim.

```bash
npm install --save-dev --save-exact prettier
npx prettier . --write
npx prettier . --check
```

`--write` menulis hasil pemformatan langsung ke berkas, sedangkan `--check` hanya memeriksa apakah berkas sudah sesuai dan mengembalikan kode keluar 1 jika ditemukan perbedaan. Perilaku ini memungkinkan editor memperbaiki format saat menyimpan, pre-commit hook memformat perubahan sebelum commit, dan CI menolak kode yang belum terformat.

## Konfigurasi dan filosofi opsi

Prettier sengaja menyediakan jumlah opsi yang terbatas. Opsi yang tersedia mencakup lebar cetak, indentasi, titik koma, tanda kutip, koma akhir, spasi kurung, posisi kurung elemen, akhir baris, pembungkusan prosa, serta pemformatan bahasa tertanam.

Konfigurasi dapat diletakkan pada `package.json`, `.prettierrc`, berkas JSON, YAML, TOML, JavaScript, atau TypeScript. Prettier mencari konfigurasi dari lokasi berkas yang sedang diformat ke arah atas dalam struktur direktori dan tidak mendukung konfigurasi global agar hasil proyek tetap sama ketika dipindahkan ke komputer lain.

*Override* memungkinkan aturan berbeda untuk ekstensi, direktori, atau berkas tertentu. Parser sebaiknya tidak ditetapkan pada tingkat teratas karena tindakan tersebut mematikan inferensi berbasis ekstensi dan dapat memaksa semua berkas memakai parser yang tidak sesuai.

## Berkas dan bagian yang tidak diformat

`.prettierignore` mengecualikan berkas atau direktori menggunakan sintaks yang sama dengan `.gitignore`. Secara bawaan, Prettier mengabaikan direktori sistem kontrol versi dan `node_modules`, serta mengikuti `.gitignore` yang berada di direktori tempat perintah dijalankan.

Komentar `prettier-ignore` dapat mempertahankan format node berikutnya pada JavaScript dan memiliki bentuk yang sesuai untuk JSX, HTML, CSS, Markdown, YAML, GraphQL, dan Handlebars. Fitur ini berguna untuk kode yang sengaja ditata khusus atau bagian yang dibuat oleh alat lain, tetapi penggunaan luas akan mengurangi konsistensi yang ingin dicapai Prettier.

## Integrasi editor, Git, dan CI

Dokumentasi merekomendasikan menjalankan Prettier dari editor, baik melalui pintasan maupun secara otomatis saat berkas disimpan. Integrasi tersedia untuk Visual Studio Code, Emacs, Vim, Helix, Sublime Text, keluarga IDE JetBrains, Visual Studio, dan editor lain. Plugin editor sebaiknya memakai versi Prettier lokal proyek agar hasilnya sama dengan CLI dan CI.

Pre-commit hook dapat dibangun dengan `lint-staged`, `pretty-quick`, `git-format-staged`, Lefthook, atau skrip shell. `lint-staged` cocok ketika Prettier dijalankan bersama ESLint atau Stylelint dan ketika proyek memerlukan dukungan perubahan yang hanya sebagian masuk tahap *staged*. Untuk CI, `prettier . --check` memeriksa format tanpa mengubah berkas dan memberikan status keluar yang dapat digunakan pipeline.

## Hubungan dengan linter

Prettier menggantikan aturan linter yang hanya berhubungan dengan format, seperti jarak kata kunci, gaya koma, atau campuran tab dan spasi. Aturan kualitas kode tetap menjadi tanggung jawab ESLint, Stylelint, atau linter lain. Pada proyek ESLint, dokumentasi menyarankan `eslint-config-prettier` untuk menonaktifkan aturan yang tidak diperlukan atau bertentangan dengan Prettier.

Urutan otomatisasi juga perlu diperhatikan. Jika `lint-staged` menjalankan ESLint dan Prettier, dokumentasi pemasangan menyarankan ESLint dijalankan lebih dahulu dan Prettier sesudahnya. Dengan urutan tersebut, hasil akhir tetap mengikuti formatter tanpa membiarkan aturan format linter menimbulkan perubahan yang saling berlawanan.

## Performa dan cache

CLI menyediakan opsi `--cache` agar berkas hanya diproses ulang ketika versi Prettier, opsi, versi Node.js, metadata atau isi berkas berubah. Strategi cache dapat memakai metadata atau konten. Metadata umumnya lebih cepat, sedangkan konten lebih aman ketika waktu modifikasi berubah tanpa perubahan isi, seperti setelah `git clone`.

Versi dan implementasi plugin tidak menjadi bagian dari kunci cache. Cache sebaiknya dihapus setelah plugin diperbarui agar hasil lama tidak digunakan ketika perilaku plugin telah berubah.

## Adopsi dan risiko perubahan

Prettier dapat diterapkan ke seluruh basis kode untuk menyeragamkan gaya lama dalam satu perubahan. Pada repositori besar, langkah ini sebaiknya dipisahkan dari perubahan fungsional agar riwayat lebih mudah ditinjau dan konflik cabang dapat dikendalikan.

Perubahan nilai bawaan dapat terjadi pada rilis besar. Sebagai contoh, nilai bawaan `trailingComma` berubah dari `es5` menjadi `all` pada Prettier 3. Karena itu, tim perlu mengunci versi, meninjau catatan rilis, dan melakukan peningkatan Prettier melalui perubahan tersendiri.

## Kapan Prettier tepat digunakan

Prettier tepat untuk tim yang ingin satu hasil format yang dapat direproduksi di editor, pre-commit hook, dan CI. Nilai utamanya bukan fleksibilitas gaya, melainkan penghapusan keputusan format berulang dan konsistensi lintas kontributor.

Prettier kurang sesuai jika proyek memerlukan kendali format yang sangat rinci atau ingin mempertahankan banyak variasi tata letak manual. Dalam kondisi tersebut, opsi terbatas dan pencetakan ulang berbasis AST dapat terasa terlalu ketat. Tim juga tetap memerlukan linter karena formatter tidak memeriksa sebagian besar masalah kualitas dan kebenaran kode.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Linters dan Formatters\|Linters dan Formatters]]
- [[References/Biome\|Biome]]
- [[References/Vite\|Vite]]
- [[References/Package Managers\|Package Managers]]

## Sumber

- [Prettier: Opinionated Code Formatter](https://prettier.io/): pengertian, manfaat, integrasi, dan posisi proyek.
- [What is Prettier?](https://prettier.io/docs/): cara kerja berbasis AST dan pencetakan ulang kode.
- [Why Prettier?](https://prettier.io/docs/why-prettier): alasan adopsi, konsistensi tim, dan penerapan pada basis kode lama.
- [Install Prettier](https://prettier.io/docs/install): instalasi lokal, penguncian versi, CI, ESLint, dan Git hooks.
- [Editor Integration](https://prettier.io/docs/editors): dukungan editor dan penggunaan versi lokal proyek.
- [Pre-commit Hook](https://prettier.io/docs/precommit): pilihan integrasi `lint-staged`, `pretty-quick`, `git-format-staged`, dan Lefthook.
- [Configuration File](https://prettier.io/docs/configuration): format konfigurasi, pencarian konfigurasi, *override*, dan EditorConfig.
- [Ignoring Code](https://prettier.io/docs/ignore): `.prettierignore` dan komentar `prettier-ignore`.
- [Options](https://prettier.io/docs/options): pilihan format, parser, bahasa, dan perubahan nilai bawaan.
- [Prettier vs. Linters](https://prettier.io/docs/comparison): pembagian tanggung jawab formatter dan linter.
- [Plugins](https://prettier.io/docs/plugins): arsitektur parser, printer, Doc, dan dukungan bahasa tambahan.
- [CLI](https://prettier.io/docs/cli): `--write`, `--check`, kode keluar, pola berkas, dan cache.
