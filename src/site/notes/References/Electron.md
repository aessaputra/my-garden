---
{"dg-publish":true,"dg-path":"Electron.md","permalink":"/electron/","title":"Electron","hideInFiletree":true,"tags":["references","programming","javascript","nodejs","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Electron","category":"references","tags":["references","programming","javascript","nodejs","performance","security"],"sources":["https://www.electronjs.org/docs/latest/","https://www.electronjs.org/docs/latest/tutorial/process-model","https://www.electronjs.org/docs/latest/tutorial/sandbox","https://www.electronjs.org/docs/latest/tutorial/ipc","https://www.electronjs.org/docs/latest/tutorial/security","https://www.electronjs.org/docs/latest/tutorial/performance","https://www.electronjs.org/docs/latest/tutorial/tutorial-packaging","https://www.electronjs.org/docs/latest/tutorial/updates","https://www.electronjs.org/docs/latest/tutorial/code-signing","https://www.electronjs.org/docs/latest/tutorial/asar-archives","https://www.electronjs.org/docs/latest/tutorial/using-native-node-modules","https://www.electronjs.org/docs/latest/tutorial/accessibility","https://www.electronforge.io/","https://www.electronforge.io/config/makers"],"created":"2026-08-29","updated":"2026-08-29"}}
---

Electron adalah framework untuk membuat aplikasi desktop Windows, macOS, dan Linux dengan [[References/JavaScript\|JavaScript]], [[References/HTML\|HTML]], dan [[References/CSS\|CSS]]. Setiap aplikasi membawa Chromium untuk merender antarmuka web dan Node.js untuk pekerjaan yang memerlukan akses sistem. Pendekatan ini memungkinkan satu basis kode berbasis teknologi web, tetapi hasilnya tetap aplikasi desktop yang harus menangani proses, izin sistem operasi, instalasi, pembaruan, dan keamanan lokal.

Electron bukan sekadar situs web yang dibungkus menjadi executable. Aplikasi memiliki beberapa proses dengan tingkat hak akses berbeda. Ia juga bukan jaminan bahwa seluruh kode dan perilaku akan identik pada setiap sistem operasi. Menu, tray, dialog, notifikasi, code signing, format installer, dan integrasi sistem tetap memiliki perbedaan platform.

## Model proses

Electron mewarisi arsitektur multi-process dari Chromium. Satu main process menjadi entry point aplikasi, mengelola lifecycle, membuat `BrowserWindow`, dan memakai API Node.js serta modul native Electron. Setiap window biasanya memiliki renderer process sendiri untuk menampilkan UI dengan standar web. Pemisahan ini membatasi dampak crash renderer dan mencegah satu window memblokir seluruh aplikasi, walaupun semua proses tetap memakai sumber daya.

Renderer tidak semestinya memperoleh akses Node.js penuh. Preload script berjalan sebelum konten halaman dan menjadi lapisan perantara untuk mengekspos kemampuan terbatas. Dengan context isolation, kode preload dan kode halaman berada pada JavaScript context berbeda. `contextBridge` dapat menyediakan API sempit tanpa menyerahkan objek privileged seperti `ipcRenderer` secara langsung.

```javascript
const { contextBridge, ipcRenderer } = require("electron");

contextBridge.exposeInMainWorld("desktop", {
  openFile: () => ipcRenderer.invoke("file:open"),
});
```

Inter-process communication (IPC) menghubungkan renderer dan main process. Channel dapat menangani pesan satu arah atau pola request-response. Handler main process harus menganggap argument dari renderer sebagai input tidak tepercaya: validasi payload, batasi operasi yang tersedia, dan periksa sender sebelum membaca data sensitif atau melakukan tindakan privileged. IPC yang sinkron atau terlalu sering juga dapat menjadi bottleneck.

Utility process dapat memindahkan pekerjaan Node.js yang berat atau rentan crash dari main process. Worker dan proses tambahan tidak otomatis mempercepat aplikasi; serialisasi pesan, lifecycle, serta penggunaan memori tetap perlu diukur.

## Akses native

Main process menyediakan modul untuk window, menu, dialog, clipboard, tray, shortcut, notification, protocol, file system melalui Node.js, dan integrasi sistem lain. Kemampuan ini menjadi kelebihan Electron dibanding aplikasi web biasa, sekaligus memperbesar dampak kerentanan. XSS pada situs web biasanya terbatas oleh browser; pada konfigurasi Electron yang buruk, kode renderer dapat mencapai API sistem dan berubah menjadi remote code execution.

Native Node modules didukung, tetapi binary addon harus cocok dengan ABI Electron, platform, dan arsitektur target. Modul yang dibangun untuk Node.js biasa mungkin perlu dikompilasi ulang dengan `@electron/rebuild`. Upgrade Electron juga dapat mengharuskan rebuild. Dependensi native mengurangi portabilitas yang biasanya diasosiasikan dengan satu basis kode dan menambah variasi build CI.

## Keamanan

Konten remote atau tidak tepercaya tidak boleh dijalankan dengan Node integration aktif. Renderer sebaiknya memakai sandbox dan context isolation, sementara preload hanya mengekspos fungsi yang benar-benar diperlukan. Sandbox renderer aktif secara default sejak Electron 20; mengaktifkan `nodeIntegration` pada renderer menonaktifkan sandbox untuk proses tersebut.

Pertahanan lain tetap dibutuhkan: gunakan Content Security Policy, batasi navigation dan pembuatan window, jangan meneruskan URL tidak tepercaya ke `shell.openExternal`, tangani permission request secara eksplisit, validasi sender IPC, dan gunakan protocol aplikasi yang aman. Menaruh seluruh kode dalam ASAR tidak menjadikannya rahasia atau aman; ASAR hanya format archive yang memudahkan packaging dan menghalangi inspeksi kasual.

Electron mendistribusikan Chromium dan Node.js bersama aplikasi. Karena itu, menunda upgrade berarti ikut menunda patch keamanan dari komponen tersebut. Pembaruan rutin harus disertai pengujian breaking changes dan regresi pada platform target.

## Performa dan penggunaan sumber daya

Electron sering memakai lebih banyak disk dan memori daripada aplikasi yang memakai runtime bersama atau toolkit native karena setiap aplikasi membawa runtime serta process tree sendiri. Namun, label "resource-intensive" tidak cukup untuk menilai satu aplikasi. Jumlah window, dependency, startup work, ukuran bundle, request jaringan, rendering UI, dan pekerjaan main process menentukan biaya aktual.

Main process berada pada jalur lifecycle, window, dan komunikasi aplikasi. Operasi CPU berat, synchronous IPC, serta blocking file I/O dapat membuat seluruh UI terasa macet. Renderer mengikuti prinsip performa web: kurangi JavaScript yang tidak perlu, tunda pekerjaan nonkritis, hindari render berlebihan, dan ukur dengan [[References/Chrome DevTools\|Chrome DevTools]]. Profil CPU, heap, startup, serta tracing lintas proses lebih berguna daripada menyimpulkan performa hanya dari framework.

Pemakaian teknologi web dapat mempercepat pengembangan ketika tim sudah menguasai ekosistem frontend dan ingin berbagi UI atau logic. Kecepatan awal tersebut dapat berkurang jika aplikasi membutuhkan integrasi native mendalam, konsumsi memori sangat rendah, startup ketat, atau banyak perilaku platform-specific.

## Packaging dan distribusi

Kode pengembangan harus dipaketkan bersama runtime Electron, dependency, aset, dan metadata aplikasi. Electron Forge menyediakan pipeline untuk membuat proyek, package aplikasi, menghasilkan distributable melalui Maker, dan mengirim artifact melalui Publisher. Format seperti DMG, ZIP, MSI, Squirrel.Windows, deb, atau Flatpak memiliki dukungan dan kebutuhan host build yang berbeda; satu perintah tidak selalu dapat menghasilkan seluruh target dari satu sistem operasi.

Code signing membuktikan identitas penerbit dan membantu sistem operasi memeriksa bahwa artifact tidak diubah. Distribusi macOS umumnya memerlukan signing dan notarization, sedangkan Windows menggunakan signing pada executable atau installer. Sertifikat, layanan signing, serta credential CI merupakan bagian dari operasi rilis, bukan detail kosmetik.

Electron menyediakan `autoUpdater` dan mendukung update feed. Update otomatis tetap memerlukan artifact yang benar, metadata rilis, signing, rollout, dan penanganan kegagalan. Kanal pembaruan juga menjadi jalur supply-chain berprivilege tinggi sehingga credential publisher dan server update harus dilindungi.

## Aksesibilitas dan pengujian

Karena UI renderer berbasis HTML, dasar aksesibilitasnya sama dengan web: struktur semantik, label, navigasi keyboard, focus, contrast, dan pengujian assistive technology. Chromium dapat mengaktifkan accessibility tree ketika teknologi bantu terdeteksi. Integrasi desktop seperti menu, shortcut, dialog, dan tray juga perlu diuji pada sistem operasi sebenarnya.

Unit test untuk logic web tidak menggantikan pengujian paket desktop. Uji alur main-renderer, IPC, permission, installer, signing, update, crash recovery, dan native dependency pada target platform. Electron membutuhkan display driver untuk pengujian GUI; CI Linux headless dapat memakai virtual display seperti Xvfb.

## Kapan Electron sesuai

Electron cocok untuk tim web yang membutuhkan aplikasi desktop lintas platform, UI kompleks, pembaruan terkontrol, dan akses native yang masih dapat ditempatkan di balik API sempit. Contoh penggunaan populer menunjukkan bahwa framework mampu mendukung aplikasi besar, tetapi tidak membuktikan bahwa Electron selalu menjadi pilihan terbaik.

Bandingkan kebutuhan dengan PWA, toolkit native, Tauri, Qt, Flutter, atau WebView sistem. Pertimbangkan ukuran distribusi, memori, startup, integrasi OS, keamanan, kemampuan tim, dan biaya rilis. Pilih Electron karena model proses dan ekosistemnya sesuai kebutuhan, bukan hanya karena kode awal dapat ditulis dengan HTML dan JavaScript.

## Sumber

1. Introduction — Electron: https://www.electronjs.org/docs/latest/
2. Process Model — Electron: https://www.electronjs.org/docs/latest/tutorial/process-model
3. Process Sandboxing — Electron: https://www.electronjs.org/docs/latest/tutorial/sandbox
4. Inter-Process Communication — Electron: https://www.electronjs.org/docs/latest/tutorial/ipc
5. Security — Electron: https://www.electronjs.org/docs/latest/tutorial/security
6. Performance — Electron: https://www.electronjs.org/docs/latest/tutorial/performance
7. Packaging Your Application — Electron: https://www.electronjs.org/docs/latest/tutorial/tutorial-packaging
8. Updating Applications — Electron: https://www.electronjs.org/docs/latest/tutorial/updates
9. Code Signing — Electron: https://www.electronjs.org/docs/latest/tutorial/code-signing
10. ASAR Archives — Electron: https://www.electronjs.org/docs/latest/tutorial/asar-archives
11. Native Node Modules — Electron: https://www.electronjs.org/docs/latest/tutorial/using-native-node-modules
12. Accessibility — Electron: https://www.electronjs.org/docs/latest/tutorial/accessibility
13. Getting Started — Electron Forge: https://www.electronforge.io/
14. Makers — Electron Forge: https://www.electronforge.io/config/makers
