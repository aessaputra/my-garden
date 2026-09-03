---
{"dg-publish":true,"dg-path":"Chrome DevTools.md","permalink":"/chrome-dev-tools/","title":"Chrome DevTools","hideInFiletree":true,"tags":["references","programming","testing","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Chrome DevTools","aliases":["DevTools Usage","Chrome Developer Tools"],"type":"reference","status":"evergreen","tags":["references","programming","testing","performance","security"],"created":"2026-08-29","updated":"2026-08-29","source_type":"official-docs","confidence":"high"}}
---

Chrome DevTools adalah seperangkat alat pengembangan yang terpasang di Google Chrome. DevTools bekerja terhadap halaman yang sedang dijalankan oleh browser: pengembang dapat memeriksa [[References/HTML\|HTML]] dan [[References/CSS\|CSS]] hasil render, menjalankan dan men-debug [[References/JavaScript\|JavaScript]], mengamati request jaringan, merekam aktivitas runtime, serta memeriksa penyimpanan dan konfigurasi keamanan. Karena alat ini memperlihatkan keadaan aktual di browser, DevTools berguna untuk menjawab bukan hanya “apa isi source code?”, tetapi juga “apa yang benar-benar dimuat, dieksekusi, dihitung, dan ditampilkan?”.

## Membuka dan mengatur DevTools

DevTools dapat dibuka dari menu Chrome, dengan klik kanan lalu **Inspect**, atau melalui pintasan keyboard. Panel yang tersedia dapat dibuka dari tab utama, menu **More tools**, atau **Command Menu**. Command Menu juga menyediakan perintah untuk membuka panel, mengambil screenshot, menonaktifkan JavaScript, dan menjalankan tindakan lain tanpa menelusuri menu.

DevTools biasanya ditambatkan di sisi atau bagian bawah jendela, tetapi dapat dipisahkan ke jendela sendiri. Pemilihan susunan panel bukan sekadar preferensi visual: tampilan vertikal lebih nyaman untuk memeriksa layout, sedangkan jendela terpisah memberi ruang lebih luas untuk trace Performance dan data Network.

## Alur kerja debugging

Penggunaan DevTools paling efektif dimulai dari gejala, bukan dari membuka semua panel sekaligus:

1. Reproduksi masalah dalam kondisi yang diketahui.
2. Periksa **Console** dan **Issues** untuk error, warning, serta masalah yang sudah dikenali browser.
3. Gunakan **Network** jika masalah berkaitan dengan pemuatan resource, request API, cache, redirect, atau timing.
4. Gunakan **Elements** untuk masalah struktur, style, layout, state elemen, dan accessibility tree.
5. Gunakan **Sources** untuk menghentikan eksekusi pada breakpoint dan menelusuri perubahan nilai.
6. Gunakan **Performance** atau **Memory** jika masalah berupa respons lambat, jank, long task, atau penggunaan memori yang terus naik.
7. Ulangi pengujian setelah perbaikan dengan kondisi yang sebanding.

Urutan ini membatasi ruang pencarian. Error `404` pada Network, misalnya, lebih informatif daripada langsung menelusuri fungsi render yang gagal menerima data.

## Elements: DOM dan CSS yang benar-benar dipakai

Panel **Elements** menampilkan DOM halaman dan aturan CSS yang diterapkan. Pengembang dapat mengubah atribut, class, teks, deklarasi style, pseudo-state, dan box model untuk menguji hipotesis secara langsung. Panel **Computed** membantu membedakan nilai akhir dari deklarasi yang kalah dalam cascade atau diwariskan dari elemen lain.

Perubahan di Elements pada dasarnya adalah eksperimen lokal. Perubahan DOM tidak otomatis mengubah source code dan umumnya hilang setelah reload. Untuk mempertahankan eksperimen lintas reload, gunakan **Local Overrides**; untuk menulis perubahan ke file proyek lokal, gunakan **Workspaces**. Keduanya memiliki tujuan berbeda: Overrides mengganti respons jaringan secara lokal, sedangkan Workspaces memetakan resource yang dimuat ke source file di komputer.

Elements juga menyediakan informasi accessibility, termasuk role, name, properti ARIA, dan accessibility tree. Pemeriksaan ini membantu memahami representasi elemen bagi teknologi bantu, tetapi tidak menggantikan pengujian manual dengan keyboard, screen reader, dan pengguna.

## Console dan Sources: memahami eksekusi JavaScript

Panel **Console** menampilkan pesan log dan error, menerima ekspresi JavaScript, serta memberi akses ke konteks halaman. Console cocok untuk memeriksa nilai, memilih elemen, menguji ekspresi kecil, dan memanggil fungsi saat diagnosis. Jangan menempelkan kode yang tidak dipahami: kode di Console berjalan dengan kemampuan yang tersedia pada konteks halaman dan dapat membaca atau mengubah data di dalamnya.

Panel **Sources** digunakan untuk debugging terstruktur. Breakpoint dapat dipasang pada baris kode, event listener, perubahan DOM, exception, request XHR/fetch, atau kondisi tertentu. Ketika eksekusi berhenti, panel menampilkan call stack, scope, nilai variabel, watch expression, dan kontrol untuk melangkah masuk, melewati, atau keluar dari fungsi.

Aplikasi modern sering mengirim bundle yang telah dikompilasi atau diminifikasi. **Source maps** memetakan kode deployed ke source code asli sehingga breakpoint dan stack trace dapat dibaca pada file yang ditulis pengembang. Hasil pemetaan tetap bergantung pada source map yang benar dan dapat diakses; jika pemetaan keliru, periksa juga generated code dan status pemuatan source map.

## Network: request, respons, dan cache

Panel **Network** merekam request selama DevTools terbuka. Setiap entri dapat diperiksa dari sisi URL, method, status, request dan response headers, payload, response body, initiator, ukuran transfer, serta fase timing. Filter berdasarkan jenis resource atau teks membantu memisahkan dokumen, script, stylesheet, gambar, font, fetch/XHR, dan request lain.

Beberapa praktik diagnosis umum:

- Aktifkan **Preserve log** untuk mempertahankan request saat navigasi.
- Gunakan **Disable cache** saat ingin membandingkan cold load secara terkendali; opsi ini berlaku ketika DevTools terbuka.
- Periksa kolom **Initiator** sebelum menyimpulkan siapa yang memicu request.
- Gunakan throttling untuk mensimulasikan kondisi jaringan tertentu, bukan untuk mengklaim pengalaman semua pengguna.
- Ekspor HAR untuk berbagi urutan request, tetapi sanitasi isinya terlebih dahulu. HAR dapat memuat URL sensitif, header autentikasi, cookie, query parameter, dan payload.

Panel Network membantu mengamati perilaku [[References/HTTP\|HTTP]] dan [[References/HTTPS\|HTTPS]], tetapi tidak otomatis menjelaskan penyebab di server. Status berhasil pun belum membuktikan bahwa isi respons benar atau bahwa aplikasi memprosesnya dengan tepat.

## Performance dan Memory

Panel **Performance** merekam aktivitas browser selama page load atau interaksi. Trace dapat memperlihatkan main-thread work, event, task JavaScript, rendering, layout, paint, screenshot, network, dan frame. Analisis biasanya dimulai dari bagian yang terasa lambat, lalu memperbesar rentang waktu tersebut dan menelusuri long task, call stack, layout shift, atau pekerjaan rendering yang mahal.

Tampilan Performance dapat membandingkan metrik lokal dengan field data dari Chrome UX Report ketika data tersedia dan pengguna mengaktifkannya. Metrik lokal berasal dari satu sesi pada perangkat, jaringan, state, dan pola interaksi tertentu; field data mengagregasi pengalaman pengguna nyata dalam periode tertentu. Keduanya menjawab pertanyaan berbeda. Throttling, device emulation, dan satu trace lokal membantu reproduksi, tetapi tidak mengubah laptop menjadi perangkat fisik pengguna.

Panel **Memory** membantu menyelidiki kebocoran memori dan alokasi yang tidak perlu melalui heap snapshot, allocation instrumentation, dan sampling. Satu snapshot besar bukan bukti kebocoran. Pola yang lebih kuat adalah objek yang tetap tertahan atau terus bertambah setelah tindakan diulang dan garbage collection diberi kesempatan berjalan.

Untuk audit yang repeatable dan berskala lebih luas, gunakan [[References/Lighthouse\|Lighthouse]] atau pengujian otomatis sebagai pelengkap. DevTools lebih cocok untuk diagnosis interaktif pada satu konteks browser, sedangkan audit otomatis membantu menjaga pemeriksaan tetap konsisten.

## Application, Security, dan Coverage

Panel **Application** mengumpulkan data yang terkait dengan manifest, service worker, storage, cache storage, cookie, IndexedDB, dan resource aplikasi lainnya. Panel ini berguna ketika state lama, service worker, atau cache membuat perilaku sulit direproduksi. Menghapus storage dapat mengakhiri sesi login dan menghapus data lokal; catat state pengujian sebelum membersihkannya.

Panel **Security** memberikan ringkasan keamanan origin dan informasi sertifikat serta koneksi untuk resource yang dimuat. Panel ini membantu menemukan mixed content atau resource yang tidak aman, tetapi bukan pemindai kerentanan aplikasi dan bukan bukti bahwa aplikasi aman secara keseluruhan.

Panel **Coverage** memperkirakan bagian CSS dan JavaScript yang digunakan atau tidak digunakan selama rekaman. Hasilnya bergantung pada alur yang dijalankan. Kode yang tampak tidak terpakai pada satu halaman atau interaksi mungkin dibutuhkan oleh route, state, atau pengguna lain; gunakan coverage sebagai petunjuk optimasi, bukan perintah untuk menghapus kode secara langsung.

## Emulasi, remote debugging, dan perubahan lokal

**Device Mode** mengemulasikan viewport, device pixel ratio, orientasi, input sentuh, dan kondisi tertentu. Fitur ini efektif untuk menemukan masalah responsive design dengan cepat, tetapi dokumentasi Chrome menegaskan bahwa emulasi bukan pengganti pengujian pada perangkat nyata. Untuk masalah yang bergantung pada hardware, browser mobile, atau interaksi fisik, gunakan remote debugging pada perangkat Android.

**Local Overrides** dapat mengganti konten response dan response headers secara lokal, termasuk response fetch/XHR. Cache dinonaktifkan ketika Overrides aktif, perubahan DOM di Elements tidak disimpan, dan file yang dipetakan melalui source map memiliki batasan. Hapus atau nonaktifkan override setelah eksperimen agar hasil lama tidak menyamarkan perilaku server saat ini.

**Workspaces** menghubungkan folder lokal dengan resource jaringan agar perubahan pada CSS, HTML, dan JavaScript dapat disimpan ke source code. Karena fitur ini memberi DevTools akses tulis ke folder yang dipilih, hubungkan hanya direktori kerja yang memang dimaksudkan untuk diedit dan tinjau diff melalui version control.

## Batas interpretasi

DevTools memperlihatkan keadaan browser yang sedang diamati, bukan seluruh sistem. Hasil dapat berubah karena versi Chrome, extension, cache, login state, eksperimen, ukuran viewport, hardware, jaringan, server, dan cara pengguna berinteraksi. Karena itu:

- catat kondisi reproduksi;
- bandingkan sebelum dan sesudah dalam kondisi serupa;
- uji pada lebih dari satu viewport dan perangkat jika dampaknya luas;
- konfirmasi masalah backend melalui log dan observability server;
- gunakan field data untuk memprioritaskan masalah performance pengguna nyata;
- jangan menganggap tidak adanya warning sebagai bukti bebas bug, aman, atau aksesibel.

DevTools memperpendek siklus observasi, hipotesis, eksperimen, dan verifikasi. Nilai utamanya bukan banyaknya panel, melainkan kemampuan memilih panel dan bukti yang sesuai dengan gejala.

## Sumber

1. Chrome DevTools overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/overview
2. Elements panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/elements
3. Console overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/console
4. Sources panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/sources
5. Network panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/network/overview
6. Performance panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/performance/overview
7. Memory panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/memory
8. Application panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/application
9. Security panel overview — Chrome for Developers: https://developer.chrome.com/docs/devtools/security
10. Device Mode — Chrome for Developers: https://developer.chrome.com/docs/devtools/device-mode
11. Override web content and HTTP response headers locally — Chrome for Developers: https://developer.chrome.com/docs/devtools/overrides
12. Set up Workspaces to save changes to source files — Chrome for Developers: https://developer.chrome.com/docs/devtools/workspaces
13. Remote debug Android devices — Chrome for Developers: https://developer.chrome.com/docs/devtools/remote-debugging
14. Accessibility features reference — Chrome for Developers: https://developer.chrome.com/docs/devtools/accessibility/reference
15. Find unused JavaScript and CSS with the Coverage tab — Chrome for Developers: https://developer.chrome.com/docs/devtools/coverage
