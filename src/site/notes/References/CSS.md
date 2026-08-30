---
{"dg-publish":true,"dg-path":"CSS.md","permalink":"/css/","title":"CSS","hideInFiletree":true,"tags":["network","web","css","guide"],"dg-note-properties":{"title":"CSS","category":"references","tags":["network","web","css","guide"],"sources":["_raw/articles/css-what-is-mdn.md","_raw/articles/css-styling-basics-mdn.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

CSS (Cascading Style Sheets) adalah bahasa untuk style dan layout web pages: mengubah font, warna, ukuran, spacing konten, membagi kolom, menambah animasi. Halaman ini menyintesis "What is CSS?" dan module "Styling basics" dari MDN.

## Peran CSS dalam web

HTML menangani struktur dan konten; CSS menangani penampilan/presentation; JavaScript menangani perilaku. Browser menerapkan default styles sendiri (headings lebih besar, links berwarna/bergaris bawah) agar halaman tetap terbaca tanpa styling eksplisit. CSS menggantikan default itu dengan desain apa pun yang diinginkan.

Document: file teks terstruktur (HTML, SVG, XML). Presenting: mengubah dokumen jadi bentuk yang bisa dipakai (rendering di browser). Browser kadang disebut user agent.

## Sintaks dasar

CSS adalah rule-based language. Rule terdiri dari selector + declaration block:

```css
h1 {
  color: red;
  font-size: 2.5em;
}
```

- **Selector**: memilih elemen HTML yang di-style (`h1`, `p`, class, id).
- **Declaration block**: kurung kurawal `{ }`.
- **Declaration**: pasangan property (sebelum colon) dan value (setelah colon), contoh `color: red`.
- Stylesheet = banyak rules ditulis berurutan.

Property punya allowable values berbeda (`color` terima color values, `font-size` terima size units). MDN property pages untuk lookup.

## Bagaimana CSS diterapkan ke HTML

1. Browser terima HTML, ubah jadi **DOM tree**.
2. CSS rules (inline atau `.css` eksternal) diurutkan ke bucket per elemen sesuai selectors.
3. Rules diterapkan ke DOM tree, hasilnya **render tree**.
4. Render tree di-paint ke browser window.

Contoh: rule `h1 { color: red }` mewarnai semua elemen `<h1>`; rule `p { ... }` memengaruhi semua paragraf.

## Topik lanjutan (module Styling basics)

- Selectors: type/class/id, attribute, pseudo-classes/pseudo-elements, combinators (child/sibling).
- Box model: semua elemen punya box; kunci layout kompleks.
- Handling conflicts: cascade, specificity, inheritance.
- Values and units: value types per property (length, percentage, color, dsb).
- Sizing, backgrounds and borders, overflow, images/media/forms, tables.
- Debugging CSS dengan DevTools.
- Lanjutan: box shadows, blend modes, filters, cascade layers, writing modes, organizing CSS.

Sumber module: MDN Styling basics (tutorial + challenges + test your skills).

## Lihat juga

- [[References/HTML\|HTML]]: struktur yang di-style oleh CSS (selector menarget elemen HTML)
- [[References/Web Browser\|Web Browser]]: bagaimana browser menerapkan CSS ke DOM → render tree → paint
- [[References/HTTP\|HTTP]]: protokol yang mengirim file CSS dari server
- [[References/Web Hosting\|Web Hosting]]: server tempat file CSS disimpan