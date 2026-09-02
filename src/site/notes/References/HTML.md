---
{"dg-publish":true,"dg-path":"HTML.md","permalink":"/html/","title":"HTML","hideInFiletree":true,"tags":["network","web","html","guide"],"dg-note-properties":{"title":"HTML","category":"references","tags":["network","web","html","guide"],"sources":["_raw/articles/html-mdn.md","_raw/articles/html-wikipedia.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

HTML (HyperText Markup Language) adalah standard markup language dan building block paling dasar dari Web: mendefinisikan makna dan struktur konten web. Halaman ini menyintesis definisi dan panduan dari MDN plus sejarah dan markup detail dari Wikipedia.

## Definisi

HTML adalah standard markup language untuk dokumen yang ditampilkan di web browser. Mendefinisikan konten dan struktur konten web. Sering dibantu CSS (penampilan/presentation) dan JavaScript (fungsi/behavior).

"Hypertext" merujuk pada links yang menghubungkan web pages, baik dalam satu website atau antar website. Dengan meng-upload konten dan me-link ke halaman lain, kamu jadi partisipan aktif World Wide Web.

Fakta teknis: extension `.html`/`.htm`, media type `text/html`, dikembangkan WHATWG (W3C formerly), initial release 1993, latest release Living Standard, extended dari SGML, extended ke XHTML.

## Sejarah

- 1980: Tim Berners-Lee (CERN) memprototipe ENQUIRE, sistem berbagi dokumen.
- 1989: memo Internet-based hypertext system; late 1990 menulis browser dan server pertama, spesifikasi HTML.
- Late 1991: "HTML Tags", deskripsi publik pertama, 18 elemen (11 masih ada di HTML 4).
- HTML formal sebagai aplikasi SGML via IETF mid-1993.
- 1995: HTML 2.0 (RFC 1866), spesifikasi standar pertama.
- 1996-: dipelihara W3C; 2000 jadi standar internasional ISO/IEC 15445:2000.
- 2004: HTML5 dimulai di WHATWG; distandardisasi 28 Oktober 2014.
- 2019: WHATWG jadi sole publisher standar HTML dan DOM (Living Standard).

## Version timeline

- HTML 2.0 (RFC 1866, Nov 1995). RFCs pendamping: file upload, tables, image maps, internationalization.
- HTML 3.2 (W3C, Jan 1997). Versi pertama murni W3C; adopsi mayoritas visual markup Netscape.
- HTML 4.0/4.01 (W3C 1997/1999). Tiga variasi Strict/Transitional/Frameset; mulai phase-out visual markup demi CSS.
- XHTML 1.0/1.1 (2000/2001). Reformulation HTML 4.01 dengan XML; XHTML 2.0 ditinggalkan 2009.
- HTML5 (28 Okt 2014 W3C Rec), 5.1 (2016), 5.2 (2017). Sekarang Living Standard di WHATWG.

## Markup dan elemen

HTML markup terdiri dari tags (dan atribut), character-based data types, character references, entity references.

- Tags berpasangan `<p>...</p>` untuk elemen; empty elements tanpa end tag (`<br>`, `<img>`).
- Nama tag case-insensitive; konvensi lowercase.
- Start tag bisa berisi atribut (id, class, href, src).
- `<DOCTYPE html>` memicu standards mode; tanpa deklarasi, browser revert ke quirks mode.

Contoh:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>This is a title</title>
  </head>
  <body>
    <div>
        <p>Hello world!</p>
    </div>
  </body>
</html>
```

Elemen umum: headings `<h1>`-`<h6>`, paragraphs `<p>`, line breaks `<br>`, links `<a href="...">`, inputs `<input type="...">`, comments `<!-- ... -->`.

Tipe markup: structural (tujuan teks), presentational (penampilan, kini via CSS), hypertext (menghubungkan dokumen).

## Fitur penting

- Forms: registrasi/login, feedback, beli produk. HTML5 constraint validation memungkinkan validasi client-side tanpa JavaScript via atribut form element.
- data-\* attributes: simpan data ekstra di elemen semantik.
- Microdata/microformats: metadata terstruktur untuk search engines dan aggregators.
- Responsive images: gambar adaptif untuk ukuran layar berbeda.
- Audio/video native: tanpa external software.
- Content categories: aturan konten mana yang boleh di konteks mana.
- Quirks vs standards mode: deklarasi doctype menentukan mode rendering.

## Peran dalam web

HTML hanya struktur. Penampilan dikerjakan CSS, perilaku dikerjakan JavaScript. Sejak 1997 W3C mendorong CSS di atas presentational HTML. Browser merender HTML menjadi multimedia web pages; tags tidak ditampilkan, hanya dipakai menginterpretasi konten.

## Lihat juga

- [[References/Web Browser\|Web Browser]]: bagaimana browser merender HTML (parsing DOM, CSSOM, critical rendering path)
- [[References/HTTP\|HTTP]]: protokol yang mengirim dokumen HTML dari server ke browser
- [[References/Domain Name\|Domain Name]]: nama domain yang membawa ke halaman HTML
- [[References/Web Hosting\|Web Hosting]]: server yang menyimpan file HTML
- [[References/DNS\|DNS]]: menerjemahkan nama domain ke server tempat HTML berada