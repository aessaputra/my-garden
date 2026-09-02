---
{"dg-publish":true,"dg-path":"React Router.md","permalink":"/react-router/","title":"React Router","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance"],"noteIcon":"","dg-note-properties":{"title":"React Router","category":"references","tags":["references","programming","javascript","architecture","performance"],"sources":["_raw/articles/react-router-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

React Router adalah pustaka routing untuk [[References/React\|React]] yang mencocokkan lokasi URL dengan antarmuka, mengelola navigasi, dan menyusun tampilan berdasarkan hierarki rute. Pustaka ini dapat dipakai untuk aplikasi satu halaman, tetapi versi modernnya tidak terbatas pada routing sisi klien. Framework Mode juga mendukung rendering sisi server dan prerendering statis.

React Router merupakan salah satu pilihan routing utama dalam ekosistem React, tetapi menyebutnya sebagai solusi standar untuk setiap aplikasi terlalu mutlak. [[References/React\|React]] sendiri tidak mewajibkan router tertentu, dan kebutuhan routing dapat ditangani oleh framework atau arsitektur lain. Pemilihannya sebaiknya didasarkan pada kebutuhan navigasi, pemuatan data, rendering, dan deployment.

## Tiga mode penggunaan

Dokumentasi React Router membagi penggunaannya menjadi Declarative Mode, Data Mode, dan Framework Mode. Fitur ketiganya bersifat bertingkat. Data Mode menambahkan kemampuan di atas Declarative Mode, sedangkan Framework Mode menambahkan perangkat framework di atas Data Mode.

Declarative Mode menyediakan pencocokan URL, navigasi, lokasi aktif, serta API seperti `<Link>`, `useNavigate`, dan `useLocation`. Mode ini sesuai ketika aplikasi sudah memiliki lapisan data sendiri atau hanya memerlukan routing komponen.

Data Mode memindahkan konfigurasi rute ke luar proses render React. Mode ini menambahkan `loader`, `action`, status navigasi tertunda, dan `useFetcher`. Pendekatan tersebut menghubungkan kebutuhan data dan mutasi dengan rute yang menggunakannya.

Framework Mode memakai plugin [[References/Vite\|Vite]] dan Route Module API. Mode ini menyediakan tipe untuk parameter serta data rute, pemisahan kode, dan strategi SPA, SSR, serta rendering statis. Konsekuensinya, React Router mengambil tanggung jawab lebih besar atas struktur aplikasi dan proses pembangunan.

## Mendefinisikan rute

Dalam Declarative Mode, `<Routes>` dan `<Route>` menghubungkan segmen URL dengan elemen React.

```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Outlet
} from "react-router";

function DashboardLayout() {
  return (
    <main>
      <h1>Dashboard</h1>
      <Outlet />
    </main>
  );
}

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="dashboard" element={<DashboardLayout />}>
          <Route index element={<Overview />} />
          <Route path="settings" element={<Settings />} />
        </Route>
        <Route path="products/:productId" element={<Product />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Rute dapat disusun secara bertingkat, dan rute anak dirender melalui `<Outlet>` milik rute induk. *Index route* bertindak sebagai anak bawaan pada URL induknya, sedangkan *layout route* menambahkan susunan UI tanpa menambah segmen URL. Segmen yang diawali `:` menjadi parameter dinamis, dan React Router juga mendukung segmen opsional serta *splat* untuk mencocokkan sisa jalur.

Data Mode memakai objek rute dan `createBrowserRouter`. Selain `path` dan komponen, objek tersebut dapat mendefinisikan pemuatan data serta aksi.

```jsx
import {createBrowserRouter, RouterProvider} from "react-router";

const router = createBrowserRouter([
  {
    path: "/projects/:projectId",
    loader: async ({params}) => {
      return fetch(`/api/projects/${params.projectId}`);
    },
    Component: Project
  }
]);

export default function App() {
  return <RouterProvider router={router} />;
}
```

## Navigasi dan URL

`<Link>` dan `<NavLink>` memungkinkan navigasi melalui elemen antarmuka tanpa menyerahkan seluruh perpindahan halaman kepada browser. `NavLink` juga menyediakan keadaan aktif untuk menandai tautan yang sesuai dengan lokasi saat ini.

Hook `useNavigate` mengembalikan fungsi untuk navigasi terprogram, termasuk berpindah ke URL tertentu, mengganti entri riwayat, atau bergerak relatif dalam *history stack*. Dokumentasi menyarankan `redirect` dalam `loader` atau `action` ketika pengalihan memang merupakan hasil pemuatan data atau mutasi, alih-alih selalu menjalankan `useNavigate` dari efek komponen.

```jsx
import {useNavigate} from "react-router";

function CancelButton() {
  const navigate = useNavigate();
  return <button onClick={() => navigate(-1)}>Batal</button>;
}
```

Navigasi berbasis angka seperti `navigate(-1)` bergantung pada riwayat browser yang tersedia. Aplikasi perlu menghindari asumsi bahwa selalu ada entri sebelumnya yang berasal dari domain atau alur yang diharapkan.

## Pemuatan data dan mutasi

Dalam Data Mode dan Framework Mode, `loader` menyediakan data untuk rute, sedangkan `action` menangani mutasi yang dipicu oleh navigasi atau formulir. React Router memanggil *loader* yang sesuai sebelum merender keadaan rute berikutnya dan dapat mengelola status tertunda selama navigasi.

Route Module dalam Framework Mode dapat mengekspor `loader`, `action`, batas kesalahan, metadata, serta komponen rute. Data dan parameter dapat diterima sebagai properti yang diturunkan tipenya dari konfigurasi rute. Model ini mengurangi kebutuhan mengulang tipe parameter dan hasil *loader* secara manual.

Elemen `<Form>` mengintegrasikan pengiriman formulir dengan navigasi dan `action` rute. Setelah aksi selesai, data *loader* terkait dapat divalidasi ulang agar antarmuka mengikuti keadaan server terbaru.

## Lazy loading dan pemisahan kode

React Router mendukung pemisahan kode, tetapi mekanismenya berbeda menurut mode. Framework Mode dapat memisahkan modul rute secara otomatis dan mendukung pemisahan ekspor sisi klien ke *chunk* terpisah. Dalam Data Mode, properti `lazy` pada objek rute dapat memuat implementasi rute secara asinkron.

```jsx
const router = createBrowserRouter([
  {
    path: "/reports",
    lazy: () => import("./routes/reports.js")
  }
]);
```

Pemisahan kode mengurangi JavaScript awal ketika rute yang jarang dipakai dipindahkan ke *chunk* lain. Hasil aktual tetap bergantung pada struktur rute, bundler, dependensi bersama, dan pola navigasi pengguna.

## Autentikasi dan pembatasan akses

React Router tidak menyediakan satu komponen `RouteGuard` universal. Pembatasan akses dapat diterapkan dengan memeriksa sesi di `loader`, melakukan `redirect`, membungkus elemen rute, atau memakai middleware sesuai mode aplikasi.

Middleware dapat menjalankan kode sebelum dan sesudah penanganan rute untuk autentikasi, pencatatan, penanganan kesalahan, atau prapemrosesan data. Middleware server tersedia dalam Framework Mode, sedangkan middleware klien dapat dipakai pada navigasi browser dalam Framework Mode dan Data Mode. Karena middleware server dan klien berjalan pada konteks berbeda, pemeriksaan otorisasi yang melindungi data tetap harus ditegakkan pada server.

Contoh konseptual pada *loader*:

```js
import {redirect} from "react-router";

export async function loader({request}) {
  const user = await getUserFromSession(request);
  if (!user) throw redirect("/login");
  return {user};
}
```

## Penanganan kesalahan dan keadaan navigasi

Data Mode dan Framework Mode menyediakan keadaan navigasi tertunda yang dapat dipakai untuk indikator pemuatan atau antarmuka optimistis. Route Module juga mendukung batas kesalahan pada tingkat rute sehingga kegagalan pemuatan, aksi, atau render dapat ditangani dekat dengan bagian antarmuka yang terdampak. Rute bertingkat membuat batas kesalahan dan pemuatan data dapat mengikuti hierarki UI.

Namun, pembagian yang terlalu rinci juga menambah jumlah status yang perlu diuji. Tim perlu menguji navigasi langsung, tombol kembali dan maju, kegagalan jaringan, pengalihan autentikasi, serta URL yang tidak cocok.

## Pemilihan mode dan batas penggunaan

Declarative Mode sesuai untuk routing komponen dengan kontrol penuh atas lapisan data dan bundling. Data Mode sesuai ketika aplikasi membutuhkan *loader*, *action*, serta status navigasi tetapi tetap ingin mengendalikan abstraksi server dan proses pembangunan. Framework Mode sesuai ketika proyek menginginkan integrasi routing, data, pemisahan kode, tipe rute, dan strategi rendering dalam satu susunan.

React Router menangani routing dan, pada mode tertentu, orkestrasi data serta rendering. Pustaka ini tidak menggantikan autentikasi server, kontrol otorisasi, validasi input, atau kebijakan cache aplikasi. Keputusan adopsi perlu mempertimbangkan mode yang digunakan, strategi deployment, integrasi data, kebutuhan SSR, dan biaya migrasi, bukan popularitas semata.

## Lihat juga

- [[References/React\|React]]
- [[References/Vite\|Vite]]
- [[References/TanStack Start\|TanStack Start]]
- [[References/Next.js\|Next.js]]
- [[References/JavaScript\|JavaScript]]
- [[References/SSR Client di Supabase\|SSR Client di Supabase]]

## Sumber

- [React Router](https://reactrouter.com/home): dokumentasi utama, API, mode penggunaan, dan panduan memulai.
- [Picking a Mode](https://reactrouter.com/start/modes): perbedaan Declarative Mode, Data Mode, dan Framework Mode.
- [Declarative routing](https://reactrouter.com/start/declarative/routing): Routes, Route, rute bertingkat, Outlet, parameter, dan splat.
- [Data routing](https://reactrouter.com/start/data/routing): objek rute, createBrowserRouter, loader, action, dan hierarki rute.
- [Framework routing](https://reactrouter.com/start/framework/routing): konfigurasi routes.ts, modul rute, nesting, dan rendering.
- [Route modules](https://reactrouter.com/start/framework/route-module): loader, action, tipe rute, batas kesalahan, dan middleware.
- [Middleware](https://reactrouter.com/how-to/middleware): autentikasi, logging, konteks, serta middleware server dan klien.
- [useNavigate](https://reactrouter.com/api/hooks/useNavigate): navigasi terprogram, history stack, replace, dan state.
- [Upgrading to v7](https://reactrouter.com/upgrading/v7): fitur versi 7, middleware, serta pemisahan ekspor rute klien.
- [Repositori resmi React Router](https://github.com/remix-run/react-router): kode sumber, paket, dokumentasi, dan rilis proyek.
- [Data loading](https://reactrouter.com/start/data/data-loading): loader, data rute, pemuatan paralel, dan revalidasi.
- [Actions](https://reactrouter.com/start/data/actions): mutasi data, formulir, action, dan status navigasi.
- [Data mode navigation](https://reactrouter.com/start/data/navigating): Link, NavLink, Form, serta navigasi tertunda.
- [Code splitting](https://reactrouter.com/how-to/code-splitting): lazy route, pemisahan modul rute, dan chunk sisi klien.
