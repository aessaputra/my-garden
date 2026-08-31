---
{"dg-publish":true,"dg-path":"SolidJS.md","permalink":"/solid-js/","title":"SolidJS","hideInFiletree":true,"tags":["references","frameworks","javascript","typescript","ui","architecture","performance"],"dg-note-properties":{"title":"SolidJS","category":"references","tags":["references","frameworks","javascript","typescript","ui","architecture","performance"],"sources":["_raw/articles/solidjs-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

SolidJS adalah pustaka JavaScript deklaratif untuk membangun antarmuka pengguna. Alih-alih memakai *virtual DOM*, Solid mengompilasi templat JSX menjadi operasi pada DOM nyata, lalu memperbarui hanya bagian yang bergantung pada data yang berubah. Pendekatan ini disebut reaktivitas berbutir halus (*fine-grained reactivity*).

## Cara kerja reaktivitas

Reaktivitas Solid berpusat pada *signal*. Fungsi `createSignal` menghasilkan dua fungsi: *getter* untuk membaca nilai dan *setter* untuk memperbaruinya. Ketika *getter* dipanggil di dalam lingkup pelacakan, Solid mencatat hubungan antara *signal* dan pelanggan yang menggunakannya. Perubahan melalui *setter* kemudian hanya menjalankan kembali pelanggan tersebut.

```jsx
import { createSignal } from "solid-js";

function Counter() {
  const [count, setCount] = createSignal(0);

  return (
    <button onClick={() => setCount(value => value + 1)}>
      Jumlah: {count()}
    </button>
  );
}
```

Dalam contoh ini, perubahan `count` memperbarui teks angka tanpa menjalankan ulang seluruh komponen. Komponen Solid umumnya dijalankan sekali untuk menyiapkan tampilan dan hubungan reaktifnya. Pembaruan berikutnya terjadi pada ekspresi yang bergantung pada *signal*, bukan melalui siklus render ulang komponen.

## JSX yang familier, model eksekusi yang berbeda

Solid mendukung JSX dan [[typescript\|TypeScript]], sehingga bentuk komponennya tampak familier bagi pengembang [[References/React\|React]]. Kemiripan tersebut terutama berada pada sintaks, bukan pada model eksekusinya. Di Solid, akses data reaktif dilakukan melalui fungsi seperti `count()`, sedangkan komponen tidak dijalankan ulang untuk setiap perubahan.

Perbedaan ini memengaruhi cara menulis kode. Membaca nilai reaktif di luar lingkup pelacakan membuat pembacaan tersebut tidak mengikuti perubahan berikutnya. Solid menyediakan primitif seperti `createEffect` dan `createMemo` untuk membuat lingkup pelacakan secara eksplisit.

## Pengelolaan state dan karakteristik

Solid menyediakan primitif reaktif untuk menyimpan, menurunkan, dan merespons perubahan data. `createSignal` menyimpan nilai, `createMemo` membentuk komputasi turunan, dan `createEffect` menjalankan efek saat dependensinya berubah. JSX juga bertindak sebagai lingkup pelacakan agar tampilan tetap sinkron dengan *state*.

Pustaka ini dirancang agar kecil, dapat di-*tree-shake*, dan efisien saat merender di klien maupun server. Klaim kinerja tetap perlu dinilai dalam konteks aplikasi dan metode pengujian, bukan dari arsitektur pustaka saja.

SolidJS cocok bagi pengembang yang menginginkan JSX dan TypeScript dengan pembaruan DOM yang langsung serta terarah. Sintaks awalnya familier, tetapi pola reaktivitasnya berbeda dari React. Karena itu, memahami *signal*, lingkup pelacakan, dan model komponen yang dijalankan sekali lebih penting daripada sekadar mengenali JSX.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[typescript\|TypeScript]]
- [[References/React\|React]]
- [[References/Svelte\|Svelte]]
- [[References/Vue.js\|Vue.js]]
- [[References/Angular\|Angular]]
- [[References/Astro\|Astro]]
- [[References/Pick a Framework\|Pick a Framework]]

## Sumber

- [Repositori resmi SolidJS](https://github.com/solidjs/solid): arsitektur, JSX, pembaruan DOM, TypeScript, dan karakteristik pustaka.
- [Intro to reactivity](https://docs.solidjs.com/concepts/intro-to-reactivity): model komponen, *signal*, pelanggan, dan pembaruan tampilan.
- [Signals](https://docs.solidjs.com/concepts/signals): `createSignal`, *getter*, *setter*, pelacakan dependensi, `createEffect`, dan `createMemo`.
- [State management](https://docs.solidjs.com/guides/state-management): primitif reaktif dan sinkronisasi JSX dengan *state*.
