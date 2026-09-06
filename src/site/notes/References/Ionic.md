---
{"dg-publish":true,"dg-path":"Ionic.md","permalink":"/ionic/","title":"Ionic","hideInFiletree":true,"tags":["references","programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"Ionic","categories":["Frameworks"],"tags":["references","programming","architecture"],"sources":["_raw/articles/ionic-research-packet.md"],"created":"2026-09-07","updated":"2026-09-07"}}
---


Ionic adalah toolkit UI open-source berlisensi MIT untuk aplikasi berbasis HTML, CSS, dan JavaScript. Komponennya dapat digunakan pada web, Android, dan iOS dengan integrasi resmi Angular, React, serta Vue.

[Dokumentasi Ionic](https://ionicframework.com/docs) menempatkan fokusnya pada kontrol, interaksi, gesture, dan animasi frontend. Framework ini bukan backend ataupun runtime native yang berdiri sendiri.

## Arsitektur

Inti Ionic merupakan pustaka [[References/Web Components\|Web Components]], memakai Custom Elements dan Shadow DOM. Integrasi framework membungkus komponen agar sesuai dengan pola aplikasi [[References/Angular\|Angular]], [[References/React\|React]], dan [[References/Vue.js\|Vue.js]].

Komponen juga dapat digunakan tanpa framework frontend melalui script. Portabilitas ini tidak berarti seluruh routing, lifecycle, atau konfigurasi aplikasi otomatis identik pada setiap integrasi.

Pada aplikasi seluler, UI Ionic dirender dalam WebView. [Core concepts](https://ionicframework.com/docs/core-concepts/fundamentals) membedakan rendering web dari akses SDK native melalui Capacitor atau Cordova.

[Capacitor](https://capacitorjs.com/docs) adalah runtime native lintas platform untuk proyek web. Plugin menyediakan akses perangkat pada platform yang mendukungnya, dengan implementasi native dan web yang sesuai.

## Komponen dan tampilan

Ionic menyediakan elemen seperti button, header, toolbar, content, serta page container. [Quickstart React](https://ionicframework.com/docs/react/quickstart) memperlihatkan penyusunan layar dari komponen tersebut.

Adaptive Styling memakai mode `ios` dan `md` untuk menyesuaikan tampilan dan perilaku. [Theming](https://ionicframework.com/docs/theming/basics) menjelaskan konfigurasi mode, CSS custom properties, dan CSS Shadow Parts.

Tampilan menyerupai native bukan berarti komponen berubah menjadi widget sistem operasi. Identitas merek dapat disesuaikan melalui tema tanpa mengganti fondasi rendering web.

## Navigasi dan integrasi framework

Ionic mendukung navigation history paralel, termasuk stack terpisah untuk tab. Pola ini berbeda dari history web linear, sebagaimana dijelaskan dalam [fundamentals](https://ionicframework.com/docs/core-concepts/fundamentals).

Pada Ionic React, `IonRouterOutlet` menampilkan halaman dan `IonPage` diperlukan untuk transisi serta layout. Ikuti kontrak integrasi dalam [quickstart](https://ionicframework.com/docs/react/quickstart), bukan markup visual saja.

## Akses native dan build

Plugin menjembatani JavaScript dengan API native. Jika integrasi belum tersedia, pengembang dapat membuat plugin lokal atau publik menurut [panduan plugin](https://capacitorjs.com/docs/plugins/creating-plugins).

Satu codebase tidak menghapus proyek Android dan iOS. Alur [Capacitor](https://capacitorjs.com/docs/basics/workflow) mencakup build web, sinkronisasi bundle, pengujian perangkat, dan kompilasi native.

`npx cap sync` menyalin bundle web yang sudah dibangun sekaligus memperbarui dependensi native. Proyek dapat dibuka melalui Xcode atau Android Studio untuk kontrol dan kompilasi platform.

Dukungan API harus diperiksa per platform. [[Mobile permissions are revocable feature dependencies\|Mobile permissions are revocable feature dependencies]] memberi dasar untuk merancang keadaan izin ditolak atau kapabilitas tidak tersedia.

## Kecocokan dan batas bukti

Ionic layak dievaluasi ketika tim ingin menggunakan keahlian web dan berbagi UI lintas target. Mulailah dari kebutuhan integrasi tersulit, bukan asumsi bahwa semua kode platform dapat dihapus.

Dokumentasi menyebut transisi hardware-accelerated dan gesture teroptimasi sebagai tujuan performa. [Pernyataan vendor](https://ionicframework.com/docs) tersebut bukan benchmark atau jaminan hasil setiap aplikasi.

Riset ini tidak menjalankan aplikasi, mengukur performa, atau mengaudit aksesibilitas. Harga layanan komersial, versi terbaru, dan lifecycle dukungan berada di luar cakupan.

## Sumber

Paket bukti dan snapshot lengkap: [[_raw/articles/ionic-research-packet\|Ionic research packet]]. Tujuh dokumen resmi diakses pada 7 September 2026 melalui 9Router; tanggal publikasi tidak diverifikasi secara independen.

## Catatan atomik

- [[Ionic separates web interfaces from native integration\|Ionic separates web interfaces from native integration]]
- [[Adaptive styling does not change the rendering model\|Adaptive styling does not change the rendering model]]
