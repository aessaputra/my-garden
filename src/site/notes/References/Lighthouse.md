---
{"dg-publish":true,"dg-path":"Lighthouse.md","permalink":"/lighthouse/","title":"Lighthouse","hideInFiletree":true,"tags":["references","programming","testing","performance","devops","ci-cd"],"noteIcon":"","dg-note-properties":{"title":"Lighthouse","category":"references","tags":["references","programming","testing","performance","devops","ci-cd"],"sources":["_raw/articles/lighthouse-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Lighthouse adalah alat audit website open-source yang menjalankan serangkaian pemeriksaan terhadap satu halaman di Chrome, lalu menghasilkan laporan berisi skor, metrik, diagnosis, dan saran perbaikan. Ia membantu menemukan masalah pada performance, accessibility, best practices, dan SEO. Lighthouse berguna sebagai alat diagnosis dan regression check, bukan sebagai bukti tunggal bahwa sebuah website cepat, aksesibel, aman, atau akan mendapat peringkat pencarian tinggi.

Deskripsi Lighthouse sebagai alat otomatis untuk meningkatkan kualitas halaman web pada dasarnya benar, tetapi perlu dua koreksi. Pertama, kategori PWA dihapus sejak Lighthouse 12, sehingga rilis saat ini tidak lagi menghasilkan skor PWA bawaan. Kedua, halaman yang memerlukan autentikasi memang dapat diaudit, tetapi Lighthouse harus dijalankan dalam sesi yang sudah login atau diberi alur autentikasi yang sesuai.

## Cara kerja

Pengguna memberi Lighthouse sebuah URL. Lighthouse membuka halaman melalui Chrome, mengumpulkan artefak seperti network records, trace eksekusi, struktur dokumen, dan informasi elemen, kemudian menjalankan audit terhadap artefak tersebut. Hasilnya dapat dibuka sebagai laporan [[References/HTML\|HTML]] atau diproses dalam bentuk JSON dan CSV.

Audit yang gagal menunjukkan area yang perlu diperiksa. Sebagian hasil berisi nilai numerik, sedangkan audit lain bersifat pass/fail atau informatif. Saran dalam laporan harus dibaca bersama konteks aplikasi: penghematan yang diperkirakan, kode pihak ketiga, kebutuhan produk, perangkat sasaran, dan apakah temuan dapat direproduksi.

Lighthouse dapat dijalankan melalui panel Lighthouse di Chrome DevTools, PageSpeed Insights, command-line interface, atau Node module. CLI memberi kontrol lebih besar atas kategori, audit, emulasi perangkat, throttling, locale, artefak, dan format output. Lighthouse 13 memerlukan Node.js 22.19 atau lebih baru.

```bash
npm install -g lighthouse
lighthouse <URL> --view
```

Untuk otomasi, laporan JSON lebih mudah disimpan dan dibandingkan:

```bash
lighthouse <URL> \
  --only-categories=performance,accessibility,best-practices,seo \
  --output=json \
  --output-path=./lighthouse-report.json
```

## Kategori audit

### Performance

Skor performance merangkum metrik laboratorium dari satu page load. Pada model skor yang didokumentasikan, metrik utamanya mencakup First Contentful Paint, Speed Index, Largest Contentful Paint, Total Blocking Time, dan Cumulative Layout Shift. Bobot dan audit dapat berubah antarversi. Lighthouse 13 juga mengganti sejumlah audit performance lama dengan performance insights yang digunakan bersama Chrome DevTools.

Skor ini dipengaruhi kondisi pengujian. Perangkat host, contention CPU, jaringan, beban server, A/B test, iklan, ekstensi browser, dan nondeterminisme browser dapat mengubah hasil walaupun kode tidak berubah. Karena itu, perbandingan harus memakai versi Lighthouse, perangkat, konfigurasi, URL, dan kondisi yang konsisten.

### Accessibility

Kategori accessibility memeriksa masalah yang dapat dideteksi otomatis, seperti accessible name, penggunaan ARIA, label form, alternative text, struktur heading, bahasa dokumen, dan kontras warna. Skornya merupakan weighted average dari audit berbasis dampak pengguna; setiap audit dinilai pass atau fail, bukan lulus sebagian.

Skor 100 tidak membuktikan aksesibilitas penuh. Pemeriksaan otomatis hanya mendeteksi sebagian masalah. Navigasi keyboard, alur dengan screen reader, urutan fokus, kejelasan instruksi, error recovery, serta kecocokan konten tetap memerlukan pengujian manual dan, bila memungkinkan, evaluasi oleh pengguna penyandang disabilitas.

### Best practices dan SEO

Best practices memeriksa pola implementasi web yang diketahui bermasalah, misalnya konfigurasi browser, keamanan dasar, atau penggunaan API yang tidak tepat. Daftar audit berubah mengikuti Chrome dan versi Lighthouse, sehingga audit lama dapat diganti atau dihapus.

Kategori SEO memeriksa dasar teknis yang dapat diautomasi, seperti crawlability, metadata, status respons, dan kualitas elemen tautan tertentu. Skor tinggi tidak menjamin ranking. Relevansi konten, reputasi, kualitas halaman lain, Core Web Vitals lapangan, dan banyak faktor pencarian lain berada di luar cakupan skor Lighthouse.

### PWA

Lighthouse 12 menghapus kategori dan skor PWA setelah perubahan kriteria installability Chrome. Website PWA tetap dapat diperiksa melalui dokumentasi installability, DevTools, pengujian manifest, service worker, perilaku offline, dan alur instalasi. Target CI baru tidak boleh bergantung pada `categories.pwa` seolah kategori tersebut masih tersedia.

## Lab data, field data, dan throttling

Lighthouse menghasilkan lab data dalam lingkungan pengujian terkontrol. Data ini berguna untuk diagnosis sebelum rilis karena satu page load dapat ditelusuri sampai resource dan task yang bermasalah. Ia berbeda dari field data yang merekam pengalaman pengguna nyata pada beragam perangkat, lokasi, jaringan, dan sesi.

Secara default Lighthouse memakai simulated throttling. Ia mengamati page load lalu mensimulasikan kondisi jaringan dan CPU yang lebih lambat. Pendekatan ini cepat dan mengurangi sebagian variasi, tetapi tetap merupakan model. Analisis mendalam dapat membutuhkan throttling pada tingkat packet serta field data untuk memeriksa apakah masalah laboratorium benar-benar dialami pengguna.

Jangan mengambil keputusan dari satu skor. Dokumentasi Lighthouse menyarankan beberapa run; median dari lima run lebih stabil daripada satu run. Bandingkan distribusi hasil dan metrik individual, bukan hanya angka kategori keseluruhan.

## Halaman terautentikasi

Run default bertindak seperti pengguna baru tanpa session atau storage sebelumnya. Untuk halaman di balik login, pilihan yang paling fleksibel adalah membuat alur login dengan Puppeteer. Alternatifnya, login lebih dulu pada Chrome lalu jalankan panel Lighthouse; nonaktifkan `Clear storage` bila sesi bergantung pada `localStorage` atau IndexedDB. CLI juga mendukung custom request headers atau koneksi ke instance Chrome yang sudah login.

Jangan menaruh token, cookie, atau credential pada command, repository, artefak CI, atau laporan yang dapat dibaca publik. Gunakan secret store milik CI dan batasi akses report karena URL, header, screenshot, atau data halaman internal dapat ikut terekam.

## Lighthouse CI

Lighthouse CI menjalankan, menyimpan, membandingkan, dan menerapkan assertion pada hasil Lighthouse. Ia dapat memberi laporan pada pull request, memantau skor dan metrik dari waktu ke waktu, serta menerapkan performance budget untuk script atau image. Integrasi ini melengkapi [[References/Playwright\|Playwright]] dan [[References/Vitest\|Vitest]]: Lighthouse mengaudit kualitas halaman, sedangkan test fungsional dan unit memeriksa perilaku aplikasi secara eksplisit.

Threshold sebaiknya berfokus pada pencegahan regresi yang bermakna, bukan mengejar skor 100. Jalankan beberapa kali, gunakan hasil representatif atau median, pin versi alat, stabilkan server pengujian, dan pisahkan warning informatif dari assertion yang memang harus menggagalkan build.

## Batas penggunaan

Lighthouse mengaudit halaman dan kondisi tertentu, bukan seluruh aplikasi. Satu URL tidak mewakili halaman lain, state setelah interaksi, pengguna kembali, variasi perangkat, atau semua jalur terautentikasi. Audit juga tidak menggantikan security assessment, usability research, pengujian lintas browser, monitoring produksi, maupun pengukuran [[References/Web Browser\|pengalaman browser]] yang nyata.

Workflow yang sehat memakai Lighthouse untuk menemukan masalah dan menjaga baseline, lalu memverifikasi dampaknya dengan trace, pengujian manual, field data, serta test aplikasi. Perbaiki penyebab pada metrik atau audit yang relevan; jangan mengubah implementasi hanya untuk menaikkan skor tanpa manfaat yang dapat dijelaskan kepada pengguna.

## Lihat juga

- [[References/HTML\|HTML]]
- [[References/JavaScript\|JavaScript]]
- [[References/HTTPS\|HTTPS]]
- [[References/Playwright\|Playwright]]
- [[References/Vitest\|Vitest]]
- [[References/Web Browser\|Web Browser]]

## Sumber

- [Introduction to Lighthouse](https://developer.chrome.com/docs/lighthouse/overview): cara kerja, metode menjalankan Lighthouse, laporan, dan Lighthouse CI.
- [Lighthouse repository](https://github.com/GoogleChrome/lighthouse): CLI, Node module, opsi konfigurasi, format output, dan kebutuhan Node.js.
- [Lighthouse performance scoring](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring): metrik, bobot, interpretasi skor, dan sumber variasi.
- [Lighthouse accessibility score](https://developer.chrome.com/docs/lighthouse/accessibility/scoring): model weighted average, audit pass/fail, dan keterbatasan otomatisasi.
- [Lighthouse SEO audits](https://developer.chrome.com/docs/lighthouse/seo/): cakupan pemeriksaan SEO dasar.
- [Lighthouse 12.0.0 release](https://github.com/GoogleChrome/lighthouse/releases/tag/v12.0.0): penghapusan kategori PWA dan reorganisasi SEO.
- [What's new in Lighthouse 13](https://developer.chrome.com/blog/lighthouse-13-0): performance insights, audit yang diganti atau dihapus, dan persyaratan Node.js.
- [Running Lighthouse on authenticated pages](https://github.com/GoogleChrome/lighthouse/blob/main/docs/authenticated-pages.md): Puppeteer, sesi DevTools, header, dan debug Chrome.
- [Score variability](https://github.com/GoogleChrome/lighthouse/blob/main/docs/variability.md): sumber variasi, pengulangan run, dan penggunaan median.
- [Network throttling](https://github.com/GoogleChrome/lighthouse/blob/main/docs/throttling.md): simulated, DevTools, proxy-level, dan packet-level throttling.
- [Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci): otomasi, assertion, histori, performance budget, dan multiple runs.
