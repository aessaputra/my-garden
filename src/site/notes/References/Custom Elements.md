---
{"dg-publish":true,"dg-path":"Custom Elements.md","permalink":"/custom-elements/","title":"Custom Elements","hideInFiletree":true,"tags":["references","javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Custom Elements","categories":["Web Components","Web APIs"],"tags":["references","javascript","development","programming"],"sources":["_raw/articles/custom-elements-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Custom Elements adalah API browser untuk mendefinisikan elemen HTML beserta perilakunya melalui class JavaScript. Sebagai bagian [[References/Web Components\|Web Components]], API ini memberi kontrak elemen yang dapat digunakan ulang.

[HTML Standard](https://html.spec.whatwg.org/multipage/custom-elements.html) mengatur construction, registration, upgrade, dan lifecycle. Tag seperti `<product-card>` merangkum implementasi, bukan menghapus kebutuhan markup internal.

## Jenis dan registrasi

Autonomous custom elements mewarisi `HTMLElement` dan dipakai sebagai tag sendiri. Customized built-in elements memperluas elemen native melalui interface yang sesuai, opsi `extends`, serta atribut `is`.

[MDN mencatat](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements) Safari tidak mendukung customized built-in elements. Jangan menyamakan dukungannya dengan autonomous elements.

Registrasi seperti `customElements.define("product-card", ProductCard)` menghubungkan nama dengan class implementasi. Definisi class saja belum mendaftarkan elemen pada registry browser.

Nama wajib mengandung tanda hubung, diawali huruf ASCII kecil, dan tidak memakai huruf ASCII besar. [Aturan define](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define) juga melarang nama tertentu.

Registry yang sama menolak registrasi ulang nama atau constructor yang sudah digunakan. Prefix proyek membantu menghindari benturan nama antar pustaka pada registry global.

Elemen dalam dokumen dapat dibuat sebelum definisinya dimuat, lalu di-upgrade setelah registrasi. [HTML Standard](https://html.spec.whatwg.org/multipage/custom-elements.html) membedakan pembuatan elemen dan pemasangan perilaku custom.

## Constructor dan lifecycle

Constructor memanggil `super()` sebelum menggunakan `this`. Gunakan untuk state awal, listener, atau shadow root; jangan bergantung pada attributes dan children yang belum tersedia menurut aturan construction.

[MDN menjelaskan lifecycle](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements) sebagai callback browser terhadap perubahan elemen, bukan fungsi yang perlu dipanggil aplikasi secara manual.

- `connectedCallback()`: elemen tersambung ke document.
- `disconnectedCallback()`: elemen terlepas dari document.
- `adoptedCallback()`: elemen berpindah ke document lain.
- `attributeChangedCallback()`: attribute yang diamati berubah, ditambahkan, atau dihapus.

Connection dapat terjadi berulang. Pisahkan setup satu kali dari pemasangan observer atau subscription yang harus dipulihkan setelah disconnection; guard tunggal tidak cukup bila cleanup menghapus resource penting.

[HTML Standard](https://html.spec.whatwg.org/multipage/custom-elements.html) meminta guard untuk inisialisasi yang benar-benar sekali. Uji removal, reinsertion, dan perpindahan, bukan hanya render pertama.

`connectedMoveCallback()` bersama `Element.moveBefore()` dapat mempertahankan state saat perpindahan. Perlakukan sebagai kemampuan terpisah yang perlu diperiksa pada browser target, bukan asumsi universal.

## Attributes, properties, dan events

Daftarkan nama attribute dalam `static observedAttributes`. Browser meneruskan nama, nilai lama, dan nilai baru ke `attributeChangedCallback()` untuk attribute tersebut.

Callback juga dapat berjalan saat attribute awal diproses, bukan hanya ketika pengguna mengubahnya. [Panduan MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements) menjelaskan urutan ini.

Attributes menjadi konfigurasi deklaratif, sedangkan properties dan methods membentuk API JavaScript. Hubungan antara property, attribute, dan rendering harus diimplementasikan, bukan diasumsikan otomatis.

Tentukan events yang dapat diamati consumer, payload, serta aturan propagasinya. Kontrak ini perlu diuji pada framework pemakai; kemampuan browser saja tidak membuktikan integrasi selalu mulus.

## Hubungan dengan Templates dan Shadow DOM

[[References/HTML Templates\|HTML Templates]] menyediakan struktur yang dapat disalin untuk setiap instance. Shadow DOM memberi batas tree dan styling; custom element dapat memakai light DOM tanpa keduanya.

[Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) dipasang secara terpisah. Mendaftarkan custom tag tidak otomatis mengisolasi DOM atau CSS, dan mode `closed` bukan batas keamanan kuat.

Gunakan templates ketika struktur berulang membutuhkan blueprint. Gunakan Shadow DOM ketika batas styling dan tree memang diperlukan, sambil mempertahankan kontrak slots dan theming yang jelas.

## Semantik, aksesibilitas, dan forms

Nama `<custom-button>` tidak otomatis memperoleh semantik atau interaksi `<button>`. [HTML Standard](https://html.spec.whatwg.org/multipage/custom-elements.html) menekankan bahwa nama tidak menentukan makna bagi accessibility tools.

Utamakan kontrol native dalam komponen bila sesuai. Jika membangun kontrol sendiri, implementasikan accessible name, role, state, focus, keyboard, serta perilaku disabled secara lengkap.

[ElementInternals](https://developer.mozilla.org/en-US/docs/Web/API/ElementInternals) menyediakan default accessibility semantics dan hooks form. Form-associated elements memakai `static formAssociated = true` dan `attachInternals()`.

`setFormValue()` menetapkan nilai submission, sedangkan `setValidity()` mengatur validity. API ini membantu integrasi tetapi tidak membuat setiap custom element otomatis menjadi kontrol form lengkap.

## Batas penerapan

Tag ringkas dapat membuat markup consumer lebih mudah dibaca, tetapi maintainability bergantung pada API, lifecycle, dokumentasi, dan pengujian. Custom Elements bukan pengganti seluruh HTML standar atau framework aplikasi.

Scoped registries menyediakan opsi untuk membatasi definisi pada subtree. Cakupan ini berfokus pada registry global; kompatibilitas fitur terbaru tetap perlu diperiksa untuk browser yang digunakan.

Riset ini memeriksa dokumentasi, bukan benchmark atau pengujian browser dan assistive technology. Perbedaan spesifikasi dan dukungan customized built-ins dipertahankan, bukan dianggap bertentangan.

## Lanjutan

- [[Custom element initialization must survive reconnection\|Custom element initialization must survive reconnection]]
- [[Custom tag names do not supply native semantics\|Custom tag names do not supply native semantics]]
- [[Custom elements make HTML the portability boundary\|Custom elements make HTML the portability boundary]]
- [[Public properties and events determine framework interoperability\|Public properties and events determine framework interoperability]]
