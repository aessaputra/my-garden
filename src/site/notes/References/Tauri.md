---
{"dg-publish":true,"dg-path":"Tauri.md","permalink":"/tauri/","title":"Tauri","hideInFiletree":true,"tags":["references","programming","javascript","security","performance"],"noteIcon":"","dg-note-properties":{"title":"Tauri","category":"references","tags":["references","programming","javascript","security","performance"],"updated":"2026-08-29","sources":["https://v2.tauri.app/start/","https://v2.tauri.app/concept/architecture/","https://v2.tauri.app/concept/process-model/","https://v2.tauri.app/concept/inter-process-communication/","https://v2.tauri.app/security/","https://v2.tauri.app/security/capabilities/","https://v2.tauri.app/security/permissions/","https://v2.tauri.app/security/csp/","https://v2.tauri.app/develop/calling-rust/","https://v2.tauri.app/concept/size/","https://v2.tauri.app/distribute/","https://v2.tauri.app/distribute/sign/macos/","https://v2.tauri.app/distribute/sign/windows/","https://v2.tauri.app/blog/tauri-20/"]}}
---

Tauri adalah framework untuk membangun aplikasi desktop dan seluler dengan webview sistem operasi. [[References/JavaScript\|JavaScript]], [[References/HTML\|HTML]], dan [[References/CSS\|CSS]] menjadi antarmuka pengguna yang diproses di webview, sementara logika backend dan integrasi sistem operasi ditulis dalam Rust. Berbeda dengan framework yang memaketkan mesin browser, Tauri tidak menyertakan runtime Chromium atau Node.js. Ia memakai webview yang sudah tersedia pada tiap sistem operasi: WKWebView pada macOS, WebView2 pada Windows, dan webkit2gtk pada Linux.

Akibatnya, aplikasi Tauri tanpa mesin terdistribusi biasanya memiliki ukuran biner jauh lebih kecil dari aplikasi setara berbasis framework yang memaketkan browser, tetapi kompatibilitas fitur web bergantung pada versi webview yang dipasang oleh sistem pengguna. Tersedia pada desktop (Windows, macOS, Linux) dan seluler (Android, iOS) sejak rilis 2.0.

Arsitektur memisahkan kepercayaan antara webview dan inti Rust. Frontend berkomunikasi ke backend melalui layer IPC yang terdefinisi erat, melalui perintah yang dikendalikan izin, bukan melalui ekspor objek global sistem ke konteks webview.

## Model proses

Proses inti adalah titik masuk aplikasi dan satu-satunya komponen dengan akses penuh ke sistem operasi. Proses ini bertanggung jawab menciptakan dan mengelola jendela, menu, tray, dan notifikasi, serta mengarahkan semua komunikasi antarproses (IPC). Semua pesan IPC lewat proses inti, sehingga dapat diawasi, disaring, dan dimanipulasi di satu tempat.

Webview berjalan dalam proses terpisah untuk setiap jendela. Webview adalah lingkungan mirip browser yang mengeksekusi aset frontend; ia tidak punya akses langsung ke sistem operasi. Akses ke perintah Rust dan plugin dikendalikan oleh sistem kapabilitas.

Model multi-process memungkinkan isolasi kegagalan dan pembatasan jangkauan eksploit: jika sebuah webview terganggu, proses lain tidak otomatis ikut gagal. Prinsip hak istimewa paling sedikit diterapkan dengan memberi tiap proses hanya izin yang dibutuhkan untuk pekerjaannya.

## Akses native

Tauri tidak mengekspos API Node.js atau objek global sistem ke webview. Sebagai gantinya, pengembang mendaftarkan perintah Rust yang dipanggil dari JavaScript lewat fungsi `invoke`. Perintah dapat menerima argumen, mengembalikan nilai, dan bersifat async. Seluruh argumen dan nilai balik harus dapat diserialisasi ke JSON.

IPC Tauri menggunakan JSON sebagai format serialisasi. Versi 2.0 menambah dukungan payload mentah untuk transfer data besar dan sistem protokol khusus untuk meningkatkan throughput pada transfer berulang. Layer komunikasi dibangun di atas pola message passing; proses penerima dapat menolak atau membuang permintaan.

Dua primitif IPC tersedia: Events untuk notifikasi satu arah, dan Commands untuk memanggil fungsi Rust dengan nilai kembali. Keduanya dikendalikan oleh sistem izin.

## Keamanan

Model keamanan Tauri membedakan dua zona kepercayaan: kode inti Rust dan kode frontend di webview. Semua data yang melewati batas harus diverifikasi; layer IPC adalah penjaga batas tersebut.

Kapabilitas mendefinisikan izin yang diberikan atau ditolak untuk jendela dan webview tertentu, disusun dalam file JSON atau TOML di direktori `capabilities`. Secara default, semua perintah plugin diblokir; hanya perintah yang secara eksplisit diizinkan oleh kapabilitas yang berjalan. Akses jarak jauh ke perintah hanya boleh untuk origin yang dipercaya dan dikonfigurasi secara eksplisit.

Izin (permissions) mendeskripsikan hak istimewa sebuah perintah, dapat mengaktifkan atau memblokir perintah, mendefinisikan scope, dan digabungkan menjadi permission set. Sistem ini memperkecil serangan surface frontend dan mencegah eskalasi hak istimewa dari webview ke inti.

Model ini tidak melindungi dari kode Rust yang tidak aman, scope yang terlalu longgar, atau eksploit pada webview sistem yang belum diperbarui. Tauri menjalani audit keamanan untuk rilis mayor dan minor, tetapi audit tidak menghilangkan semua risiko; tim tetap harus memantau kerentanan pada dependensi Rust dan JavaScript.

## Performa dan penggunaan sumber daya

Klaim ukuran paket yang lebih kecil berasal dari tidak memaketkan mesin browser. Namun ukuran sebenarnya bergantung pada aset frontend, plugin yang dimuat, dan ketersediaan runtime webview di sistem pengguna.

Di Windows, WebView2 perlu diinstal atau diunduh untuk sistem lama; pada Linux, webkit2gtk harus tersedia sebagai dependensi sistem. Dukungan multimedia dapat menambah ukuran paket jika GStreamer diaktifkan.

Penggunaan memori aplikasi dipengaruhi oleh webview sistem, bukan oleh keputusan Tauri. Profiling dapat dilakukan dengan [[References/Chrome DevTools\|Chrome DevTools]] untuk renderer dan toolchain Rust asli untuk proses inti.

## Packaging dan distribusi

Tauri CLI menyediakan perintah `dev` dan `build` untuk pengembangan dan produksi. Sebelum build, frontend dikompilasi menjadi aset statis yang ditunjuk oleh `frontendDist` di `tauri.conf.json`.

Distribusi mencakup signing, bundling, dan opsi publikasi per platform: DMG dan App Store untuk macOS, MSI dan EXE untuk Windows, serta paket DEB, RPM, dan AppImage untuk Linux. Code signing dibutuhkan untuk distribusi luar toko pada macOS (termasuk notarisasi) dan direkomendasikan pada Windows guna menghindari peringatan SmartScreen.

Code signing tidak sama dengan notarisasi, penerbitan, atau pembaruan otomatis. Tauri mendukung pembaruan in-app melalui plugin updater dengan penandatanganan digital yang wajib sebagai verifikasi asal usul.

## Aksesibilitas dan pengujian

Pengujian unit, integrasi, dan end-to-end didukung. Tes end-to-end memakai protokol WebDriver pada Windows, Linux, dan macOS. Proses inti dapat didiagnosis dengan debugger asli Rust seperti GDB atau LLDB. Aksesibilitas dijalankan oleh webview sistem dan ditentukan oleh kemampuan OS.

## Kapan Tauri sesuai

Pilih Tauri jika ukuran bundle kecil dan keamanan berbasis Rust menjadi prioritas, frontend web cocok untuk aplikasi, dan tim bersedia memakai toolchain Rust untuk logika backend.

Pilih alternatif lain jika tim tidak siap mempelajari Rust, kebutuhan bergantung pada perilaku web tertentu yang tidak selalu konsisten antar webview sistem, atau aplikasi memerlukan ekosistem Node.js yang dapat diinstal kembali. Ketergantungan pada webview sistem berarti perilaku frontend dapat bervariasi antar platform dan versi OS, hal yang harus diuji per platform. Bandingkan dengan [[References/Electron\|Electron]] bila kerangka tersebut lebih cocok karena model proses dan akses Node.js penuh yang disediakannya, dan pilih berdasarkan kebutuhan nyata, bukan karena asumsi ukuran atau kecepatan yang tidak diukur.

## Sumber

1. What is Tauri — Tauri: https://v2.tauri.app/start/
2. Tauri Architecture — Tauri: https://v2.tauri.app/concept/architecture/
3. Process Model — Tauri: https://v2.tauri.app/concept/process-model/
4. Inter-Process Communication — Tauri: https://v2.tauri.app/concept/inter-process-communication/
5. Security — Tauri: https://v2.tauri.app/security/
6. Capabilities — Tauri: https://v2.tauri.app/security/capabilities/
7. Permissions — Tauri: https://v2.tauri.app/security/permissions/
8. Content Security Policy — Tauri: https://v2.tauri.app/security/csp/
9. Calling Rust from the Frontend — Tauri: https://v2.tauri.app/develop/calling-rust/
10. App Size — Tauri: https://v2.tauri.app/concept/size/
11. Distribute — Tauri: https://v2.tauri.app/distribute/
12. macOS Code Signing — Tauri: https://v2.tauri.app/distribute/sign/macos/
13. Windows Code Signing — Tauri: https://v2.tauri.app/distribute/sign/windows/
14. Tauri 2.0 Stable Release — Tauri: https://v2.tauri.app/blog/tauri-20/
