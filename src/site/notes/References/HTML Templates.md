---
{"dg-publish":true,"dg-path":"HTML Templates.md","permalink":"/html-templates/","title":"HTML Templates","hideInFiletree":true,"tags":["references","javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"HTML Templates","categories":["Web Components","Web APIs"],"tags":["references","javascript","development","programming"],"sources":["_raw/articles/html-templates-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

HTML Templates menggunakan elemen `<template>` sebagai cetak biru markup yang dapat digunakan ulang. Pada penggunaan biasa, kontennya tidak langsung dirender dan baru dipasang melalui JavaScript.

[HTML Standard](https://html.spec.whatwg.org/multipage/scripting.html#the-template-element) menjelaskan template sebagai fragmen HTML untuk dikloning dan disisipkan oleh script.

## Cara kerja

Konten template berada dalam `DocumentFragment` melalui properti `template.content`, bukan sebagai child DOM biasa dari elemen template. Script dalam template document tetap inert sebelum aktivasi melalui penggunaan kontennya.

[MDN menjelaskan](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template) bahwa `content` bersifat read-only sebagai properti, tetapi subtree di dalam fragment tetap dapat dimanipulasi.

Alurnya: ambil elemen template, salin `content`, isi data pada salinan, lalu sisipkan salinan ke DOM biasa atau shadow root. Template tetap tersedia bagi instance berikutnya.

Gunakan `document.importNode(template.content, true)` untuk salinan mendalam dalam konteks document tujuan. `template.content.cloneNode(true)` juga menyalin subtree, tetapi memakai document asal saat cloning.

Perbedaan document menentukan registry untuk konstruksi custom element. [MDN merekomendasikan importNode](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode) untuk konten template yang memuat custom elements.

Menyisipkan `template.content` secara langsung memindahkan children dan mengosongkan fragment sumber. [DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment) bukan wrapper yang ikut terpasang.

## Hubungan dengan Web Components

[[References/Web Components\|Web Components]] menggabungkan primitive yang berbeda. Custom Elements memberi nama elemen dan lifecycle; Shadow DOM memberi batas tree; template menyediakan struktur berulang; slots menyediakan titik komposisi.

Custom element dapat mengambil template, mengimpor salinannya, lalu memasangnya pada shadow root. Setiap instance memperoleh struktur internal sendiri tanpa menulis ulang markup tersebut.

[Contoh MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_templates_and_slots) memakai pola tersebut. Style menjadi terenkapsulasi karena dipasang di shadow root, bukan karena berasal dari template.

`<slot name="title">` menerima light DOM dengan `slot="title"`. Slot tanpa nama menerima konten yang tidak diberi nama slot; fallback tampil ketika tidak ada node yang ditugaskan.

Slot mengomposisikan node consumer tanpa menjadikannya child internal shadow tree. Template tetap berguna tanpa custom element, sedangkan slot memerlukan konteks Shadow DOM untuk fungsi distribusinya.

## Declarative Shadow DOM

`<template shadowrootmode="open">` adalah jalur berbeda: parser dapat membuat shadow root pada parent yang memenuhi syarat. Struktur shadow dapat tersedia tanpa JavaScript untuk memanggil `attachShadow()`.

[MDN membedakan](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template#declarative_shadow_dom) jalur deklaratif ini dari template fragment biasa. Karena itu, tidak semua penggunaan template menunggu cloning manual.

## Batas dan praktik

Template biasa bukan sistem data binding atau reactive rendering. Perubahan data aplikasi memerlukan pembaruan node melalui kode sendiri atau pustaka yang mengelola rendering.

Cloning tidak menyalin listener dari `addEventListener()` atau properti `onclick`, sesuai [aturan cloning](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode).

Pasang listener pada hasil salinan atau gunakan delegasi pada ancestor yang sesuai. Pilihan ini harus mengikuti struktur event dan kebutuhan tiap instance.

Hindari ID duplikat ketika salinan masuk ke document yang sama. Konten inert juga bukan bukti bahwa markup tidak tepercaya telah disanitasi sebelum dipasang.

Efisiensi utamanya adalah reuse struktur dan pemisahan inisialisasi instance. [MDN mengingatkan](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment#performance) bahwa keuntungan performa fragment sering dibesar-besarkan.

Tidak ada benchmark atau pengujian lintas browser dalam riset ini. Fitur template terbaru seperti out-of-order patching tidak dibahas; cakupan berfokus pada fragment dan hubungan dengan Web Components.

## Lanjutan

- [[Cloning template content preserves the reusable blueprint\|Cloning template content preserves the reusable blueprint]]
- [[Template reuse does not provide reactive rendering\|Template reuse does not provide reactive rendering]]
- [[Templates stay inert while slots preserve consumer ownership\|Templates stay inert while slots preserve consumer ownership]]
