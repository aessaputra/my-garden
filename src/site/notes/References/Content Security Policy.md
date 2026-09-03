---
{"dg-publish":true,"dg-path":"Content Security Policy.md","permalink":"/content-security-policy/","title":"Content Security Policy","hideInFiletree":true,"tags":["references","security","programming","javascript","http"],"noteIcon":"","dg-note-properties":{"title":"Content Security Policy","category":"references","tags":["references","security","programming","javascript","http"],"sources":["_raw/articles/content-security-policy-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Content Security Policy (CSP) adalah mekanisme keamanan browser yang memungkinkan pengelola situs membatasi sumber daya dan perilaku yang diizinkan pada sebuah dokumen. Kebijakan ini umumnya dikirim melalui header respons HTTP `Content-Security-Policy`, lalu browser menerapkan setiap direktif sebelum memuat atau menjalankan sumber daya yang terkait. CSP terutama dipakai untuk mengurangi dampak injeksi konten seperti *cross-site scripting* (XSS), tetapi juga dapat membatasi penyematan halaman, pengiriman formulir, koneksi jaringan, dan pemuatan sumber daya yang tidak aman.

CSP tidak memperbaiki kerentanan pada kode aplikasi. Spesifikasi W3C menempatkannya sebagai pertahanan berlapis, bukan pengganti validasi masukan, pengodean keluaran, dan sanitasi yang benar. Kebijakan yang lemah atau terlalu permisif tetap dapat membiarkan kode berbahaya berjalan, terutama jika menggunakan `'unsafe-inline'`, `'unsafe-eval'`, wildcard yang luas, atau daftar origin yang dapat menyajikan konten buatan pengguna.

## Cara browser menerapkan kebijakan

Sebuah kebijakan terdiri atas direktif yang dipisahkan dengan titik koma. Setiap direktif mengatur perilaku tertentu, sedangkan nilainya menentukan sumber atau tindakan yang diperbolehkan.

```http
Content-Security-Policy: default-src 'self'; script-src 'self'; img-src 'self' https://images.example; object-src 'none'
```

Pada contoh tersebut, `default-src 'self'` menjadi aturan cadangan bagi jenis sumber daya yang tidak memiliki direktif khusus. `script-src 'self'` membatasi JavaScript pada origin dokumen, `img-src` menambahkan satu origin gambar, dan `object-src 'none'` menolak seluruh sumber daya berbasis objek. Direktif yang lebih khusus menggantikan fallback untuk jenisnya, bukan menambah nilai ke `default-src`.

Fallback tidak berlaku untuk semua direktif. Sebagai contoh, `frame-ancestors` tidak mengambil nilai dari `default-src`, sehingga aturan penyematan harus ditulis secara eksplisit. Beberapa kelompok fallback juga berlapis: `script-src` menjadi fallback bagi `script-src-elem` dan `script-src-attr`, sedangkan `style-src` menjadi fallback bagi varian elemen dan atribut gaya.

Kebijakan sebaiknya dikirim sebagai header respons karena metode ini mendukung seluruh fitur CSP. Elemen `<meta http-equiv="Content-Security-Policy">` dapat dipakai ketika header server tidak dapat diatur, tetapi tidak mendukung fitur seperti `frame-ancestors`, `sandbox`, dan pelaporan. Kebijakan `Content-Security-Policy-Report-Only` juga tidak dapat disampaikan melalui elemen `meta`.

## Direktif yang umum digunakan

Direktif pengambilan sumber daya menentukan lokasi yang boleh digunakan browser untuk memuat konten.

- `script-src` mengatur JavaScript, termasuk berkas eksternal, skrip sebaris, penangan kejadian sebaris, dan bentuk eksekusi terkait.
- `style-src` mengatur lembar gaya dan CSS sebaris.
- `img-src`, `font-src`, dan `media-src` membatasi gambar, font, audio, serta video.
- `connect-src` membatasi tujuan koneksi seperti `fetch`, XHR, `EventSource`, dan WebSocket.
- `frame-src` mengatur halaman yang boleh dimuat oleh dokumen dalam bingkai, sedangkan `frame-ancestors` mengatur pihak yang boleh menyematkan dokumen tersebut.
- `form-action` membatasi tujuan pengiriman formulir dan dapat mengurangi risiko formulir injeksi yang mengirim data ke origin penyerang.
- `base-uri` membatasi URL yang dapat dipakai elemen `<base>`, sehingga penyerang tidak mudah mengubah resolusi URL relatif.
- `object-src 'none'` menonaktifkan pemuatan objek dan plugin lama yang tidak diperlukan.
- `upgrade-insecure-requests` meminta browser memperlakukan URL HTTP yang tidak aman sebagai URL HTTPS.

Nilai `'self'` merujuk pada origin dokumen, termasuk skema dan port yang relevan, bukan seluruh domain organisasi. Nilai `'none'` harus berdiri sendiri dalam sebuah direktif; bila digabungkan dengan ekspresi sumber lain, nilai lain membuatnya tidak efektif sebagai larangan total. Sementara itu, sumber berbasis skema seperti `https:` dapat terlalu luas karena mengizinkan semua origin yang tersedia melalui skema tersebut.

## Mitigasi XSS dengan CSP ketat

CSP dapat memblokir skrip dari origin yang tidak diizinkan, skrip sebaris tanpa otorisasi, penangan kejadian sebaris, URL `javascript:`, dan fungsi evaluasi kode seperti `eval()`. Namun, daftar origin saja sulit dipelihara dan dapat kehilangan efektivitas bila salah satu host yang diizinkan menyajikan konten yang dapat dikendalikan penyerang. MDN dan OWASP karena itu merekomendasikan CSP ketat berbasis *nonce* atau hash untuk mengendalikan eksekusi skrip.

### Nonce

*Nonce* adalah nilai acak yang dibuat ulang untuk setiap respons HTTP. Server menempatkan nilai yang sama dalam `script-src` dan atribut `nonce` pada setiap elemen skrip yang memang dipercaya.

```http
Content-Security-Policy: script-src 'nonce-r4nd0m'; object-src 'none'; base-uri 'none'
```

```html
<script nonce="r4nd0m" src="/app.js"></script>
```

Browser hanya menjalankan skrip bila kedua nilai cocok. Nilai tersebut harus tidak dapat diprediksi dan unik untuk setiap respons agar penyerang tidak dapat menggunakannya pada skrip injeksi. Implementasi server harus memberi *nonce* melalui mesin templat kepada skrip yang telah dipercaya, bukan menambahkannya secara otomatis ke setiap elemen `<script>`, karena cara kedua dapat memberi *nonce* kepada elemen yang disisipkan penyerang.

Kebijakan berbasis *nonce* cocok untuk HTML yang dibangkitkan secara dinamis. Konsekuensinya, respons HTML tidak dapat digunakan sebagai berkas statis yang sama untuk semua permintaan tanpa mekanisme lain yang menyisipkan nilai baru.

### Hash

Kebijakan berbasis hash mengizinkan skrip atau gaya hanya bila digest kontennya cocok dengan nilai yang tercantum pada kebijakan. CSP mendukung SHA-256, SHA-384, dan SHA-512 dengan hasil yang dikodekan menggunakan Base64.

```http
Content-Security-Policy: script-src 'sha256-BASE64_DIGEST'; object-src 'none'; base-uri 'none'
```

Browser menghitung hash konten lalu membandingkannya dengan kebijakan sebelum menjalankan skrip. Untuk skrip eksternal, elemen juga harus memiliki atribut `integrity` yang sesuai. Pendekatan ini cocok untuk halaman statis, tetapi setiap perubahan konten, termasuk spasi pada skrip sebaris, mengubah hash dan menuntut pembaruan kebijakan.

### `strict-dynamic`

Ekspresi `'strict-dynamic'` meneruskan kepercayaan dari skrip akar yang memiliki *nonce* atau hash kepada skrip yang dimuat secara dinamis oleh skrip tersebut. Pada browser yang mendukungnya, keberadaan ekspresi ini menyebabkan sumber berbasis host, skema, `'self'`, dan `'unsafe-inline'` dalam direktif yang sama diabaikan.

Fitur ini memudahkan aplikasi yang memakai pemuat skrip, tetapi kepercayaannya ikut meluas ke seluruh rantai pemuatan dinamis. Jika skrip tepercaya membuat elemen skrip dari masukan yang tidak aman, CSP tidak dapat menggantikan validasi pada jalur tersebut.

## Nilai yang melemahkan kebijakan

`'unsafe-inline'` mengizinkan bentuk kode sebaris yang biasanya diblokir CSP, termasuk elemen skrip sebaris, penangan kejadian, dan URL `javascript:`. Nilai ini menghapus sebagian besar manfaat CSP terhadap XSS dan sebaiknya diganti dengan *nonce*, hash, atau pemindahan kode ke berkas eksternal yang sesuai.

`'unsafe-eval'` mengizinkan fungsi yang mengubah string menjadi kode, termasuk `eval()`, konstruktor `Function`, serta bentuk tertentu dari `setTimeout()` dan `setInterval()`. Mengaktifkannya memperluas jalur eksekusi yang dapat dimanfaatkan injeksi, sehingga dependensi yang memerlukannya perlu ditinjau atau diganti.

Wildcard dan daftar host yang luas juga bukan bukti bahwa setiap konten pada host tersebut aman. Kebijakan harus memberikan izin sekecil mungkin dan menilai kemampuan setiap origin untuk menyajikan skrip, pengalihan, JSONP, atau konten yang dapat diunggah pengguna.

## Perlindungan terhadap clickjacking

Clickjacking terjadi ketika situs penyerang menyematkan halaman target dalam elemen seperti `iframe`, menyamarkannya, lalu mengarahkan pengguna agar tanpa sadar berinteraksi dengan halaman target. Direktif `frame-ancestors` mengatur origin yang boleh menyematkan respons tersebut dan merupakan kontrol CSP utama untuk membatasi serangan ini.

```http
Content-Security-Policy: frame-ancestors 'none'
```

Nilai `'none'` melarang seluruh penyematan, sedangkan `'self'` hanya mengizinkan dokumen dari origin yang sama. Bila halaman memang perlu disematkan oleh mitra, daftar origin harus ditulis secara eksplisit. Direktif ini harus dikirim melalui header respons dan tidak bekerja melalui elemen `meta`.

`X-Frame-Options` masih dapat dipakai untuk kompatibilitas dengan browser lama, tetapi browser modern yang mendukung `frame-ancestors` akan mengutamakan direktif CSP tersebut. Atribut cookie `SameSite=Lax` atau `SameSite=Strict` dapat menjadi lapisan tambahan untuk skenario yang memerlukan sesi, tetapi tidak menggantikan pembatasan penyematan.

## Trusted Types dan DOM XSS

Direktif `require-trusted-types-for 'script'` meminta browser menolak string biasa pada titik injeksi DOM tertentu dan hanya menerima objek Trusted Types. Aplikasi membuat objek tersebut melalui kebijakan transformasi yang ditentukan sendiri, sedangkan direktif `trusted-types` dapat membatasi nama kebijakan yang boleh dibuat.

Trusted Types membantu memusatkan pemeriksaan sebelum data mencapai API seperti `innerHTML`, tetapi keamanan tetap bergantung pada transformasi yang diterapkan aplikasi. Kebijakan yang hanya membungkus string tanpa sanitasi tidak menjadikan masukan tersebut aman. Dukungan browser juga perlu diperiksa sebelum kontrol ini dijadikan satu-satunya mitigasi DOM XSS.

## Pelaporan pelanggaran

CSP dapat mengirim laporan ketika browser memblokir atau mendeteksi perilaku yang melanggar kebijakan. Direktif `report-to` menunjuk nama endpoint yang dipetakan melalui header `Reporting-Endpoints`; laporan dikirim sebagai JSON melalui HTTP POST dengan tipe konten `application/reports+json`.

```http
Reporting-Endpoints: csp-endpoint="https://example.com/csp-reports"
Content-Security-Policy: default-src 'self'; report-to csp-endpoint
```

`report-to` menggantikan `report-uri`, tetapi dokumentasi MDN dan OWASP masih menyarankan keduanya selama dukungan lintas browser belum seragam. Laporan mencatat pelanggaran kebijakan, bukan keberhasilan serangan, dan dapat dipakai untuk menemukan sumber daya sah yang belum tercakup oleh konfigurasi.

## Penerapan bertahap dengan mode report-only

Header `Content-Security-Policy-Report-Only` mengevaluasi kebijakan dan menghasilkan laporan tanpa memblokir perilaku yang melanggar aturan. Mode ini memungkinkan tim menemukan dependensi, kode sebaris, dan koneksi yang akan rusak sebelum kebijakan diterapkan.

Sebuah respons dapat membawa kebijakan aktif dan kebijakan *report-only* secara bersamaan. Browser menegakkan `Content-Security-Policy`, sementara kebijakan kedua hanya menghasilkan laporan. Pendekatan ini berguna untuk menguji versi yang lebih ketat tanpa melonggarkan kebijakan yang sudah berlaku.

Penerapan bertahap dapat dimulai dengan inventaris sumber daya dan perilaku halaman, dilanjutkan dengan kebijakan *report-only*, perbaikan pelanggaran yang sah, lalu pengaktifan kebijakan secara bertahap. Laporan perlu ditinjau pada setiap tahap karena kebijakan yang terlalu sempit dapat memblokir fungsi aplikasi yang sah.

## Contoh kebijakan awal

Kebijakan berikut menunjukkan pola ketat berbasis *nonce* untuk aplikasi dinamis:

```http
Content-Security-Policy:
  default-src 'none';
  script-src 'nonce-{RANDOM}' 'strict-dynamic';
  style-src 'self';
  img-src 'self' data:;
  font-src 'self';
  connect-src 'self' https://api.example;
  object-src 'none';
  base-uri 'none';
  form-action 'self';
  frame-ancestors 'none';
  upgrade-insecure-requests;
  report-to csp-endpoint
```

Contoh tersebut bukan templat universal. `data:` pada `img-src`, origin API, kebutuhan gaya, penyematan, pekerja web, dan layanan pihak ketiga harus disesuaikan dengan perilaku aplikasi yang nyata. Kebijakan yang lebih luas dari kebutuhan menambah jalur serangan, sedangkan kebijakan yang terlalu sempit merusak fungsi sah.

## Pemeriksaan operasional

CSP perlu diuji dan ditinjau setiap kali dependensi atau arsitektur aplikasi berubah. Pemeriksaan otomatis dapat menguji keberadaan header, mendeteksi `'unsafe-inline'` dan `'unsafe-eval'`, serta memastikan *nonce* berbeda pada respons yang terpisah. Pengujian browser kemudian memastikan alur pengguna utama tetap berfungsi di bawah kebijakan aktif.

Kebijakan juga harus konsisten pada seluruh respons HTML yang relevan, bukan hanya halaman utama. Respons pekerja memerlukan header CSP sendiri karena pekerja umumnya tidak mewarisi kebijakan dari dokumen pembuatnya. Jika beberapa kebijakan aktif diterapkan pada respons yang sama, hasilnya semakin membatasi karena sebuah pemuatan harus lolos dari seluruh kebijakan.

## Ringkasan

CSP membatasi sumber dan perilaku yang diizinkan browser, sehingga injeksi kode dan penyematan lintas origin menjadi lebih sulit dieksploitasi. Perlindungan XSS yang kuat menggunakan kebijakan ketat berbasis *nonce* atau hash, menghindari `'unsafe-inline'` dan `'unsafe-eval'`, serta tetap mempertahankan sanitasi dan pengodean keluaran. `frame-ancestors` menangani risiko clickjacking, sementara Trusted Types mempersempit penggunaan titik injeksi DOM tertentu.

Pelaporan dan mode *report-only* membantu penerapan dilakukan tanpa menebak seluruh kebutuhan aplikasi sejak awal. Nilai CSP bergantung pada ketepatan kebijakan, proses pengujian, dan pemeliharaan setelah aplikasi berubah.

## Lihat juga

- [[References/HTTP\|HTTP]]
- [[References/HTTPS\|HTTPS]]
- [[References/CORS\|CORS]]
- [[References/JavaScript\|JavaScript]]
- [[References/Web Browser\|Web Browser]]

## Sumber

- [Content Security Policy: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP): konsep, pola CSP ketat, Trusted Types, penerapan, dan pelaporan.
- [Content-Security-Policy header: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy): sintaks, direktif, fallback, ekspresi sumber, pekerja, dan beberapa kebijakan.
- [Direktif script-src: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/script-src): sumber JavaScript, nonce, hash, kode sebaris, evaluasi, dan `strict-dynamic`.
- [Content-Security-Policy-Report-Only: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy-Report-Only): pengujian kebijakan tanpa pemblokiran dan konfigurasi endpoint.
- [Content Security Policy Level 3: W3C](https://www.w3.org/TR/CSP3): spesifikasi CSP, model pemrosesan, tujuan keamanan, dan mekanisme pelaporan.
- [Content Security Policy Cheat Sheet: OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html): penerapan CSP ketat, kebijakan berbasis nonce atau hash, dan praktik konfigurasi.
- [Clickjacking: MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/Clickjacking): mekanisme clickjacking serta mitigasi melalui `frame-ancestors` dan kontrol tambahan.
- [Direktif report-to: MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/report-to): pemetaan endpoint dan format laporan pelanggaran CSP.
