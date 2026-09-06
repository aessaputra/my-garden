---
{"dg-publish":true,"dg-path":"Desktop Applications in JavaScript.md","permalink":"/desktop-applications-in-java-script/","title":"Desktop Applications in JavaScript","hideInFiletree":true,"tags":["references","javascript","architecture","security"],"noteIcon":"","dg-note-properties":{"title":"Desktop Applications in JavaScript","categories":["Frameworks"],"tags":["references","javascript","architecture","security"],"sources":["_raw/articles/desktop-applications-in-javascript-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---


Aplikasi desktop berbasis JavaScript memakai teknologi web untuk UI dan runtime desktop untuk integrasi OS. [Electron](https://www.electronjs.org/) mendukung Windows, macOS, dan Linux.

## Pilihan runtime

[[References/Electron\|Electron]] membawa Chromium dan Node.js. Browser yang dipaketkan memberi target rendering terkontrol, sebagaimana dijelaskan [dokumentasi Electron](https://www.electronjs.org/).

[NW.js](https://docs.nwjs.io/For%20Users/Getting%20Started/) juga berbasis Chromium dan Node.js, dengan pemanggilan modul Node langsung dari konteks browser. Modelnya tidak identik dengan pemisahan proses Electron.

[NW.js tersedia](https://nwjs.io/) untuk Linux, macOS, dan Windows. Native Node modules tetap memerlukan perhatian pada kompatibilitas binary, bukan sekadar berbagi source code.

[[References/Tauri\|Tauri]] memakai inti Rust dan WebView sistem untuk HTML, CSS, serta JavaScript. [Model proses Tauri](https://v2.tauri.app/concept/process-model/) tidak menyertakan pustaka WebView dalam executable akhir.

Tauri memakai WebView2 pada Windows, WKWebView pada macOS, dan WebKitGTK pada Linux. Perbedaan ini dijelaskan dalam [dokumentasi runtime](https://v2.tauri.app/concept/process-model/).

Jadi, memakai JavaScript untuk frontend Tauri tidak berarti seluruh aplikasi ditulis dalam JavaScript atau menjalankan Node.js sebagai runtime bawaannya.

## Akses filesystem dan integrasi native

[Electron](https://www.electronjs.org/) menyediakan API main process untuk window, menu, dialog, dan notifikasi. Akses sistem bukan hak yang perlu diberikan penuh kepada konten UI.

[Tauri filesystem plugin](https://v2.tauri.app/plugin/file-system/) menyediakan operasi berkas dari JavaScript dan Rust. Permissions serta scopes mengatur operasi dan jalur yang dapat diakses frontend.

[NW.js Getting Started](https://docs.nwjs.io/For%20Users/Getting%20Started/) memperlihatkan API native dan penggunaan Node.js. Native addons perlu dibangun ulang agar sesuai ABI NW.js.

## Batas keamanan

[Electron security](https://www.electronjs.org/docs/latest/tutorial/security) melarang Node integration untuk remote content, menganjurkan context isolation, sandbox, dan pemeriksaan sender IPC.

[Tauri capabilities](https://v2.tauri.app/security/capabilities/) mengatur paparan core kepada frontend. Namun, app commands yang didaftarkan melalui `invoke_handler` tersedia untuk semua window aplikasi secara default.

Pembatasan app commands dapat dikonfigurasi lewat `AppManifest::commands`. Jangan menyamakan default tersebut dengan izin plugin; lihat [aturan capabilities](https://v2.tauri.app/security/capabilities/).

## Contoh dan trade-off

Maintainer Electron menyebut VS Code dan Discord sebagai pengguna dalam [Why Electron](https://www.electronjs.org/docs/latest/why-electron). Contoh adopsi bukan bukti bahwa framework selalu paling cepat atau hemat.

Reuse keterampilan web dapat membantu pengembangan, tetapi tidak membuktikan durasi atau biaya proyek tertentu. Integrasi OS, distribusi, pengujian, dan pemeliharaan tetap perlu diperhitungkan.

[Electron mengakui](https://www.electronjs.org/docs/latest/why-electron) bundling menambah ukuran disk. WebView sistem mengurangi isi executable Tauri, bukan otomatis menurunkan penggunaan RAM setiap workload.

[Panduan performa Electron](https://www.electronjs.org/docs/latest/tutorial/performance) menekankan profiling. Ukur startup, idle memory, CPU, latensi interaksi, serta ukuran distribusi secara terpisah.

Rekomendasi: bandingkan fitur dan workload setara pada perangkat target. Jangan melemahkan keamanan untuk mengejar angka benchmark atau menjanjikan keunggulan universal terhadap toolkit native.

## Batas riset

Sembilan dokumen substantif diambil melalui 9Router. URL dokumentasi NW.js lama menghasilkan 404; URL kanonik berhasil diambil. Tidak dilakukan build aplikasi, benchmark, atau pengujian lintas OS.

Pernyataan performa vendor diperlakukan sebagai pengalaman mereka, bukan benchmark independen. Halaman [[References/Electron\|Electron]] dan [[References/Tauri\|Tauri]] yang sudah ada tidak diubah dalam riset ini.

## Lanjutan

- [[Desktop runtime choices trade package size for rendering control\|Desktop runtime choices trade package size for rendering control]]
- [[Native desktop access needs explicit trust boundaries\|Native desktop access needs explicit trust boundaries]]
- [[Desktop performance claims need workload measurements\|Desktop performance claims need workload measurements]]
