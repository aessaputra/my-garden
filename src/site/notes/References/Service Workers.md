---
{"dg-publish":true,"dg-path":"Service Workers.md","permalink":"/service-workers/","title":"Service Workers","hideInFiletree":true,"tags":["references","programming","javascript","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Service Workers","category":"references","tags":["references","programming","javascript","performance","security"],"sources":["_raw/articles/service-workers-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Service worker adalah worker [[References/JavaScript\|JavaScript]] berbasis event yang didaftarkan untuk suatu origin dan scope. Browser dapat menjalankannya di antara aplikasi web dan jaringan, lalu mengirim event seperti `install`, `activate`, `fetch`, dan `push`. Dengan mekanisme ini, service worker dapat memilih respons dari jaringan, Cache Storage, atau respons buatan aplikasi. Kemampuan tersebut memungkinkan offline fallback, strategi caching, background synchronization, dan penerimaan push message ketika halaman tidak sedang terbuka.

Service worker sering dijelaskan sebagai proxy, tetapi proxy ini berjalan di browser pengguna dan hanya mengendalikan request dalam scope registrasinya. File service worker tidak melakukan caching secara otomatis. Developer tetap harus menulis handler, menentukan resource yang disimpan, mengatur versi cache, serta menangani pembaruan dan kegagalan jaringan.

## Model eksekusi

Service worker adalah jenis web worker. Kodenya berjalan di worker context, terpisah dari main thread, dan tidak memiliki akses langsung ke DOM. Komunikasi dengan halaman dilakukan melalui pesan atau API lain yang tersedia. Karena browser dapat menghentikan worker saat tidak ada event yang perlu diproses, state penting tidak boleh hanya disimpan dalam variabel global. Gunakan penyimpanan persisten seperti Cache Storage atau IndexedDB jika data harus bertahan setelah worker dihentikan.

Browser dapat memulai service worker untuk menangani event meskipun tidak ada dokumen yang aktif. Namun, service worker bukan proses latar belakang yang selalu hidup. Masa hidupnya terkait dengan event. `event.waitUntil()` memberi tahu browser bahwa pekerjaan asynchronous masih berlangsung, sedangkan `event.respondWith()` menentukan respons untuk sebuah `fetch` event.

Service worker hanya tersedia dalam secure context, umumnya [[References/HTTPS\|HTTPS]]. Browser memperlakukan `localhost` sebagai secure context untuk pengembangan lokal. Pembatasan ini diperlukan karena worker dapat memengaruhi navigasi dan resource yang diterima pengguna; script yang disusupi pada koneksi tidak aman dapat mempertahankan kontrol terhadap request berikutnya.

## Registrasi dan scope

Halaman mendaftarkan service worker melalui `navigator.serviceWorker.register()`. `scriptURL` menentukan file worker, sedangkan opsi `scope` menentukan rentang URL yang dapat dikendalikan. Secara default, scope mengikuti direktori tempat script service worker berada. Script yang diletakkan terlalu dalam tidak dapat mengendalikan halaman di atas direktorinya kecuali server memberi izin melalui header `Service-Worker-Allowed`.

Registrasi pertama tidak membuat halaman yang sedang terbuka langsung dikendalikan. Browser harus mengunduh, memeriksa, memasang, dan mengaktifkan worker terlebih dahulu. Karena itu, fitur inti aplikasi sebaiknya tetap berfungsi ketika Service Worker API tidak tersedia atau worker belum mengambil alih. Service worker cocok sebagai progressive enhancement, bukan satu-satunya jalur untuk memuat antarmuka awal.

## Lifecycle dan pembaruan

Lifecycle utama terdiri dari registrasi, instalasi, waiting, aktivasi, dan pengendalian client:

1. Halaman memanggil `register()`.
2. Browser mengunduh dan mengevaluasi script.
3. Event `install` berjalan. Tahap ini sering dipakai untuk precaching resource minimum.
4. Worker baru masuk status waiting jika versi lama masih mengendalikan tab.
5. Setelah worker lama tidak lagi mengendalikan client, worker baru diaktifkan.
6. Event `activate` dapat membersihkan cache versi lama.
7. Worker aktif menangani event untuk client dalam scope.

Browser memeriksa perubahan script service worker. Jika script baru tidak identik dengan versi aktif, browser memasangnya sebagai worker baru. Mempertahankan URL script yang stabil, misalnya `/sw.js`, mencegah worker lama terus menyajikan halaman yang hanya mendaftarkan nama file versi lama.

`skipWaiting()` dapat memaksa worker yang menunggu untuk segera masuk tahap aktivasi. `clients.claim()` dapat membuat worker aktif segera mengendalikan client dalam scope. Penggunaan keduanya perlu dirancang hati-hati. Halaman lama dapat mulai dikendalikan worker baru sementara JavaScript dan aset yang sedang berjalan masih berasal dari versi sebelumnya. Ketidakcocokan versi ini dapat menghasilkan UI rusak atau response dengan format yang tidak sesuai.

## Intersepsi request

`fetch` event muncul untuk navigasi dan pemuatan subresource dalam scope, termasuk script, stylesheet, gambar, dan request fetch/XHR. Handler dapat memanggil `respondWith()` dengan:

- response dari jaringan;
- response dari Cache Storage;
- response yang dibuat dengan `Response`;
- strategi gabungan dengan fallback ketika jaringan gagal.

Service worker tidak mengubah aturan dasar browser seperti same-origin policy dan [[References/CORS\|CORS]]. Ia juga tidak membuat server tersedia saat offline. Offline support hanya berlaku untuk resource atau data yang telah disimpan, dapat dibuat secara lokal, atau memiliki fallback yang disiapkan.

## Strategi caching

Cache Storage menyimpan pasangan `Request` dan `Response` dalam cache bernama. Isinya tidak kedaluwarsa atau diperbarui sendiri seperti kebijakan cache aplikasi yang otomatis. Aplikasi bertanggung jawab atas pemilihan resource, pembaruan, versi nama cache, dan penghapusan entri lama. Browser tetap dapat menghapus origin storage ketika menghadapi tekanan penyimpanan.

Strategi dipilih berdasarkan jenis data:

- **Cache first** cocok untuk aset berversi yang jarang berubah. Cache diperiksa lebih dahulu, lalu jaringan digunakan ketika tidak ada kecocokan.
- **Network first** mencoba jaringan dan memakai cache ketika request gagal. Strategi ini sesuai untuk konten yang perlu cukup segar tetapi tetap memiliki fallback.
- **Stale while revalidate** segera mengembalikan response cache sambil meminta versi baru untuk penggunaan berikutnya.
- **Network only** sesuai untuk request yang tidak boleh dipenuhi dari cache.
- **Cache only** sesuai untuk resource yang telah dipastikan tersedia melalui precaching.

Precaching biasanya dilakukan pada event `install` untuk app shell dan resource minimum. Menyimpan semua aset pada tahap ini memboroskan bandwidth dan penyimpanan, memperbesar peluang instalasi gagal, serta dapat mempertahankan konten lama. Cache runtime dapat mengisi resource saat benar-benar diminta.

Nama cache sebaiknya membawa identitas aplikasi dan versi, terutama ketika beberapa aplikasi berbagi satu origin. Pada event `activate`, hapus hanya cache yang memang dimiliki aplikasi tersebut. Menghapus semua cache origin dapat merusak aplikasi lain yang berada pada host yang sama.

## Offline bukan sekadar cache

Offline experience perlu menjelaskan keadaan aplikasi. Jika halaman atau data tidak tersedia, tampilkan fallback yang dapat dipahami daripada error jaringan generik. Mutasi data juga membutuhkan rancangan khusus: aplikasi dapat menyimpan aksi ke antrean lokal, menandainya sebagai pending, lalu mengirim ulang ketika jaringan tersedia. Konflik, duplikasi request, autentikasi kedaluwarsa, dan kegagalan permanen tetap harus ditangani oleh logika aplikasi.

Background Synchronization API dapat meminta service worker menjalankan pekerjaan setelah koneksi kembali stabil, tetapi dukungan browsernya tidak merata. Aplikasi tetap memerlukan fallback, misalnya mencoba ulang ketika halaman dibuka atau pengguna menekan tombol sinkronisasi.

## Push message dan notifikasi

Push API memungkinkan server mengirim pesan ke subscription yang terkait dengan service worker. Browser dapat membangunkan worker untuk menangani event `push` walaupun aplikasi tidak sedang berada di foreground. Worker kemudian dapat memperbarui state atau menampilkan notifikasi melalui `showNotification()` jika izin dan kebijakan browser mengizinkannya.

Push tidak berarti pesan dapat tiba tanpa koneksi jaringan. Pesan memerlukan konektivitas antara user agent dan push service; manfaat service worker adalah halaman tidak harus terbuka ketika pesan diterima. Subscription endpoint juga merupakan capability URL yang sensitif. Server harus melindungi endpoint, memverifikasi otorisasi saat membuat atau mengaitkan subscription, dan menangani subscription yang kedaluwarsa.

Notifikasi memerlukan izin pengguna dan tidak boleh dijadikan syarat untuk fungsi utama. Permission prompt yang muncul tanpa konteks cenderung mengganggu dan dapat ditolak. Push juga bukan pengganti mekanisme sinkronisasi data yang dapat diverifikasi, karena delivery dan kebijakan latar belakang bergantung pada browser serta sistem operasi.

## Debugging dan operasi

Panel Application pada [[References/Chrome DevTools\|Chrome DevTools]] dapat memeriksa registration, status lifecycle, scope, controlled clients, Cache Storage, dan service worker log. Mode offline dan bypass for network membantu menguji strategi, tetapi skenario produksi juga perlu diuji dengan reload, beberapa tab, update dari versi lama, cache rusak, storage eviction, autentikasi kedaluwarsa, dan koneksi yang putus saat request berlangsung.

Masalah service worker sering tampak seperti bug yang “tetap ada” setelah deployment karena worker atau cache lama masih aktif. Pemeriksaan harus mencakup versi worker yang mengendalikan halaman, status waiting, nama cache, response source, dan apakah tab lama masih terbuka. Unregister dan Clear storage berguna untuk diagnosis, tetapi bukan solusi pembaruan bagi pengguna produksi.

Library Workbox menyediakan routing, precaching, strategi cache, dan integrasi build di atas API tingkat rendah. Workbox mengurangi boilerplate, tetapi keputusan tentang freshness, fallback, invalidasi, dan kompatibilitas versi tetap menjadi tanggung jawab aplikasi.

## Batas dan risiko

Service worker menambah lapisan state dan lifecycle di luar halaman serta server. Manfaat performance dan offline muncul hanya jika strategi cache sesuai dengan data. Cache yang salah dapat menyajikan aplikasi usang, membocorkan response lintas sesi pada perangkat bersama, atau mempertahankan data setelah logout. Jangan cache response privat tanpa kebijakan yang jelas, dan hapus data lokal yang tidak lagi boleh tersedia ketika identitas pengguna berubah.

Service worker bukan syarat tunggal untuk Progressive Web App, bukan jaminan aplikasi dapat bekerja sepenuhnya offline, dan bukan mekanisme yang otomatis mempercepat semua request. Ukur hasilnya, uji update dari beberapa versi lama, serta sediakan jalur pemulihan ketika instalasi atau aktivasi gagal.

## Sumber

1. Service Workers — W3C Candidate Recommendation Draft: https://www.w3.org/TR/service-workers/
2. Service Worker API — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
3. ServiceWorkerContainer.register() — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register
4. ServiceWorkerGlobalScope fetch event — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/fetch_event
5. Cache interface — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Cache
6. Push API — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Push_API
7. Service workers — web.dev: https://web.dev/learn/pwa/service-workers
8. The service worker lifecycle — web.dev: https://web.dev/articles/service-worker-lifecycle
9. Caching — web.dev: https://web.dev/learn/pwa/caching
10. Workbox — web.dev: https://web.dev/learn/pwa/workbox
11. ServiceWorkerRegistration.update() — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerRegistration/update
12. ServiceWorkerGlobalScope.skipWaiting() — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting
13. Clients.claim() — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim
14. Background Synchronization API — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Background_Synchronization_API
