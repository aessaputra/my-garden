---
{"dg-publish":true,"dg-path":"Linters dan Formatters.md","permalink":"/linters-dan-formatters/","title":"Linters dan Formatters","hideInFiletree":true,"tags":["references","programming","coding-standards","testing","workflow","ci-cd"],"noteIcon":"","dg-note-properties":{"title":"Linters dan Formatters","category":"references","tags":["references","programming","coding-standards","testing","workflow","ci-cd"],"sources":["_raw/articles/linters-formatters-research-packet.md"],"created":"2026-09-03","updated":"2026-09-03","confidence":"high"}}
---

Linter menganalisis kode untuk menemukan pola bermasalah atau pelanggaran aturan. Formatter menata representasi kode secara deterministik agar hasilnya konsisten.

Keduanya meningkatkan kualitas lebih awal, tetapi tidak menggantikan pemeriksaan tipe, pengujian, review manusia, atau validasi perilaku program.

## Pembagian tanggung jawab

Linter bekerja pada aturan. Temuannya dapat mencakup bug potensial, penggunaan API yang keliru, kode tidak terpakai, aksesibilitas, keamanan, kompleksitas, dan kebijakan tim.

Formatter mengurai kode lalu mencetak ulang tata letaknya. Ia menangani indentasi, spasi, pemenggalan baris, tanda kutip, koma, dan detail presentasi lain tanpa menilai tujuan bisnis program.

Pemisahan ini mencegah satu alat dijadikan jaminan palsu. Kode yang rapi masih dapat salah, sedangkan kode yang benar secara logika masih dapat melanggar aturan keamanan atau maintainability.

Istilah `formatter` juga dapat berarti format keluaran diagnostik linter. Pada konteks itu, formatter mengubah tampilan laporan menjadi teks, JSON, atau format CI, bukan mengubah source code.

## Cara kerja linter

Parser mengubah source code menjadi struktur sintaks. Aturan mengunjungi struktur itu, mengenali pola, lalu menghasilkan diagnostic dengan lokasi, severity, dan kadang perbaikan.

Plugin, parser, processor, dan custom syntax memperluas domain analisis. Karena itu, dukungan nama bahasa belum tentu berarti dukungan lengkap untuk framework, template, atau aturan proyek tertentu.

Linting sintaks lokal relatif murah. Typed linting memakai informasi tipe dan graf proyek untuk menemukan masalah lintas berkas, tetapi meminta analisis proyek dan menambah waktu eksekusi.

Pada TypeScript, aturan bertipe berguna untuk kontrak nullable, promise, operasi boolean, dan API generik. Biaya tersebut perlu diukur pada repositori nyata, terutama monorepo besar.

## Cara kerja formatter

Formatter modern biasanya membangun AST atau CST, lalu mencetak struktur itu menurut kebijakan stabil. Hasil ditentukan oleh versi alat, konfigurasi, plugin, parser, dan isi berkas.

[[References/Prettier\|Prettier]] membuang sebagian besar gaya asli dan mencetak ulang AST. Pendekatan berpendapat mengurangi perdebatan gaya, tetapi memberi ruang konfigurasi yang sengaja terbatas.

Formatter tidak membuktikan bahwa transformasi aman untuk seluruh ekosistem. Versi dan plugin perlu dikunci, perubahan besar perlu dipisahkan, dan diff tetap perlu ditinjau saat migrasi.

## Konflik antara lint dan format

Linter lama sering memuat aturan kualitas sekaligus aturan gaya. Saat formatter khusus dipakai, aturan gaya linter dapat menjadi mubazir atau menghasilkan perubahan bolak-balik.

Pada kombinasi [[References/ESLint\|ESLint]] dan Prettier, tanggung jawab format diberikan kepada Prettier. Konfigurasi seperti `eslint-config-prettier` menonaktifkan aturan ESLint yang bertentangan atau tidak lagi diperlukan.

Toolchain terpadu juga memerlukan koordinasi internal. Dokumentasi Ruff, misalnya, mencantumkan aturan lint yang sebaiknya dihindari agar formatter tidak memperkenalkan lint error baru.

Prinsipnya sederhana: satu pemilik untuk keputusan format. Linter tetap menangani kualitas, sedangkan formatter menjadi sumber kebenaran tunggal untuk tata letak.

## Autofix dan risikonya

Autofix mempercepat perbaikan mekanis, tetapi setiap alat memiliki batas keamanan. Fix yang diklasifikasikan aman dirancang untuk mempertahankan semantik, bukan membuktikannya secara formal.

[[References/Biome\|Biome]] dan Ruff membedakan fix aman dari fix tidak aman. Fix tidak aman dapat mengubah runtime behavior atau menghapus komentar, sehingga perlu opsi eksplisit dan review manual.

Jalankan fix aman pada perubahan terbatas, periksa diff, lalu jalankan test dan type check. Jangan mencampur migrasi formatter berskala besar dengan perubahan fitur.

## Alur kerja berlapis

Editor memberi feedback tercepat. Format on save dan diagnostic lint membantu pengembang memperbaiki masalah tanpa menunggu pipeline.

Pre-commit hook dapat memproses berkas staged agar pemeriksaan cepat. Hook bukan enforcement final karena dapat dilewati, gagal terpasang, atau berbeda antarlingkungan.

CI menjadi quality gate bersama. Gunakan mode nonmutasi seperti `prettier --check`, `eslint`, `biome ci`, atau padanannya, lalu gagalkan pipeline berdasarkan exit status.

CI sebaiknya tidak diam-diam menulis source code. Jika perbaikan otomatis diinginkan, buat patch atau pull request terpisah agar perubahan terlihat dan dapat ditinjau.

Urutan umum untuk JavaScript dan TypeScript:

1. Format atau periksa format.
2. Jalankan lint sintaks dan aturan proyek.
3. Jalankan typed linting bila dipakai.
4. Jalankan type checker.
5. Jalankan test.
6. Bangun artifact bila relevan.

Urutan fix lokal dapat berbeda. Jika linter juga menulis format, jalankan fix linter dahulu dan formatter terakhir, lalu pastikan pemeriksaan ulang tidak menghasilkan diff baru.

## Memilih alat

Pilih berdasarkan bahasa, parser, framework, plugin, kecepatan, kualitas diagnostic, dukungan editor, stabilitas konfigurasi, dan kebutuhan CI.

Untuk JavaScript dan TypeScript, ESLint plus Prettier memberi ekosistem aturan luas dan pemisahan tugas yang jelas. Konsekuensinya ialah dua konfigurasi dan koordinasi konflik.

Biome menyatukan lint, format, dan organisasi impor. Ia cocok bila dukungan bahasa dan aturan proyek sudah memadai, tetapi migrasi perlu mengaudit aturan atau plugin yang tidak terpetakan.

Untuk Python, Ruff menyediakan linter dan formatter terpadu. Untuk stylesheet, Stylelint memberi aturan, custom syntax, autofix, batas warning, dan keluaran diagnostik yang sesuai CI.

Tidak ada satu kombinasi yang unggul untuk semua repositori. Ukur waktu lokal dan CI, cakupan aturan penting, false positive, serta perubahan output pada kode proyek sendiri.

## Konfigurasi yang dapat direproduksi

Pasang alat sebagai dependensi proyek dan kunci versinya. Editor, hook, dan CI harus memakai binary serta konfigurasi yang sama.

Tetapkan ignore untuk generated code, vendor, build output, dan artifact. Pengecualian source code biasa perlu alasan karena ignore yang luas dapat menyembunyikan masalah.

Mulai dari preset rekomendasi, lalu sesuaikan berdasarkan risiko. Jadikan bug dan keamanan sebagai error; gunakan warning sementara untuk adopsi bertahap dengan tenggat penghapusan.

Catat pengecualian di dekat kode atau konfigurasi. Suppression tanpa alasan mudah berubah menjadi jalur permanen bagi temuan yang seharusnya diperbaiki.

## Evaluasi penerapan

Baseline awal perlu mencatat jumlah error, warning, waktu eksekusi, file yang diabaikan, dan aturan yang dinonaktifkan. Data ini membedakan perbaikan nyata dari sekadar perubahan konfigurasi.

Metrik yang berguna mencakup warning baru, waktu feedback lokal, durasi CI, false positive, suppression, serta bug yang ditemukan sebelum review atau produksi.

Target `zero warning` hanya bernilai jika aturan relevan dan suppression diaudit. Menurunkan severity atau mengabaikan direktori dapat mempercantik angka tanpa meningkatkan kualitas.

## Batas

Linting tetap merupakan analisis berbasis model aturan. Ia dapat melewatkan bug yang membutuhkan state runtime, data produksi, concurrency, integrasi eksternal, atau pemahaman domain.

Formatting meningkatkan konsistensi dan keterbacaan, bukan correctness. Klaim performa antaralat juga bergantung pada repositori, cache, hardware, aturan, dan mode typed analysis.

Dokumentasi alat berubah cepat. Periksa ulang perintah, dukungan bahasa, klasifikasi fix, dan aturan konflik saat melakukan peningkatan versi.

## Lihat juga

- [[References/ESLint\|ESLint]]
- [[References/Prettier\|Prettier]]
- [[References/Biome\|Biome]]
- [[References/JavaScript\|JavaScript]]
- [[References/AI-Powered Code Review\|AI-Powered Code Review]]
- [[References/Refactoring dengan AI\|Refactoring dengan AI]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [Core Concepts: ESLint](https://eslint.org/docs/latest/use/core-concepts): linter, rules, fixes, parser, processor, plugin, serta formatter diagnostik.
- [ESLint CLI](https://eslint.org/docs/latest/use/command-line-interface): fix, batas warning, output, dan exit code untuk otomasi.
- [What is Prettier?](https://prettier.io/docs/): pencetakan ulang AST dan konsistensi format.
- [Integrating with Linters](https://prettier.io/docs/integrating-with-linters): pembagian tugas serta konflik aturan gaya.
- [Prettier CLI](https://prettier.io/docs/cli): mode write, check, pola berkas, dan exit code CI.
- [Biome Linter](https://biomejs.dev/linter/): aturan, diagnostic, serta safe dan unsafe fixes.
- [Biome Formatter](https://biomejs.dev/formatter/): filosofi formatter berpendapat dan pemeriksaan format.
- [Continuous Integration: Biome](https://biomejs.dev/recipes/continuous-integration/): perbedaan `check` dan `ci`.
- [Typed Linting](https://typescript-eslint.io/getting-started/typed-linting): kemampuan dan biaya analisis berbasis tipe.
- [Ruff Linter](https://docs.astral.sh/ruff/linter/): rule selection serta klasifikasi fix.
- [Ruff Formatter](https://docs.astral.sh/ruff/formatter/): formatter Python terpadu dan aturan lint yang konflik.
- [Stylelint Options](https://stylelint.io/user-guide/options/): autofix, custom syntax, warning threshold, dan output diagnostic.
