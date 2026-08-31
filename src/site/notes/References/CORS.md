---
{"dg-publish":true,"dg-path":"CORS.md","permalink":"/cors/","title":"CORS","hideInFiletree":true,"tags":["references","security","programming","javascript","http"],"dg-note-properties":{"title":"CORS","category":"references","tags":["references","security","programming","javascript","http"],"sources":["_raw/articles/cors-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

![2026-08-29-how-cors-works.png](/img/user/Attachments/2026-08-29-how-cors-works.png)
CORS (*Cross-Origin Resource Sharing*) adalah mekanisme berbasis header HTTP yang memungkinkan server menentukan origin lain yang boleh membaca respons melalui browser. Origin ditentukan oleh kombinasi skema, host, dan port, sehingga perubahan pada salah satu unsur tersebut membuat dua URL berbeda origin. Mekanisme ini memungkinkan aplikasi web mengakses API, font, tekstur WebGL, atau sumber daya lain yang berada di domain berbeda tanpa meniadakan perlindungan bawaan browser.

## Hubungan dengan same-origin policy

Browser menerapkan *same-origin policy* untuk membatasi interaksi dokumen atau skrip dengan sumber daya dari origin lain. Kebijakan ini terutama membatasi pembacaan lintas origin dan membantu mencegah situs berbahaya membaca data dari layanan tempat pengguna sedang masuk atau dari jaringan internal yang dapat dijangkau browser.

CORS bukan mekanisme autentikasi dan bukan pengganti otorisasi pada server. CORS menentukan apakah JavaScript pada origin tertentu boleh membaca respons, sedangkan server tetap harus memeriksa identitas pengguna dan hak akses untuk setiap permintaan sensitif. Klien nonbrowser seperti `curl` dapat mengirim nilai `Origin` sendiri dan tidak terikat pada pembatasan pembacaan yang diterapkan browser.

Karena itu, pernyataan bahwa CORS "mencegah akses tidak sah" perlu dibatasi. CORS mengurangi pembacaan lintas origin yang tidak diizinkan di browser, tetapi tidak mencegah permintaan langsung ke API dan tidak menggantikan perlindungan terhadap CSRF.

## Alur permintaan CORS

Pada permintaan lintas origin, browser menambahkan header `Origin` yang berisi origin pemanggil tanpa bagian path. Server memutuskan apakah origin tersebut diizinkan dan, jika diizinkan, mengirim `Access-Control-Allow-Origin` pada respons. Browser kemudian membandingkan kebijakan pada respons dengan konteks permintaan sebelum mengekspos status, header, dan isi respons kepada JavaScript.

Permintaan yang memenuhi batas metode, header, dan tipe media tertentu dapat dikirim tanpa *preflight*. Metode yang termasuk daftar aman CORS adalah `GET`, `HEAD`, dan `POST`, tetapi penggunaan header atau `Content-Type` di luar daftar aman dapat memicu pemeriksaan tambahan. Tidak adanya *preflight* bukan berarti permintaan bebas dari CORS. Server tetap harus mengirim header izin yang tepat agar browser menyerahkan respons kepada kode pemanggil.

## Preflight dengan OPTIONS

Untuk permintaan yang tidak memenuhi kriteria daftar aman, browser mengirim permintaan `OPTIONS` sebelum permintaan utama. Permintaan *preflight* menyertakan `Origin`, `Access-Control-Request-Method`, dan bila diperlukan `Access-Control-Request-Headers` untuk menjelaskan metode serta header yang akan dipakai.

Server merespons dengan kombinasi `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, dan `Access-Control-Allow-Headers`. Browser hanya mengirim permintaan utama jika respons tersebut mengizinkan origin, metode, dan header yang diminta. Hasil *preflight* dapat disimpan sementara melalui `Access-Control-Max-Age`, meskipun browser dapat memberlakukan batas internal yang lebih rendah daripada nilai dari server.

Contoh pertukaran *preflight*:

```http
OPTIONS /orders HTTP/1.1
Origin: https://app.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type, authorization

HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 600
Vary: Origin
```

Header CORS harus tersedia pada respons *preflight* dan respons utama sesuai kebutuhan. Kesalahan implementasi dapat muncul ketika `OPTIONS` ditangani dengan benar tetapi respons aktual tidak memuat `Access-Control-Allow-Origin`, atau ketika middleware autentikasi menolak `OPTIONS` sebelum kebijakan CORS dijalankan.

## Header penting

`Access-Control-Allow-Origin` menerima satu origin eksplisit atau `*` untuk sumber daya publik tanpa kredensial. Jika server memilih origin secara dinamis dari daftar izin, server perlu membandingkan `Origin` dengan daftar tersebut dan mengembalikan origin yang cocok, bukan memantulkan setiap nilai tanpa validasi.

`Access-Control-Allow-Methods` menyatakan metode yang diperbolehkan setelah *preflight*, sedangkan `Access-Control-Allow-Headers` menyatakan header permintaan yang diizinkan. `Access-Control-Expose-Headers` menambah header respons yang boleh dibaca JavaScript di luar header respons yang sudah masuk daftar aman. `Access-Control-Max-Age` mengatur berapa lama hasil *preflight* dapat disimpan oleh browser.

Ketika respons untuk origin eksplisit dapat berbeda berdasarkan header `Origin`, server sebaiknya mengirim `Vary: Origin`. Header ini memberi tahu cache bahwa representasi respons bergantung pada origin pemanggil dan mencegah respons CORS untuk satu origin digunakan secara keliru bagi origin lain.

## Kredensial dan cookie

Kredensial dalam konteks Fetch mencakup cookie HTTP, sertifikat klien TLS, dan informasi autentikasi HTTP. Pada `fetch()`, permintaan lintas origin perlu memakai `credentials: "include"` jika aplikasi memang harus menyertakan kredensial. Server juga harus mengirim `Access-Control-Allow-Credentials: true` dan menyebut origin secara eksplisit melalui `Access-Control-Allow-Origin`.

Wildcard `*` tidak dapat dipakai untuk `Access-Control-Allow-Origin` pada respons berkredensial. Permintaan *preflight* sendiri tidak menyertakan kredensial, tetapi respons *preflight* harus menyatakan apakah permintaan utama boleh menggunakan kredensial.

Contoh permintaan berkredensial:

```js
const response = await fetch("https://api.example/account", {
  credentials: "include",
});
```

```http
Access-Control-Allow-Origin: https://app.example
Access-Control-Allow-Credentials: true
Vary: Origin
```

Mengizinkan kredensial memperbesar dampak kesalahan konfigurasi. MDN menyarankan agar akses berkredensial dibatasi pada origin spesifik yang benar-benar diperlukan dan agar server tidak sekadar memantulkan nilai `Origin` dari permintaan.

## Konfigurasi aman

Kebijakan CORS sebaiknya diterapkan hanya pada endpoint yang memang perlu diakses lintas origin. MDN menyarankan penggunaan jumlah origin dan sumber daya sekecil mungkin, misalnya mengaktifkan CORS pada endpoint API publik tanpa menerapkannya pada seluruh situs.

Daftar izin harus memuat origin lengkap, termasuk skema dan port bila tidak menggunakan port bawaan. Pencocokan berbasis potongan string atau ekspresi reguler yang terlalu luas dapat menerima domain penyerang yang tampak mirip dengan domain tepercaya. Nilai `null` pada `Access-Control-Allow-Origin` juga sebaiknya dihindari karena dokumen tersandbox dan sejumlah skema menghasilkan origin `null` yang dapat disalahgunakan.

OWASP menyoroti dua pola berbahaya: wildcard pada sumber daya yang tidak semestinya bersifat publik dan pemantulan header `Origin` tanpa pemeriksaan daftar izin. Pengujian perlu mencakup origin asing, subdomain yang tidak dipercaya, origin `null`, permintaan berkredensial, metode nonstandar, serta header khusus untuk memastikan server tidak memberikan izin lebih luas daripada yang dimaksud.

## CORS bukan solusi untuk semua akses lintas domain

CORS berlaku pada pembacaan sumber daya lintas origin oleh browser melalui API seperti `fetch()` dan `XMLHttpRequest`. Komunikasi antardokumen melalui `iframe` memakai aturan dan API lain, misalnya `window.postMessage`, sedangkan pembatasan penyematan dapat melibatkan `Content-Security-Policy` atau `X-Frame-Options`.

Mengatur `mode: "no-cors"` juga bukan cara untuk melewati kebijakan server. Mode tersebut membatasi metode dan header yang dapat digunakan serta menghasilkan respons *opaque* yang isi dan headernya tidak dapat dibaca JavaScript. Solusi yang tepat adalah memperbaiki konfigurasi pada server pemilik sumber daya, menggunakan layanan perantara yang dipercaya, atau menjaga permintaan tetap dalam origin yang sama.

## Ringkasan

CORS adalah protokol persetujuan antara browser dan server untuk pembacaan respons lintas origin. Server menyatakan origin, metode, header, dan penggunaan kredensial yang diperbolehkan, sedangkan browser menegakkan keputusan tersebut. Penerapan yang aman memerlukan daftar izin yang ketat, penanganan *preflight* yang benar, `Vary: Origin` untuk respons dinamis, serta autentikasi, otorisasi, dan perlindungan CSRF yang tetap berdiri sendiri.

## Lihat juga

- [[References/HTTP\|HTTP]]
- [[References/JavaScript\|JavaScript]]
- [[References/Web Browser\|Web Browser]]
- [[References/Jaringan (Network)\|Jaringan (Network)]]

## Sumber

- [Cross-Origin Resource Sharing: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS): konsep CORS, permintaan sederhana, *preflight*, kredensial, dan header terkait.
- [Same-origin policy: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Same-origin_policy): definisi origin, pembatasan akses lintas origin, dan hubungan dengan CORS.
- [Access-Control-Allow-Origin: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Origin): sintaks, wildcard, origin eksplisit, origin `null`, dan `Vary: Origin`.
- [CORS configuration: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CORS): panduan pembatasan origin dan endpoint dengan prinsip izin minimum.
- [Fetch Standard: WHATWG](https://fetch.spec.whatwg.org): definisi normatif Fetch, metode yang masuk daftar aman, kredensial, dan protokol CORS.
- [Testing Cross Origin Resource Sharing: OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing): risiko salah konfigurasi, validasi origin, dan pengujian keamanan CORS.
