---
{"dg-publish":true,"dg-path":"Web Performance.md","permalink":"/web-performance/","title":"Web Performance","hideInFiletree":true,"tags":["references","performance","development","testing","javascript"],"noteIcon":"","dg-note-properties":{"title":"Web Performance","categories":["Web Performance","Developer Tools"],"tags":["references","performance","development","testing","javascript"],"sources":["_raw/articles/web-performance-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Web performance adalah kualitas kecepatan dan efisiensi pengalaman website atau aplikasi web. Cakupannya meliputi pengiriman resource, kemunculan content, responsiveness terhadap input, visual stability, serta konsumsi CPU, memory, network, dan server.

Performance bukan satu waktu loading. [Core Web Vitals](https://web.dev/articles/vitals) memisahkan tiga outcome pengguna: loading melalui LCP, responsiveness melalui INP, dan visual stability melalui CLS.

## Loading

Loading dimulai sebelum browser menerima HTML. DNS, connection setup, TLS, server processing, redirects, cache policy, dan network latency memengaruhi waktu pengiriman. Setelah itu browser menemukan, mengunduh, mem-parsing, dan merender resources.

[MDN menjelaskan](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/How_browsers_work) bahwa latency dan beban main thread merupakan dua masalah utama. Request chain panjang, render-blocking CSS, JavaScript besar, images, fonts, dan third-party scripts dapat menunda content.

LCP mengukur kapan elemen content terbesar yang terlihat selesai dirender. [Panduan LCP](https://web.dev/articles/lcp) menetapkan target 2,5 detik atau kurang pada persentil ke-75, dipisahkan antara mobile dan desktop.

LCP bukan ukuran seluruh kesiapan halaman. Content dapat terlihat sebelum event handler siap, sedangkan halaman interaktif dapat memiliki elemen terbesar yang lambat karena image tertentu.

## Responsiveness

Responsiveness menggambarkan seberapa cepat interface memberi frame berikutnya setelah click, tap, atau keyboard input. JavaScript task panjang, rendering mahal, synchronous layout, dan event handler berat dapat menunda feedback.

[INP](https://web.dev/articles/inp) menilai interaction latency sepanjang kunjungan. Nilai 200 milidetik atau kurang dipandang baik pada persentil ke-75. Metric ini biasanya melaporkan interaksi terburuk dengan perlakuan outlier untuk halaman yang memiliki banyak interaksi.

INP tidak mengukur seluruh interaction quality. Scrolling, animation smoothness, correctness, dan task completion membutuhkan metric atau pengujian tambahan.

## Visual stability

Layout shift yang tidak diharapkan dapat membuat pengguna kehilangan posisi membaca atau menekan kontrol yang salah. Penyebab umum mencakup media tanpa dimensi, advertisement, content injection, dan pergantian font.

[CLS](https://web.dev/articles/cls) menilai pergeseran melalui proporsi viewport yang terdampak dan jarak perpindahan. Target yang direkomendasikan adalah 0,1 atau kurang pada persentil ke-75.

CLS rendah tidak menjamin layout mudah dipahami atau accessible. Ia mengukur unexpected movement, bukan seluruh mutu visual dan interaction design.

## Lab dan field

Lab data berasal dari device dan network preset yang dikendalikan. Data ini reproducible dan cocok untuk debugging, eksperimen, serta regression testing sebelum rilis.

Field data berasal dari kunjungan nyata. Ia mencakup keragaman device, network, geography, cache state, route, interaction, dan user behavior, tetapi aggregation dapat menyembunyikan cohort lambat.

[Perbedaan lab dan field](https://web.dev/articles/lab-and-field-data-differences) bukan kontradiksi otomatis. Field data menilai outcome produksi, sedangkan lab traces membantu menemukan penyebab yang dapat ditindaklanjuti.

## Strategi optimasi

Mulai dari bottleneck yang terukur. Perbaikan dapat mencakup caching, compression, image sizing, font loading, resource prioritization, code splitting, pengurangan third-party code, server tuning, serta pembagian long task.

Performance budget dan automated checks mencegah regresi, tetapi threshold lokal harus mengikuti route dan populasi pengguna. [[References/Lighthouse\|Lighthouse]] menyediakan lab audit, sedangkan [[References/Chrome DevTools\|Chrome DevTools]] mendukung trace, network inspection, dan profiling.

Pengukuran produksi perlu dipisahkan menurut mobile, desktop, route, geography, dan release. Nilai persentil lebih berguna daripada average karena memperlihatkan pengalaman pengguna yang lebih lambat.

## Dampak dan batas

Performance dapat memengaruhi engagement, conversion, dan satisfaction, tetapi dampaknya kontekstual. [Kumpulan studi kasus web.dev](https://web.dev/articles/why-speed-matters) menunjukkan asosiasi positif pada beberapa organisasi, bukan uplift universal.

Core Web Vitals tidak menggantikan accessibility, reliability, security, correctness, atau task success. Sebuah produk dapat lulus threshold dan tetap gagal memenuhi kebutuhan pengguna.

Web Performance berhubungan dengan [[References/Web Browser\|Web Browser]], [[References/HTTP\|HTTP]], [[References/Cache-Control\|Cache-Control]], [[References/Service Workers\|Service Workers]], [[References/Server-Side Rendering\|Server-Side Rendering]], [[References/Static Site Generators\|Static Site Generators]], dan [[References/Testing Your Apps\|Testing Your Apps]].

## Lanjutan

- [[Fast loading requires reducing both latency and main-thread work\|Fast loading requires reducing both latency and main-thread work]]
- [[Responsiveness must be measured across the entire visit\|Responsiveness must be measured across the entire visit]]
- [[Visual stability protects user intent, not merely appearance\|Visual stability protects user intent, not merely appearance]]
- [[Field data should judge users while lab data diagnoses causes\|Field data should judge users while lab data diagnoses causes]]
