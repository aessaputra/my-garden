---
{"dg-publish":true,"dg-path":"Streamed Responses.md","permalink":"/streamed-responses/","title":"Streamed Responses","hideInFiletree":true,"tags":["references","programming","performance","network","javascript"],"noteIcon":"","dg-note-properties":{"title":"Streamed Responses","type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","performance","network","javascript"],"sources":["https://fetch.spec.whatwg.org/","https://streams.spec.whatwg.org/","https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch","https://developer.mozilla.org/en-US/docs/Web/API/Response/body","https://developer.mozilla.org/en-US/docs/Web/API/TextDecoderStream","https://www.rfc-editor.org/rfc/rfc9110.txt","https://www.rfc-editor.org/rfc/rfc9112.txt","https://www.rfc-editor.org/rfc/rfc9113.txt","https://nginx.org/en/docs/http/ngx_http_proxy_module.html","https://developer.chrome.com/docs/capabilities/web-apis/fetch-streaming-requests","https://developer.chrome.com/blog/sw-readablestreams","https://web.dev/articles/rendering-on-the-web"],"created":"2026-08-29","updated":"2026-08-29"}}
---

Streamed response adalah response yang body-nya diproduksi, dikirim, dan dikonsumsi secara bertahap. Client dapat memproses byte yang sudah tiba tanpa menunggu seluruh body selesai. Pola ini dapat menurunkan waktu menuju hasil pertama dan membatasi penggunaan memori untuk payload besar, tetapi tidak otomatis mengurangi total waktu transfer atau jumlah pekerjaan.

Streaming harus bekerja di seluruh jalur. Aplikasi server perlu menulis data secara bertahap, runtime harus melakukan flush, proxy atau [[References/Cloudflare\|CDN]] tidak boleh menahan body terlalu lama, dan client harus membaca stream alih-alih memakai API yang menunggu body lengkap.

## Streaming pada HTTP

Streaming response bukan metode HTTP baru dan bukan sinonim untuk `Transfer-Encoding: chunked`. Pada HTTP/1.1, chunked transfer coding membingkai content yang panjang akhirnya belum diketahui sebagai rangkaian chunk. HTTP/2 dan HTTP/3 menggunakan framing protokol masing-masing; `Transfer-Encoding: chunked` tidak dipakai dengan cara yang sama. Aplikasi sebaiknya menulis body bertahap dan membiarkan server HTTP memilih framing yang sesuai dengan versi koneksi.

Batas chunk transport tidak mempunyai makna aplikasi. Satu record JSON, satu baris, atau satu karakter UTF-8 dapat terpecah di beberapa chunk, sementara beberapa record dapat tiba dalam satu chunk. Format aplikasi memerlukan framing sendiri, misalnya newline-delimited JSON (NDJSON), length prefix, atau format event stream.

Server mengirim status dan header sebelum body. Setelah header dikirim, server tidak dapat mengganti status awal hanya karena pekerjaan berikutnya gagal. Protokol aplikasi perlu mendefinisikan pesan error di dalam stream, penutupan prematur, dan cara client membedakan akhir normal dari kegagalan.

## Membaca response dengan Fetch

`fetch()` memenuhi promise ketika status dan header response sudah tersedia, yang dapat terjadi sebelum body selesai. `Response.body` mengekspos body sebagai `ReadableStream` byte. Metode `response.text()`, `response.json()`, `response.blob()`, dan `response.arrayBuffer()` tetap mengonsumsi body penuh sebelum menghasilkan nilai akhir, sehingga metode tersebut tidak menyediakan pemrosesan inkremental kepada kode pemanggil.

Untuk text stream, byte perlu didekode secara stateful karena satu karakter dapat terbelah di batas chunk. `TextDecoderStream` mempertahankan state decoder antar-chunk. Parser kemudian harus mempertahankan sisa record yang belum lengkap.

```javascript
const response = await fetch("<URL>");
if (!response.ok || !response.body) {
  throw new Error(`HTTP ${response.status}`);
}

const stream = response.body.pipeThrough(new TextDecoderStream());
let pending = "";

for await (const chunk of stream) {
  pending += chunk;
  const lines = pending.split("\n");
  pending = lines.pop() ?? "";

  for (const line of lines) {
    if (line.trim()) processRecord(JSON.parse(line));
  }
}

if (pending.trim()) processRecord(JSON.parse(pending));
```

Contoh ini mengasumsikan satu nilai JSON lengkap per baris. Ia tidak cocok untuk satu dokumen JSON besar yang baru valid setelah penutup terakhir. Server dan client harus menyepakati media type, delimiter, encoding, serta skema error.

## Streams API dan backpressure

Streams API menyediakan `ReadableStream`, `WritableStream`, dan `TransformStream`. `pipeThrough()` menghubungkan transformasi, sedangkan `pipeTo()` mengirim hasil ke sink. Pipe chain meneruskan backpressure ke arah sumber ketika consumer tidak mampu menerima data secepat producer menghasilkannya. Mekanisme ini membatasi pertumbuhan queue jika seluruh pipeline menghormati sinyal tersebut.

Backpressure pada Web Streams tidak menjamin seluruh jaringan atau framework menghentikan producer secara sempurna. Server, database driver, proxy, dan library dapat memiliki buffer sendiri. Pantau penggunaan memori dan respons terhadap client lambat pada deployment sebenarnya.

Body response hanya dapat dikonsumsi sekali. Setelah reader diperoleh, stream terkunci; setelah data dibaca, body dianggap disturbed. Gunakan `Response.clone()` atau `ReadableStream.tee()` hanya ketika dua consumer memang diperlukan. Cabang yang lebih lambat dapat menambah buffering, jadi penggandaan stream bukan operasi gratis.

Batalkan request melalui `AbortController` ketika pengguna berpindah halaman atau hasil tidak lagi diperlukan. Server juga perlu mendeteksi koneksi yang ditutup agar pekerjaan upstream tidak terus berjalan tanpa consumer.

## Format dan use case

HTML dapat diparse dan dirender secara progresif pada navigation response. Streaming server-side rendering dapat mengirim shell atau markup awal sebelum bagian lambat selesai. Manfaatnya bergantung pada urutan markup, resource yang memblokir parser, pekerjaan JavaScript, dan hydration. Konten yang terlihat lebih awal belum tentu sudah interaktif.

Untuk daftar atau ekspor besar, NDJSON dan format berbasis baris lebih mudah diproses secara inkremental daripada satu array JSON. Server-Sent Events (SSE) cocok untuk aliran event satu arah yang memiliki format, event ID, dan perilaku reconnect. WebSocket lebih cocok ketika komunikasi dua arah dan model message diperlukan. Pemilihan protokol harus mengikuti kebutuhan komunikasi, bukan sekadar keinginan untuk “real-time”.

File media dan arsip dapat diproses dengan pipeline byte atau transform stream. Manfaat memori muncul ketika setiap tahap memproses chunk dan tidak menggabungkan ulang seluruh payload sebelum menghasilkan output.

## Buffering di sepanjang jalur

Pemanggilan `write()` di aplikasi tidak membuktikan chunk langsung mencapai browser. Runtime dapat menahan data sampai buffer penuh. Compression middleware dapat menunggu cukup banyak input. Reverse proxy dan CDN juga dapat menggabungkan chunk.

NGINX mengaktifkan `proxy_buffering` secara default. Saat buffering aktif, NGINX membaca response upstream ke buffer dan dapat menulis sebagian ke temporary file. Saat dinonaktifkan, response diteruskan secara sinkron saat diterima. NGINX juga dapat membaca header `X-Accel-Buffering`, tetapi header tersebut bersifat khusus NGINX, bukan standar HTTP.

Konfigurasi harus dibatasi pada endpoint yang memerlukan streaming. Menonaktifkan buffering secara global dapat mengubah penggunaan memori, disk, koneksi, dan throughput proxy. Uji deployment end-to-end karena perilaku lokal tanpa proxy tidak mewakili production.

[[References/Cache-Control\|Cache-Control]] mengatur reuse oleh cache, bukan buffering transmisi secara umum. `Cache-Control: no-cache` tidak memerintahkan setiap proxy untuk segera melakukan flush. Kebijakan cache untuk streamed response perlu ditentukan terpisah berdasarkan apakah representasi selesai dapat digunakan ulang dengan aman.

## Performance

Streaming terutama memperbaiki latency menuju byte, record, atau tampilan pertama. Total download masih ditentukan ukuran payload dan throughput jaringan. Server juga dapat tetap mengerjakan jumlah komputasi yang sama. Chunk yang sangat kecil meningkatkan overhead pemanggilan, framing, parsing, dan rendering; chunk yang terlalu besar menghilangkan manfaat inkremental.

UI sebaiknya membatasi frekuensi update. Menulis DOM untuk setiap token atau byte dapat menghasilkan terlalu banyak layout, paint, atau render framework. Gabungkan update berdasarkan record, interval pendek, atau animation frame tanpa menahan hasil terlalu lama.

Streaming HTML dapat mempercepat First Contentful Paint, tetapi hasil tidak universal. Jika bagian awal tidak berguna, CSS datang terlambat, JavaScript memblokir parser, atau hydration berat, perceived performance dapat tetap buruk. Ukur waktu header, first byte, first record, completion, memory peak, cancellation, dan metrik rendering pada kondisi jaringan realistis.

## Keamanan dan keandalan

Setiap data streamed tetap harus melewati validasi output. Jangan memasukkan potongan HTML atau Markdown yang belum aman ke DOM. Sanitasi setiap chunk secara terpisah juga tidak cukup bila token berbahaya dapat terbelah antar-chunk; parser dan sanitizer perlu mempertahankan konteks gabungan.

Tetapkan batas durasi, ukuran total, ukuran record, queue, dan laju input. Tangani malformed record, timeout, disconnect, retry, duplicate event, serta stream yang berhenti tanpa terminator. Jika resume didukung, gunakan cursor atau event ID yang stabil dan pastikan operasi client idempotent.

Header autentikasi dan otorisasi harus diselesaikan sebelum stream dimulai. Jangan mengirim status sukses dan data privat sebelum pemeriksaan akses selesai. Setelah sebagian body terkirim, rollback pada client tidak mungkin dilakukan; aplikasi hanya dapat menghentikan stream dan menjelaskan kegagalan melalui format yang telah disepakati.

## Diagnosis

Gunakan panel Network di [[References/Chrome DevTools\|Chrome DevTools]] untuk memeriksa waktu header, durasi request, content type, compression, dan apakah data tiba bertahap. DevTools sendiri dapat menyajikan preview setelah buffering tertentu, jadi verifikasi dengan consumer yang mencatat timestamp setiap record.

Uji empat lapisan secara terpisah: generator aplikasi, server atau runtime, reverse proxy/CDN, dan browser. Bandingkan endpoint langsung dengan endpoint production. Simulasikan consumer lambat, cancellation, error setelah header, serta koneksi yang terputus di tengah record. Streaming dinyatakan bekerja bila data dapat diproses secara bertahap pada client akhir, bukan hanya ketika log server menunjukkan beberapa `write()`.

## Sumber

1. Fetch Standard — WHATWG: https://fetch.spec.whatwg.org/
2. Streams Standard — WHATWG: https://streams.spec.whatwg.org/
3. Using the Fetch API — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
4. Response: body property — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Response/body
5. TextDecoderStream — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/TextDecoderStream
6. RFC 9110: HTTP Semantics — RFC Editor: https://www.rfc-editor.org/rfc/rfc9110.txt
7. RFC 9112: HTTP/1.1 — RFC Editor: https://www.rfc-editor.org/rfc/rfc9112.txt
8. RFC 9113: HTTP/2 — RFC Editor: https://www.rfc-editor.org/rfc/rfc9113.txt
9. Module ngx_http_proxy_module — NGINX: https://nginx.org/en/docs/http/ngx_http_proxy_module.html
10. Streaming requests with the Fetch API — Chrome for Developers: https://developer.chrome.com/docs/capabilities/web-apis/fetch-streaming-requests
11. Stream Your Way to Immediate Responses — Chrome for Developers: https://developer.chrome.com/blog/sw-readablestreams
12. Rendering on the Web — web.dev: https://web.dev/articles/rendering-on-the-web
