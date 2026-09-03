---
{"dg-publish":true,"dg-path":"SvelteKit.md","permalink":"/svelte-kit/","title":"SvelteKit","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance"],"noteIcon":"","dg-note-properties":{"title":"SvelteKit","category":"references","tags":["references","programming","javascript","architecture","performance"],"sources":["_raw/articles/sveltekit-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

SvelteKit adalah framework aplikasi web untuk [[References/Svelte\|Svelte]] yang menyediakan routing, pemuatan data, penanganan formulir, endpoint server, rendering, pembangunan, dan deployment dalam satu struktur proyek. Framework ini dapat digunakan untuk situs statis, aplikasi dengan server-side rendering, aplikasi satu halaman, maupun kombinasi beberapa strategi tersebut per rute.

SvelteKit sering disebut sederhana dan cepat, tetapi kedua istilah itu perlu diberi konteks. Konvensi berbasis berkas mengurangi konfigurasi awal, sementara performa dipengaruhi oleh strategi rendering, kode aplikasi, data, aset, adapter, dan infrastruktur deployment. Keunggulan proyek tetap perlu diukur pada hasil produksi, bukan disimpulkan dari framework saja.

## Struktur proyek dan routing berbasis berkas

Direktori `src/routes` menentukan URL aplikasi. Sebuah `+page.svelte` mendefinisikan halaman, `+layout.svelte` membungkus halaman dan layout di bawahnya, `+error.svelte` menangani kesalahan pada cabang rute, dan `+server.js` atau `+server.ts` mendefinisikan endpoint HTTP.

```text
src/routes/
├── +layout.svelte
├── +page.svelte
├── about/
│   └── +page.svelte
└── blog/
    └── [slug]/
        ├── +page.server.ts
        └── +page.svelte
```

Struktur tersebut menghasilkan rute `/`, `/about`, dan `/blog/[slug]`. Nama direktori dalam kurung siku menjadi parameter dinamis yang tersedia melalui `params`, sehingga `/blog/hello-world` memberikan nilai `slug` berupa `hello-world`.

Halaman dirender di server pada permintaan awal dan di browser untuk navigasi berikutnya secara bawaan. Layout berlaku bagi subdirektori di bawahnya dan dapat memuat data sendiri melalui `+layout.js` atau `+layout.server.js`.

## Memuat data

SvelteKit menggunakan fungsi `load` untuk menyediakan data sebelum halaman atau layout dirender. Fungsi dalam `+page.js` atau `+layout.js` bersifat universal. Fungsi tersebut berjalan di server saat SSR dan dapat berjalan di browser pada navigasi sisi klien.

Gunakan `+page.server.js` atau `+layout.server.js` ketika pemuatan data harus selalu berlangsung di server, misalnya karena mengakses basis data atau variabel lingkungan privat.

```ts
// src/routes/blog/[slug]/+page.server.ts
import {error} from "@sveltejs/kit";
import type {PageServerLoad} from "./$types";
import {getPost} from "$lib/server/posts";

export const load: PageServerLoad = async ({params}) => {
  const post = await getPost(params.slug);

  if (!post) error(404, "Artikel tidak ditemukan");

  return {post};
};
```

```svelte
<!-- src/routes/blog/[slug]/+page.svelte -->
<script lang="ts">
  let {data} = $props();
</script>

<h1>{data.post.title}</h1>
<div>{@html data.post.body}</div>
```

Data dari fungsi `load` diberikan kepada halaman melalui properti `data`. Data layout juga mengalir ke layout dan halaman turunannya. Pemisahan antara modul universal dan modul server membantu mencegah kode rahasia masuk ke bundle browser, tetapi pengembang tetap harus menempatkan akses privat hanya di modul server.

## Form actions dan mutasi

Berkas `+page.server.js` dapat mengekspor `actions` untuk menerima permintaan `POST` dari elemen `<form>`. Pendekatan ini bekerja tanpa JavaScript sisi klien, kemudian dapat ditingkatkan secara progresif agar pengalaman pengguna lebih halus.

```ts
// src/routes/login/+page.server.ts
import {fail, redirect} from "@sveltejs/kit";
import type {Actions} from "./$types";

export const actions: Actions = {
  default: async ({request, cookies}) => {
    const form = await request.formData();
    const email = form.get("email");

    if (typeof email !== "string" || !email) {
      return fail(400, {email, missing: true});
    }

    cookies.set("session", await createSession(email), {path: "/"});
    redirect(303, "/dashboard");
  }
};
```

Setelah action selesai tanpa pengalihan atau kesalahan tak terduga, halaman dirender kembali dan fungsi `load` terkait dijalankan ulang. Dokumentasi juga mencatat *remote functions* sebagai API eksperimental yang ditujukan menjadi cara yang direkomendasikan untuk komunikasi klien dan server pada pengembangan baru. Form actions tetap lengkap dan akan terus berfungsi, sedangkan API eksperimental dapat berubah.

## Endpoint server dan hooks

Berkas `+server.js` mengekspor fungsi berdasarkan metode HTTP, seperti `GET`, `POST`, `PATCH`, atau `DELETE`. Hasilnya adalah endpoint yang dapat mengembalikan objek `Response`, sehingga SvelteKit dapat menyediakan API dalam proyek yang sama dengan antarmuka.

Hooks memungkinkan aplikasi menyisipkan perilaku ke dalam alur request. Hook server `handle` berjalan untuk setiap request dan dapat mengisi `event.locals`, memeriksa sesi, menambahkan header, atau meneruskan request melalui `resolve`. `handleFetch` dapat mengubah request yang dibuat server, sedangkan hook lain menangani kesalahan dan transport data.

Autentikasi di hook dapat menyediakan identitas pengguna kepada fungsi `load`, action, dan endpoint. Otorisasi tetap harus diperiksa pada setiap operasi server yang dilindungi. Menyembunyikan tombol atau mengalihkan halaman di browser bukan pengganti pemeriksaan izin pada server.

## SSR, prerendering, dan CSR

SvelteKit merender komponen pertama kali di server secara bawaan, mengirim HTML ke browser, lalu melakukan hidrasi agar halaman interaktif. Router sisi klien mengambil alih navigasi berikutnya.

Tiga opsi halaman mengatur strategi rendering:

- `ssr` menentukan apakah halaman dirender di server.
- `csr` menentukan apakah JavaScript sisi klien dimuat untuk hidrasi dan navigasi.
- `prerender` menentukan apakah keluaran dibuat sebagai HTML statis saat proses pembangunan.

Opsi dapat ditetapkan pada halaman, kelompok rute melalui layout, atau seluruh aplikasi melalui layout akar. Karena anak dapat mengganti nilai induknya, satu aplikasi dapat melakukan prerender untuk halaman pemasaran, SSR untuk halaman dinamis, dan rendering sisi klien untuk bagian administrasi.

```ts
// src/routes/blog/+layout.ts
export const prerender = true;
```

Prerendering sesuai ketika dua pengguna yang meminta URL yang sama dapat menerima konten server yang sama. Halaman personal, data yang harus selalu mutakhir, atau konten berdasarkan sesi biasanya memerlukan rendering dinamis. Data pribadi tidak boleh dipanggang ke keluaran statis yang dapat dilihat semua pengguna.

## Static site generation dan SPA

`@sveltejs/adapter-static` menghasilkan kumpulan berkas statis untuk hosting tanpa server JavaScript. Jika seluruh situs dapat diprerender, adapter tersebut berfungsi sebagai static site generator. Untuk campuran halaman statis dan SSR, gunakan adapter yang mendukung server bersama opsi `prerender` pada rute yang sesuai.

SvelteKit juga dapat menghasilkan SPA yang dirender sepenuhnya di browser dengan menonaktifkan SSR dan menentukan halaman fallback. Dokumentasi menyarankan prerendering sebanyak mungkin karena SPA murni dapat membawa dampak pada waktu muat awal, aksesibilitas, dan SEO. Jika semua halaman dapat diprerender, SSG biasanya lebih tepat daripada fallback SPA.

## Code splitting dan navigasi

SvelteKit melakukan *code splitting* agar browser hanya memuat kode yang diperlukan oleh halaman saat ini. Framework ini juga memiliki pramuat aset, pemuatan fungsi `load` secara paralel, penggabungan request data server, inlining data hasil SSR, invalidasi konservatif, dan pramuat tautan.

Optimasi tersebut tidak menjamin aplikasi otomatis cepat. Gambar besar, query lambat, JavaScript pihak ketiga, layout yang memblokir, dan konfigurasi cache yang buruk tetap dapat mendominasi waktu muat. Dokumentasi menyarankan pengujian terhadap build produksi melalui mode preview, bukan hanya server pengembangan.

## Adapter dan deployment

SvelteKit memakai adapter untuk mengubah hasil build menjadi keluaran yang sesuai dengan target deployment. Adapter resmi tersedia untuk Node, Cloudflare, Netlify, Vercel, dan hosting statis. Adapter dipilih di `svelte.config.js`.

```js
import adapter from "@sveltejs/adapter-node";

export default {
  kit: {
    adapter: adapter()
  }
};
```

Adapter bukan sekadar tombol deployment universal. Dukungan streaming, runtime Node, batas ukuran fungsi, akses filesystem, variabel lingkungan, dan API platform berbeda pada setiap target. Aplikasi perlu diuji pada lingkungan yang menyerupai deployment sebenarnya dan mengikuti dokumentasi adapter yang dipilih.

## Batas penggunaan dan pemilihan

SvelteKit sesuai ketika proyek Svelte memerlukan routing terstruktur, pemuatan data, endpoint server, formulir, beberapa strategi rendering, dan deployment melalui adapter. Konvensinya mengurangi keputusan awal, tetapi proyek kompleks tetap memerlukan desain untuk autentikasi, otorisasi, cache, basis data, observabilitas, dan batas runtime.

Pilih strategi rendering per jenis halaman. Gunakan prerendering untuk konten stabil, SSR untuk HTML dinamis yang harus tersedia pada respons awal, dan CSR ketika interaksi browser memang tidak membutuhkan hasil server. Ukur hasil produksi berdasarkan Core Web Vitals, ukuran JavaScript, waktu respons server, pola cache, dan biaya operasional.

## Lihat juga

- [[References/Svelte\|Svelte]]
- [[References/React\|React]]
- [[References/Next.js\|Next.js]]
- [[References/React Router\|React Router]]
- [[References/Vite\|Vite]]
- [[References/JavaScript\|JavaScript]]

## Sumber

- [SvelteKit introduction](https://svelte.dev/docs/kit): pengenalan framework, kemampuan utama, dan panduan memulai.
- [Routing](https://svelte.dev/docs/kit/routing): file-based routing, +page, +layout, +server, parameter dinamis, dan rute akar.
- [Loading data](https://svelte.dev/docs/kit/load): fungsi load, data prop, layout data, universal vs server load, dan ketergantungan.
- [Form actions](https://svelte.dev/docs/kit/form-actions): POST form, default action, named actions, form prop, dan remote functions.
- [Page options](https://svelte.dev/docs/kit/page-options): prerender, ssr, csr, entry, dan strategi rendering per halaman.
- [Adapters](https://svelte.dev/docs/kit/adapters): adapter resmi untuk Node, Cloudflare, Netlify, Vercel, dan static.
- [Static site generation](https://svelte.dev/docs/kit/adapter-static): SSG, fallback SPA, dan konfigurasi prerendering.
- [Single-page apps](https://svelte.dev/docs/kit/single-page-apps): SPA fallback, SSR opsional per halaman, dan deployment statis.
- [Hooks](https://svelte.dev/docs/kit/hooks): handle, handleFetch, locals, request event, dan hook server.
- [Performance](https://svelte.dev/docs/kit/performance): code splitting, pramuat, parallel loading, invalidasi, dan diagnosa.
- [Repositori resmi SvelteKit](https://github.com/sveltejs/kit): kode sumber, rilis, masalah, dan diskusi proyek.
