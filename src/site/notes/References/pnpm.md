---
{"dg-publish":true,"dg-path":"pnpm.md","permalink":"/pnpm/","title":"pnpm","hideInFiletree":true,"tags":["references","javascript","programming","npm","package-manager","development","security"],"dg-note-properties":{"title":"pnpm","category":"references","tags":["references","javascript","programming","npm","package-manager","development","security"],"sources":["_raw/articles/pnpm-expanded.md"],"created":"2026-08-30","updated":"2026-08-30","confidence":"high"}}
---

pnpm adalah pengelola paket JavaScript yang dirancang untuk mempercepat instalasi dan mengurangi penggunaan ruang penyimpanan. Keunggulan tersebut berasal dari cara pnpm menyimpan berkas paket satu kali dalam *content-addressable store*, lalu menghubungkannya ke setiap proyek dengan *hard link* dan *symbolic link*. Nama pnpm sering dikaitkan dengan frasa *performant npm*, tetapi dokumentasi resmi saat ini lebih menekankan fungsi dan karakteristiknya daripada perluasan nama tersebut.

## Cara kerja penyimpanan

Pada pengelola paket yang menyalin dependensi ke setiap proyek, versi paket yang sama dapat tersimpan berulang kali. pnpm menempatkan berkas berdasarkan kontennya di penyimpanan bersama. Jika dua proyek memakai versi dependensi yang sama, keduanya merujuk ke berkas yang sama; jika versi baru hanya mengubah sebagian berkas, hanya bagian yang berubah yang perlu ditambahkan ke penyimpanan.

Proses instalasinya memisahkan resolusi dependensi, perhitungan struktur `node_modules`, dan pengaitan berkas dari penyimpanan. Tahapan tersebut dapat berjalan dengan tumpang tindih sehingga pnpm tidak harus menunggu seluruh resolusi selesai sebelum mulai mengambil dan menghubungkan paket. Benchmark resmi pnpm menunjukkan hasil yang lebih cepat daripada npm pada skenario uji yang dipublikasikan, tetapi angka tersebut bergantung pada kondisi seperti cache, lockfile, isi `node_modules`, latensi jaringan, dan karakteristik proyek.

## Struktur dependensi yang lebih ketat

Secara default, pnpm hanya menempatkan dependensi langsung proyek di akar `node_modules`; paket lainnya disusun dalam virtual store dan dihubungkan melalui symlink. Struktur ini mencegah kode aplikasi mengimpor paket yang tersedia secara kebetulan tetapi tidak tercantum dalam `package.json`, yang biasa disebut *phantom dependency*.

Klaim bahwa pnpm selalu menggunakan isolasi dependensi sepenuhnya perlu diberi batas. Konfigurasi standar saat ini dapat bersifat semiketat agar kompatibel dengan paket yang memiliki dependensi tidak dideklarasikan, sedangkan `publicHoistPattern` atau mode `nodeLinker: hoisted` dapat membuat paket tambahan terlihat dari akar proyek. Karena itu, tingkat isolasi bergantung pada konfigurasi dan kebutuhan kompatibilitas alat.

## Dukungan monorepo

pnpm mendukung *workspace* secara bawaan melalui `pnpm-workspace.yaml`, sehingga beberapa aplikasi dan pustaka dapat dikelola dalam satu repositori. Protokol `workspace:` memastikan sebuah dependensi diselesaikan dari paket lokal yang sesuai dan menggagalkan instalasi bila versi yang diminta tidak tersedia di workspace, alih-alih diam-diam mengambilnya dari registry.

Fitur *catalogs* memusatkan rentang versi dependensi yang dipakai oleh banyak paket. Setiap `package.json` dapat merujuk ke entri `catalog:`, sehingga pembaruan versi dilakukan pada satu tempat, duplikasi konfigurasi berkurang, dan konflik penggabungan lebih mudah dihindari. pnpm tidak menyediakan seluruh fungsi manajemen rilis monorepo secara bawaan; dokumentasinya merekomendasikan alat seperti Changesets atau Rush untuk pengaturan versi dan publikasi yang lebih kompleks.

## Keamanan rantai pasok

Struktur `node_modules` yang ketat membantu mencegah penggunaan dependensi tak terdeklarasi, tetapi itu bukan perlindungan lengkap terhadap paket berbahaya. pnpm menyediakan beberapa kontrol tambahan untuk mengurangi risiko rantai pasok: menonaktifkan eksekusi otomatis `postinstall` milik dependensi pada pnpm v10, membatasi skrip pembangunan ke paket yang dipercaya, menolak sumber transitif yang tidak lazim, menunda versi yang baru diterbitkan, dan mencegah penurunan bukti kepercayaan melalui `trustPolicy`.

Lockfile tetap perlu disimpan di repositori agar versi tidak berubah tanpa disengaja. Jika `pnpm-lock.yaml` dipindai untuk kerentanan atau pembuatan SBOM, alat pemindai juga harus mendukung format lockfile pnpm dengan benar agar dependensi tidak terlewat.

## Kelebihan dan pertimbangan penggunaan

pnpm paling menarik untuk mesin yang menangani banyak proyek JavaScript, CI dengan instalasi berulang, dan monorepo besar. Penyimpanan bersama mengurangi duplikasi berkas, sedangkan workspace, protokol `workspace:`, dan catalogs membantu menjaga konsistensi dependensi.

Konsekuensinya, struktur berbasis link dapat bermasalah dengan alat lama yang mengasumsikan `node_modules` datar. pnpm menyediakan mode hoisted sebagai jalur kompatibilitas, tetapi mode ini mengurangi sebagian ketatnya isolasi dependensi. Migrasi sebaiknya disertai pengujian proses build, plugin, bundler, skrip instalasi, deployment, dan alat keamanan yang membaca lockfile.

Popularitas pnpm dapat dilihat dari penggunaannya pada proyek terbuka seperti Next.js, Vite, Nuxt, Vue, Astro, Prisma, Turborepo, dan SvelteKit. Bukti tersebut menunjukkan adopsi yang luas, tetapi tidak dengan sendirinya membuktikan bahwa pnpm telah melampaui npm atau pengelola paket lain dalam pangsa penggunaan keseluruhan.

## Lihat juga

- [[References/Package Managers\|Package Managers]]
- [[References/npm\|npm]]
- [[References/Yarn\|Yarn]]
- [[References/JavaScript\|JavaScript]]
- [[References/Vite\|Vite]]

## Sumber

- [Motivation | pnpm](https://pnpm.io/motivation): penyimpanan berbasis konten, hard link, symlink, dan proses instalasi.
- [pnpm vs npm | pnpm](https://pnpm.io/pnpm-vs-npm): perbedaan struktur `node_modules` dan kepatuhan terhadap `package.json`.
- [Workspace | pnpm](https://pnpm.io/workspaces): workspace, protokol `workspace:`, publikasi, dan batas manajemen rilis.
- [Catalogs | pnpm](https://pnpm.io/catalogs): pemusatan rentang versi dependensi dalam monorepo.
- [Mitigating supply chain attacks | pnpm](https://pnpm.io/supply-chain-security): kontrol skrip instalasi, lockfile, sumber dependensi, usia rilis, dan kebijakan kepercayaan.
- [Node-Modules & Hoisting Settings | pnpm](https://pnpm.io/settings/node-modules): mode linker, hoisting, dan tingkat isolasi dependensi.
- [Benchmarks of JavaScript Package Managers | pnpm](https://pnpm.io/benchmarks): metodologi dan hasil benchmark npm serta pnpm.
