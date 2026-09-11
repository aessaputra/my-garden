---
{"dg-publish":true,"dg-path":"Cookie-Based Authentication.md","permalink":"/cookie-based-authentication/","title":"Cookie-Based Authentication","hideInFiletree":true,"tags":["references","security","auth"],"noteIcon":"","dg-note-properties":{"title":"Cookie-Based Authentication","categories":["APIs"],"type":"reference","tags":["references","security","auth"],"sources":["_raw/articles/cookie-based-authentication-user-summary-2026-09-11.md","_raw/articles/authentication-strategies-research-packet.md","_raw/articles/cors-expanded.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Cookie-based authentication memakai cookie browser untuk membawa kredensial pada request. Dalam pola session server yang umum, cookie berisi session ID opaque, sedangkan server menyimpan data session dan memakai ID tersebut untuk mencarinya.

Cookie adalah mekanisme penyimpanan dan pengiriman, bukan format kredensial. Cookie juga dapat membawa token; karena itu, cookie-based authentication tidak selalu berarti seluruh state session tersimpan di server.

## Alur session server

1. Server memvalidasi login dan membuat session ID acak yang sulit ditebak.
2. Server menyimpan session lalu mengirim ID melalui header `Set-Cookie`.
3. Browser mengirim cookie pada request berikutnya yang memenuhi aturan cookie dan kredensial request.
4. Server memvalidasi session dan memeriksa izin sebelum mengakses resource.
5. Saat logout, server membatalkan session dan menghapus cookie browser.

[[Session server memberi revocation cepat dengan biaya state\|Session server memberi revocation cepat dengan biaya state]] menjelaskan manfaat pencabutan terpusat serta biaya session store. Menghapus cookie saja tidak membatalkan salinan ID yang telah dicuri.

## Keamanan

Gunakan [[References/HTTPS\|HTTPS]], flag `Secure` dan `HttpOnly`, serta kebijakan `SameSite` yang sesuai. `HttpOnly` membatasi pembacaan cookie oleh JavaScript, tetapi tidak mencegah semua tindakan berbahaya melalui XSS. Perbarui ID setelah login atau perubahan privilege untuk mengurangi session fixation.

Pengiriman cookie otomatis dapat memungkinkan CSRF: browser korban mengirim request berkredensial yang dipicu situs penyerang. Gunakan proteksi CSRF yang sesuai, misalnya token anti-CSRF tervalidasi pada operasi perubahan state, disertai pemeriksaan origin. `SameSite` membantu, tetapi bukan pengganti seluruh proteksi aplikasi.

## Request lintas origin

Untuk `fetch()` lintas origin yang memakai cookie, client membutuhkan `credentials: "include"`. Agar respons dapat dibaca, server harus mengizinkan origin spesifik, bukan `*`, dan mengirim `Access-Control-Allow-Credentials: true` sesuai aturan [[References/CORS\|CORS]]. CORS bukan proteksi CSRF.

Cross-origin tidak selalu cross-site. Aturan `SameSite` dan kebijakan third-party cookie tetap berlaku secara terpisah; konfigurasi CORS yang benar tidak menjamin cookie akan dikirim. Dukungan native browser memudahkan penggunaan, tetapi pengamanan dan lifecycle session tetap perlu dirancang.

## Sumber

- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html): session ID, cookie, expiry, dan invalidation.
- [MDN: Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS): request berkredensial dan pembatasan browser.
- [[References/Authentication Strategies\|Authentication Strategies]] dan [[References/CORS\|CORS]]: sintesis serta evidence yang tersedia di vault.
