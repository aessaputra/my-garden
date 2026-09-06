---
{"dg-publish":true,"dg-path":"Web Components.md","permalink":"/web-components/","title":"Web Components","hideInFiletree":true,"tags":["references","javascript","development","programming","frameworks"],"noteIcon":"","dg-note-properties":{"title":"Web Components","categories":["Web Components","Web APIs"],"tags":["references","javascript","development","programming","frameworks"],"sources":["_raw/articles/web-components-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Web Components adalah kelompok teknologi platform web untuk membuat elemen HTML kustom yang dapat digunakan ulang. Tiga fondasi utamanya adalah Custom Elements, Shadow DOM, serta HTML Templates dengan Slots.

Teknologi tersebut dapat digunakan bersama, tetapi tidak selalu wajib hadir sebagai satu paket. Custom element dapat memakai light DOM, Shadow DOM dapat digunakan pada host biasa, dan template berguna tanpa custom element.

## Custom Elements

Custom Elements API memungkinkan author mendaftarkan nama elemen dan class implementasinya melalui `CustomElementRegistry`. Nama autonomous custom element memakai tanda hubung untuk menghindari collision dengan elemen HTML masa depan.

[MDN membedakan](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements) autonomous custom elements yang mewarisi `HTMLElement` dan customized built-in elements yang memperluas elemen standar.

Browser mengelola upgrade dan lifecycle elemen. Callback seperti `connectedCallback`, `disconnectedCallback`, `adoptedCallback`, dan `attributeChangedCallback` menghubungkan behavior dengan perubahan keberadaan atau attribute.

Constructor memiliki batas khusus. Setup yang bergantung pada children atau document connection lebih aman ditempatkan pada lifecycle yang sesuai, dan cleanup harus menangani sambungan berulang.

## Shadow DOM

Shadow DOM memasang shadow tree pada host dan membentuk boundary dari document tree biasa. Internal selector serta identifier lebih terlindung dari collision dengan style dan markup sekitar.

[Dokumentasi Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) menjelaskan host, root, tree, dan boundary. Root dapat dibuat secara imperative atau melalui declarative shadow DOM.

Encapsulation bukan isolasi absolut. Inherited properties, CSS custom properties, slots, exposed parts, composed events, focus, dan accessibility tree mengikuti aturan yang dapat menyeberangi boundary.

Mode `closed` membatasi akses melalui `element.shadowRoot`, tetapi bukan security boundary terhadap script lain yang berjalan dalam origin dan execution environment sama.

## Templates dan Slots

Elemen `template` menyimpan subtree yang tidak dirender secara langsung. Code dapat clone atau import content tersebut sebagai struktur berulang untuk shadow tree maupun DOM biasa.

[Templates dan slots](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_templates_and_slots) memisahkan struktur internal dari content consumer. Slot bernama menjadi insertion point bagi light DOM yang tetap dimiliki consumer.

Template bukan rendering engine. Ia tidak menyediakan reactivity, state management, sanitization, diffing, scheduling, atau data binding secara otomatis.

## Public API

API komponen mencakup attributes, properties, methods, events, slots, CSS custom properties, parts, semantics, dan lifecycle assumptions. Stabilitas API ini menentukan apakah reuse benar-benar aman.

Attributes cocok untuk nilai serializable dan declarative markup. Properties dapat menerima nilai JavaScript kompleks, tetapi framework dapat memakai aturan assignment berbeda berdasarkan versi dan runtime.

Events perlu mendokumentasikan nama, payload, bubbling, cancellation, dan `composed`. Event yang tidak composed tidak melewati shadow boundary, sedangkan event composed dapat diamati di luar component.

## Interoperabilitas

Core Custom Elements tersedia pada current browser engines menurut [HTML Standard](https://html.spec.whatwg.org/multipage/custom-elements.html). Itu tidak mencakup seluruh browser lama, tooling, SSR, hydration, atau framework behavior.

[React mendokumentasikan](https://react.dev/reference/react-dom/components#custom-html-elements) cara custom HTML elements menerima attributes, properties, dan event listeners. Kontrak serupa tetap perlu diuji pada framework dan versi target lain.

[Custom Elements Everywhere](https://custom-elements-everywhere.com/) menunjukkan bahwa standard primitive dapat bekerja lintas aplikasi tanpa selalu seamless. Test suite proyek itu bersifat panduan interoperabilitas, bukan normative specification.

## Accessibility dan forms

Autonomous custom elements tidak otomatis memperoleh semantics elemen native yang tampilannya ditiru. Author tetap bertanggung jawab atas accessible name, role, state, keyboard behavior, focus, contrast, dan error communication.

HTML Standard menyediakan hooks untuk default accessibility semantics serta form-associated custom elements. Hooks tersebut membantu integrasi, tetapi tidak menggantikan pengujian dengan browser, keyboard, dan assistive technology.

## Kapan digunakan

Web Components cocok untuk design system lintas framework, embedded widget, gradual migration, dan interface yang perlu bertahan melampaui lifecycle satu framework.

Framework component biasa dapat lebih sederhana ketika seluruh aplikasi memakai satu runtime dan tidak membutuhkan distribusi lintas stack. Web Components juga tidak menggantikan router, application state, build system, atau rendering framework.

Web Components berhubungan dengan [[References/HTML\|HTML]], [[References/CSS\|CSS]], [[References/JavaScript\|JavaScript]], [[References/Web APIs\|Web APIs]], [[DOM turns documents into scriptable objects\|DOM turns documents into scriptable objects]], [[References/Design Systems\|Design Systems]], [[References/React\|React]], dan [[References/Astro\|Astro]].

## Lanjutan

- [[Custom elements make HTML the portability boundary\|Custom elements make HTML the portability boundary]]
- [[Shadow DOM reduces collisions without eliminating integration work\|Shadow DOM reduces collisions without eliminating integration work]]
- [[Templates stay inert while slots preserve consumer ownership\|Templates stay inert while slots preserve consumer ownership]]
- [[Public properties and events determine framework interoperability\|Public properties and events determine framework interoperability]]
