---
{"dg-publish":true,"dg-path":"Web APIs.md","permalink":"/web-ap-is/","title":"Web APIs","hideInFiletree":true,"tags":["references","javascript","network","security"],"noteIcon":"","dg-note-properties":{"title":"Web APIs","category":"references","tags":["references","javascript","network","security"],"sources":["_raw/articles/web-apis-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04","confidence":"high"}}
---

Web API adalah interface yang dibuka browser runtime kepada JavaScript untuk halaman, network, navigasi, dan hardware.

Ia bukan bagian dari bahasa itu sendiri. Skrip yang sama mendapat kemampuan berbeda di setiap runtime tempat ia berjalan.

## Pembagian runtime

Browser API dikirim bersama browser dan membuka halaman beserta kemampuan device di sekitarnya.

Third party API datang sebagai library yang diambil dari web, dan versioning menjadi tanggung jawab pemanggil. Lihat [taksonomi MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction).

## DOM

DOM memodelkan dokumen sebagai nodes dan objects agar skrip dapat mengubah structure, style, dan content.

Ia language neutral dan tidak tersedia di runtime seperti Node.js. Lihat [referensi DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).

## Network dengan fetch

Method fetch menerima resource path plus options dan mengembalikan promise untuk Response object.

Ia resolve saat headers tiba walau status HTTP error, sehingga pemanggil wajib memeriksa Response.ok. Lihat [referensi Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

Semantik CORS dan Origin tetap berlaku, dan request bodies berperilaku sebagai single use streams.

## History

Single page app menukar konten tanpa reload, sehingga ekspektasi Back dan Forward rusak.

Method pushState mensintesis entries sementara popstate memberi sinyal perubahan entry untuk rerendering. Lihat [panduan History](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API).

## Akses hardware

Akses kamera dan mikrofon lewat getUserMedia dengan video dan audio constraints.

Ia butuh user permission plus secure context, dan akan reject dengan named errors bila tidak terpenuhi. Lihat [referensi getUserMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia).

Permissions Policy dapat memblokir source sepenuhnya, sehingga graceful fallback perlu dirancang sejak awal.

## Memilih

Gunakan DOM API untuk struktur halaman, fetch untuk data server, History untuk navigasi klien, dan hardware API hanya dengan consent flow.

Periksa runtime support lebih dulu, karena ketersediaan berbeda antar browser dan context.

## Batasan

Seluk beluk CORS dan taksonomi hardware error yang lengkap perlu konfirmasi companion page di luar ingest ini.

Perilaku third party API juga bergeser mengikuti versi vendor di luar dokumentasi browser.

## Terkait

- [[Web APIs live in the runtime, not the language\|Web APIs live in the runtime, not the language]]
- [[DOM turns documents into scriptable objects\|DOM turns documents into scriptable objects]]
- [[fetch() treats network as promises with explicit control\|fetch() treats network as promises with explicit control]]
- [[History API restores back button behavior in SPAs\|History API restores back button behavior in SPAs]]
- [[Hardware APIs demand permission and secure context\|Hardware APIs demand permission and secure context]]
- [[References/JavaScript\|JavaScript]]
- [[References/HTTP\|HTTP]]
- [[References/HTTPS\|HTTPS]]
- [[References/CORS\|CORS]]

## Sumber

- [Introduction to web APIs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction): MDN, runtime split of browser and third party APIs.
- [Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model): MDN, node model and language independence.
- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API): MDN, promise mechanics with Request and Response.
- [Working with the History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API): MDN, pushState repair for single page navigation.
- [MediaDevices getUserMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia): MDN, constraints, permission, and secure context.
