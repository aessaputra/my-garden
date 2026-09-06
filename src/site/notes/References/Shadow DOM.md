---
{"dg-publish":true,"dg-path":"Shadow DOM.md","permalink":"/shadow-dom/","title":"Shadow DOM","hideInFiletree":true,"tags":["references","javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Shadow DOM","categories":["Web Components","Web APIs"],"tags":["references","javascript","development","programming"],"sources":["_raw/articles/shadow-dom-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Shadow DOM adalah mekanisme browser untuk memasang DOM tree terpisah pada elemen host. Batas tree membantu menjaga struktur internal dan styling komponen dari perubahan tidak sengaja oleh halaman pemakai.

[MDN menjelaskan](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) shadow host, shadow root, shadow tree, dan shadow boundary. Mekanisme ini bukan sandbox JavaScript atau jaminan kerahasiaan.

## Struktur dan pembuatan

Host tetap berada di document tree. Shadow root menjadi akar tree internal yang terpasang pada host; light DOM adalah konten biasa di luar tree internal tersebut.

`host.attachShadow({ mode: "open" })` membuat shadow root dan mengembalikan referensinya. [attachShadow](https://developer.mozilla.org/en-US/docs/Web/API/Element/attachShadow) hanya berlaku pada elemen host yang memenuhi syarat.

Node dapat ditambahkan ke root melalui DOM API, termasuk salinan struktur dari [[References/HTML Templates\|HTML Templates]]. [[References/Custom Elements\|Custom Elements]] memberi registrasi dan lifecycle, bukan syarat mutlak untuk memakai Shadow DOM.

Host biasa seperti `<div>` dapat memiliki shadow root. Sebaliknya, custom element dapat menggunakan light DOM ketika enkapsulasi tree tidak diperlukan.

## Open dan closed

Pada mode `open`, kode dapat memperoleh root melalui `host.shadowRoot`. Pada mode `closed`, properti tersebut mengembalikan `null`; kode pembuat tetap memperoleh referensi dari `attachShadow()`.

Query seperti `document.querySelectorAll()` tidak otomatis masuk ke shadow tree. Pada root terbuka, kode dapat mengakses `host.shadowRoot` lalu menjalankan query di sana.

[MDN memperingatkan](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) bahwa closed root bukan mekanisme keamanan kuat. Batas akses ini terutama mencegah ketergantungan tidak sengaja pada implementasi.

Shadow DOM tidak membuat origin, realm JavaScript, atau sistem izin terpisah. Jangan menggunakannya untuk menyembunyikan rahasia, menjalankan kode tidak tepercaya, atau menggantikan otorisasi.

## CSS dan theming

Selector halaman biasa tidak menargetkan node internal shadow tree. Selector internal juga memiliki scope sendiri, sehingga class generik komponen lebih terlindung dari benturan dengan stylesheet halaman.

[CSS scoping](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scoping) menjelaskan batas selector tersebut. Ini bukan pemutusan seluruh hubungan style: inheritance dan mekanisme styling publik tetap berlaku.

`:host` menargetkan host dari stylesheet internal. Custom properties dapat membawa nilai tema dari luar, sehingga komponen dapat memakai warna atau ukuran yang disediakan consumer.

[CSSWG](https://drafts.csswg.org/css-scoping/) menjelaskan custom properties sebagai sarana melewatkan nilai ke dalam komponen. Nama property publik sebaiknya diperlakukan sebagai kontrak, bukan detail yang bebas diubah.

Atribut `part` pada elemen internal membuka styling melalui `::part()`. [CSS shadow parts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_shadow_parts) memungkinkan author mengekspos bagian tertentu tanpa membuka seluruh struktur.

Stylesheet dapat ditempatkan dalam `<style>` atau dipasang melalui `adoptedStyleSheets`. [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) menjelaskan berbagi constructed stylesheet antar tree.

## Slots dan events

Slots mengomposisikan light DOM consumer dengan struktur shadow. Node yang ditugaskan tetap berasal dari light DOM, bukan berubah menjadi child internal shadow tree; pola ini dijelaskan dalam [[References/HTML Templates\|HTML Templates]].

[Event.composed](https://developer.mozilla.org/en-US/docs/Web/API/Event/composed) menentukan apakah event dapat melewati shadow boundary. Closed root tidak berarti semua event berhenti di dalam komponen.

`composedPath()` membantu memeriksa jalur propagasi. [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Event/composedPath) menunjukkan bahwa jalur yang terlihat dari luar closed root tidak menyertakan node internalnya.

Dokumentasikan event publik dan aturan propagasinya. Hindari membuat consumer bergantung pada node internal yang muncul dalam jalur event root terbuka, karena struktur tersebut dapat berubah.

## Declarative Shadow DOM

`<template shadowrootmode="open">` memungkinkan parser membuat shadow root pada parent yang memenuhi syarat. Jalur ini dapat membawa struktur shadow melalui HTML server tanpa pemanggilan `attachShadow()` manual.

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) membedakan pembuatan deklaratif dari API JavaScript. Struktur yang sudah tampil tidak otomatis memiliki seluruh perilaku interaktif aplikasi.

## Pemilihan dan batas

Dalam [[References/Web Components\|Web Components]], Shadow DOM berguna ketika komponen memerlukan batas tree dan styling. Ia opsional, bukan syarat semua komponen reusable atau pengganti API publik yang baik.

Enkapsulasi tetap menyisakan pekerjaan untuk theming, focus, keyboard, semantik, dan integrasi framework. Ringkasnya markup pemakai tidak membuktikan bahwa komponen sudah mudah dipelihara atau aksesibel.

Riset ini memakai dokumentasi MDN dan CSSWG, tanpa benchmark, audit keamanan, atau pengujian browser. Bagian eksperimental dalam editor draft tidak dijadikan rekomendasi implementasi.

## Lanjutan

- [[Closed shadow roots are not security boundaries\|Closed shadow roots are not security boundaries]]
- [[Shadow styling needs an explicit public contract\|Shadow styling needs an explicit public contract]]
- [[Shadow DOM reduces collisions without eliminating integration work\|Shadow DOM reduces collisions without eliminating integration work]]
- [[Templates stay inert while slots preserve consumer ownership\|Templates stay inert while slots preserve consumer ownership]]
