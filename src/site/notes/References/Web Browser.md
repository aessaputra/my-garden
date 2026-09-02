---
{"dg-publish":true,"dg-path":"Web Browser.md","permalink":"/web-browser/","title":"Web Browser","hideInFiletree":true,"tags":["network","web","guide"],"noteIcon":"","dg-note-properties":{"title":"Web Browser","category":"references","tags":["network","web","guide"],"sources":["_raw/articles/web-browser-ramotion.md","_raw/articles/how-browsers-work-mdn.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

Web browser adalah aplikasi yang mengakses dan menampilkan website di Internet: bertindak seperti penerjemah, mengambil informasi dari web server dan menampilkannya sebagai web page. Halaman ini menyintesis gambaran umum dari Ramotion dan detail teknis rendering dari MDN.

## Sejarah singkat

- 1990: WorldWideWeb (Nexus), browser pertama oleh Tim Berners-Lee (CERN), text-only.
- 1991: Line Mode Browser, akses untuk terminal lama.
- 1993: Mosaic (NCSA), browser grafis pertama yang menampilkan gambar.
- 1994: Netscape Navigator, browser komersial pertama yang dipakai luas; browser wars dimulai.
- 1995: Internet Explorer, bundling Windows sehingga market share besar; memicu inovasi cepat (kecepatan, HTTPS, multimedia, JavaScript).
- 2004: Firefox, open-source dengan fokus privasi.
- 2008: Chrome, kecepatan plus integrasi Google, jadi market leader.
- Sekarang: Chrome, Firefox, Safari, Edge, Opera dengan tabbed browsing, secure connections, web app integration.

## Komponen browser

- User Interface (UI): address bar, tombol back/forward, tabs.
- Rendering Engine: membangun representasi visual webpage. Blink (Chrome), WebKit (Safari), Gecko (Firefox).
- Networking Component: mengambil file website (kode, gambar, video) dari server.
- JavaScript Engine: eksekusi JavaScript untuk pengalaman dinamis.
- Security Components: enkripsi (HTTPS), proteksi dari website jahat.

## Tipe browser

- Desktop: fitur lengkap (tabs, extensions, keamanan lanjut).
- Mobile: layar kecil dan touch, kecepatan utama. Safari iOS, Chrome for Android.
- Embedded: versi mini dalam aplikasi lain, contoh email clients, social apps, gaming consoles.

## Fitur modern

Tabbed browsing, bookmarks, browsing history, downloads manager, search bar, UI customization, extensions/add-ons, synchronization antar perangkat, HTTPS support, pop-up blocker, incognito mode.

## Keamanan dan privasi

- HTTPS dengan indikator gembok di address bar.
- Tracker blocking dan sandboxing (isolasi website dari OS).
- Keep updated; kelola third-party cookies, permissions, clear data/cache.

## bagaimana browser bekerja
![2026-08-31-how-browser-works.webp\|How browser wors](/img/user/Attachments/2026-08-31-how-browser-works.webp)
### Navigation
1. **DNS lookup**: temukan IP asset page. Sekali per hostname; tiap hostname unik (fonts, images, scripts, ads) butuh lookup sendiri, berisiko lambat di mobile.
2. **TCP handshake**: three-way handshake (SYN, SYN-ACK, ACK), tiga pesan bolak-balik.
3. **TLS negotiation** (HTTPS): cipher enkripsi dan verifikasi server, lima round trips tambahan.
Total 8 round trips sebelum request konten.

### Response
- Browser kirim HTTP GET (HTML). **TTFB** (Time to First Byte): waktu sampai packet HTML pertama, biasanya 14KB pertama.
- **TCP slow start**: CWND (congestion window) dikendalikan; ACK diterima maka CWND digandakan, tidak ada ACK maka CWND dibagi dua. MCC = 1500 bytes via Ethernet; CWND mulai 1/2/4/10 MSS.

### Parsing
- **DOM tree**: HTML → tokenization → tree construction. Script tanpa `async`/`defer` memblokir rendering.
- **Preload scanner**: request resource prioritas tinggi (CSS, JS, fonts) di background tanpa menunggu parser utama.
- **CSSOM tree**: CSS → map of styles, cascade; sangat cepat (kurang dari satu DNS lookup).
- JavaScript compilation (main thread, kecuali web workers); accessibility tree (AOM) untuk screen readers.

### Render (critical rendering path)
1. **Style**: gabung DOM + CSSOM jadi render tree (node visible saja; `display:none` tidak masuk, `visibility:hidden` masuk).
2. **Layout**: hitung geometri tiap box; reflow = perhitungan ulang. Setiap elemen hampir selalu box.
3. **Paint**: rasterisasi box menjadi pixel. Semua kerja main thread harus < 16.67ms (60fps).
4. **Compositing**: gabungkan layer (GPU) dalam urutan benar. Layer baik untuk performa tapi mahal memory.

### Interactivity
- **Time to Interactive (TTI)**: page merespons interaksi dalam 50ms setelah First Contentful Paint.
- Main thread sibuk dengan JavaScript maka tidak responsif (jank scroll/click). Contoh: script 2MB, page terlihat cepat tapi tidak bisa scroll sampai script dieksekusi.

## Catatan performa

- Sertakan CSS dan HTML untuk render pertama dalam 14KB pertama (browser mulai render dari data yang ada).
- Jangan overuse CSS layers (memori mahal).
- Deklarasikan dimensi gambar untuk hindari reflow.
- Gunakan `async`/`defer` untuk script agar tidak memblokir parsing.

## Lihat juga

- [[References/DNS\|DNS]]: langkah 1 loading halaman, DNS lookup dari browser
- [[References/HTTP\|HTTP]]: request/response setelah DNS dan TCP/TLS selesai
- [[References/How Does Internet Work\|How Does Internet Work]]: TCP handshake, ports, sockets, TLS
- [[References/Domain Name\|Domain Name]]: nama domain yang diketik di address bar
- [[References/Web Hosting\|Web Hosting]]: server yang menyimpan file yang diminta browser