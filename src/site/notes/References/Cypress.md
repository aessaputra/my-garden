---
{"dg-publish":true,"dg-path":"Cypress.md","permalink":"/cypress/","title":"Cypress","hideInFiletree":true,"tags":["references","programming","javascript","typescript","testing"],"noteIcon":"","dg-note-properties":{"title":"Cypress","category":"references","tags":["references","programming","javascript","typescript","testing"],"sources":["_raw/articles/cypress-expanded.md"],"created":"2026-09-01","updated":"2026-09-01","confidence":"high"}}
---

Cypress adalah framework pengujian aplikasi web berbasis [[References/JavaScript\|JavaScript]]. Kode test berjalan di browser bersama aplikasi, sementara proses Node dan proxy Cypress menangani tugas sistem serta jaringan.

Cypress dikenal untuk end-to-end testing, tetapi cakupannya juga meliputi component, API, dan accessibility testing. Pilihan jenis test perlu mengikuti risiko yang hendak diperiksa.

## Arsitektur eksekusi

Test browser dapat membaca DOM, memakai API browser, dan berinteraksi dekat dengan aplikasi. Pekerjaan backend, seperti akses database atau proses sistem, dijalankan melalui Node events atau `cy.task()`.

Cypress mengantrikan command, lalu menjalankannya secara berurutan. Nilai dari command sebelumnya menjadi subjek command berikutnya, sehingga rangkaian `cy.get(...).find(...).should(...)` bersifat deklaratif.

Model ini berbeda dari Promise biasa. Command Cypress tidak perlu dan umumnya tidak boleh diberi `await`. Data dari rantai command diakses dengan `.then()` atau assertion Cypress.

## Mocha, Chai, BDD, dan TDD

Cypress memakai Mocha untuk struktur suite, test, dan hooks. Chai menyediakan assertion. Sinon, jQuery, dan Lodash juga dibundel untuk spies, stubs, seleksi DOM, serta utilitas.

Gaya BDD memakai `describe`, `it`, dan `expect`. Gaya TDD dapat memakai `suite`, `test`, serta assertion yang sesuai. Keduanya berbagi runner Cypress dan tidak mengubah arsitektur eksekusi.

```js
describe("login", () => {
  it("membuka dashboard", () => {
    cy.visit("/login");
    cy.get("[data-cy=email]").type("user@example.com");
    cy.get("[data-cy=submit]").click();
    cy.url().should("include", "/dashboard");
  });
});
```

## Jenis pengujian

End-to-end test menjalankan alur pengguna dari browser sampai backend dan integrasi terkait. Cakupannya tinggi, tetapi server, data, akun, dan layanan pengujian membuatnya lebih mahal untuk disiapkan dan dirawat.

Component test memasang komponen secara terisolasi dalam browser nyata. Umpan baliknya lebih cepat dan kegagalan lebih mudah dilokalisasi, tetapi tidak membuktikan seluruh sistem bekerja sebagai satu alur.

API test memeriksa endpoint tanpa selalu melalui UI. Accessibility testing membantu menemukan pelanggaran aksesibilitas. Keduanya melengkapi, bukan menggantikan, pengujian perilaku pengguna.

## Retry-ability dan automatic waiting

Query yang terhubung dan assertion dicoba ulang dari awal sampai kondisi lolos atau timeout. Non-query, seperti `cy.visit()` atau aksi yang telah dijalankan, tidak otomatis diulang dengan mekanisme yang sama.

Retry-ability mengurangi kebutuhan `cy.wait(angka)`. Tunggu kondisi yang dapat diamati, seperti elemen terlihat atau request beralias selesai, agar test mengikuti kesiapan aplikasi.

Test retries berbeda. Fitur itu menjalankan ulang test yang gagal sebanyak konfigurasi tertentu dan tidak aktif secara bawaan. Retry dapat mendeteksi flakiness, tetapi tidak memperbaiki race condition.

## Open mode dan debugging

`cypress open` menyediakan browser interaktif, aplikasi yang diuji, dan Command Log yang diperbarui saat test berjalan. Setiap command dapat dipilih untuk memeriksa snapshot DOM serta output konsol terkait.

Time travel berarti berpindah di antara snapshot command untuk melihat state pada saat itu. Fitur ini bukan mesin yang memundurkan aplikasi hidup lalu mengeksekusi ulang masa lalu secara bebas.

Open mode mengawasi perubahan spec. Ketika file berubah, Cypress memuat ulang dan menjalankan kembali seluruh test dalam spec tersebut. Ini memberi umpan balik cepat tanpa rerun manual.

## Browser dan viewport

Cypress mendukung browser keluarga Chrome, Firefox, WebKit, dan Electron berbasis Chromium yang dibundel. Browser selain Electron perlu terpasang pada mesin lokal atau lingkungan CI.

Dukungan WebKit memberi cakupan terhadap engine Safari. Itu tidak identik dengan menguji setiap versi Safari pada perangkat Apple asli, sehingga kasus yang sensitif terhadap perangkat tetap memerlukan pengujian nyata.

Ukuran viewport dapat diatur untuk memeriksa layout responsif. Perubahan viewport meniru dimensi tampilan, bukan seluruh karakteristik perangkat keras dan sistem operasi.

## Network control

`cy.intercept()` dapat mengamati request, memberi alias, memodifikasi request atau response, dan men-stub response. Intercept dibersihkan sebelum setiap test agar state jaringan tidak bocor antar-test.

Stub jaringan mempercepat skenario error dan kondisi langka. Test penting tetap perlu memakai backend nyata agar mock yang usang tidak memberi keyakinan palsu terhadap integrasi.

Cypress mem-proxy koneksi WebSocket, tetapi `cy.intercept()` belum men-stub frame atau pesan WebSocket individual. Batas ini perlu diperiksa untuk aplikasi real-time.

## Instalasi dan penggunaan

Pasang Cypress sebagai development dependency, lalu buka mode interaktif:

```bash
pnpm add -D cypress
pnpm exec cypress open
```

Jalankan suite headless untuk otomasi:

```bash
pnpm exec cypress run
```

TypeScript didukung melalui deklarasi tipe resmi. Spec dapat ditulis dalam TypeScript, tetapi model runtime Cypress tetap berpusat pada JavaScript di browser.

## CI dan keandalan

Pipeline CI umumnya memasang dependensi, menyalakan aplikasi, menunggu server siap, lalu menjalankan `cypress run`. Menjalankan test sebelum server siap menimbulkan race condition yang tidak berkaitan dengan kualitas aplikasi.

Parallelization dan distribusi spec dapat mempersingkat durasi, tetapi menambah penggunaan runner dan kompleksitas state. Database, akun, port, serta data fixture harus terisolasi.

Simpan screenshot, video, atau hasil rekaman saat gagal sesuai kebutuhan. Artefak membantu diagnosis CI, tetapi retensi pada setiap test dapat menambah waktu, penyimpanan, dan biaya.

## Praktik yang disarankan

Gunakan selector stabil seperti atribut `data-cy`. Hindari selector yang bergantung pada class styling atau struktur DOM yang tidak memiliki makna perilaku.

Buat test mandiri. Siapkan state melalui API atau task, kendalikan data, dan hindari ketergantungan pada urutan spec. Jangan menyelesaikan flakiness hanya dengan menaikkan timeout.

Rahasia tidak boleh dimasukkan ke kode atau diekspos ke konteks browser. Simpan secret di lingkungan CI dan gunakan mekanisme Cypress yang mempertahankan akses istimewa sesuai kebutuhan.

## Trade-off dan pemilihan alat

Cypress memberi debugging lokal yang kuat, retry-ability, network control, dan integrasi erat dengan browser. Nilai utamanya adalah loop pengembangan yang dapat diperiksa langsung.

Cypress tidak mengendalikan lebih dari satu browser terbuka secara bersamaan. Skenario multi-user lintas browser, multi-tab kompleks, atau otomatisasi di luar web dapat memerlukan plugin atau alat lain.

Gunakan Cypress untuk alur web penting dan component test yang mendapat manfaat dari browser interaktif. Gunakan [[References/Jest\|Jest]] atau [[References/Vitest\|Vitest]] untuk unit test cepat, lalu bandingkan [[References/Playwright\|Playwright]] bila lintas konteks menjadi kebutuhan utama.

## Keterbatasan dan confidence

Confidence tinggi untuk perilaku yang dijelaskan karena seluruh klaim material berasal dari dokumentasi resmi Cypress. Tidak ada benchmark independen, sehingga halaman ini tidak mengklaim Cypress paling cepat atau paling stabil.

Dukungan browser, API, serta fitur berbayar Cypress Cloud berubah seiring waktu. Periksa dokumentasi versi aktif sebelum keputusan bergantung pada kompatibilitas atau biaya tertentu.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Jest\|Jest]]
- [[References/Vitest\|Vitest]]
- [[References/Playwright\|Playwright]]
- [[References/Web Browser\|Web Browser]]

## Sumber

- [Introduction to Cypress](https://docs.cypress.io/app/core-concepts/introduction-to-cypress): command, query, subject, assertion, dan model asinkron.
- [Testing Types](https://docs.cypress.io/app/core-concepts/testing-types): end-to-end, component, API, dan accessibility testing.
- [Retry-ability](https://docs.cypress.io/app/core-concepts/retry-ability): query, assertion, non-query, timeout, dan automatic waiting.
- [Open Mode](https://docs.cypress.io/app/core-concepts/open-mode): Command Log, time travel, snapshot, dan debugging interaktif.
- [Writing and Organizing Tests](https://docs.cypress.io/app/core-concepts/writing-and-organizing-tests): Mocha, Chai, BDD, TDD, struktur proyek, dan watch mode.
- [Bundled Libraries](https://docs.cypress.io/app/references/bundled-libraries): Mocha, Chai, Sinon, jQuery, dan Lodash.
- [Trade-offs](https://docs.cypress.io/app/references/trade-offs): konteks browser, jalur Node, multi-browser, WebSocket, dan batas arsitektur.
- [Install Cypress](https://docs.cypress.io/app/get-started/install-cypress): instalasi paket, binary, system requirements, dan Electron.
- [Cross Browser Testing](https://docs.cypress.io/app/guides/cross-browser-testing): Chrome-family, Firefox, WebKit, dan strategi CI.
- [cy.intercept](https://docs.cypress.io/api/commands/intercept): spying, stubbing, route matching, request, dan response.
- [Test Retries](https://docs.cypress.io/app/guides/test-retries): konfigurasi retry test, hooks, dan deteksi flakiness.
- [Best Practices](https://docs.cypress.io/app/core-concepts/best-practices): selector, isolasi, state, waiting, dan pengelolaan rahasia.
- [TypeScript Support](https://docs.cypress.io/app/tooling/typescript-support): deklarasi tipe, IntelliSense, dan konfigurasi TypeScript.
- [Continuous Integration](https://docs.cypress.io/app/continuous-integration/overview): instalasi, server readiness, caching, artefak, dan parallelization.
