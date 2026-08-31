---
{"dg-publish":true,"dg-path":"AI dan Pengembangan Perangkat Lunak Tradisional.md","permalink":"/ai-dan-pengembangan-perangkat-lunak-tradisional/","title":"AI dan Pengembangan Perangkat Lunak Tradisional","hideInFiletree":true,"tags":["programming","coding","gpt","research","testing","security"],"dg-note-properties":{"title":"AI dan Pengembangan Perangkat Lunak Tradisional","category":"references","tags":["programming","coding","gpt","research","testing","security"],"sources":["_raw/articles/ai-dan-pengembangan-perangkat-lunak-tradisional.md"],"created":"2026-08-26","updated":"2026-08-26","confidence":"medium"}}
---

Pengembangan perangkat lunak tradisional mengandalkan pengembang untuk menuliskan instruksi dan aturan secara eksplisit. Komputer kemudian menjalankan logika tersebut secara deterministik, langkah demi langkah. Pendekatan ini sesuai untuk masalah yang terdefinisi dengan jelas dan memiliki jumlah keluaran yang terbatas. Setelah program selesai ditulis dan diperbaiki, operasi yang sama cenderung menghasilkan keluaran yang sama. Pengembang juga dapat menelusuri alur kode untuk mencari sumber kesalahan.

Mekanisme model yang menghasilkan kode dijelaskan di [[References/Cara Kerja LLM\|Cara Kerja LLM]], sedangkan penerapannya secara khusus pada antarmuka dibahas di [[References/Generative AI for Frontend Development\|Generative AI for Frontend Development]].

Pengembangan dengan bantuan AI mengubah sebagian proses tersebut. Model yang dilatih menggunakan kumpulan data kode dapat menghasilkan cuplikan kode, menjelaskan basis kode, menyarankan perbaikan, membantu proses *debugging*, dan mengerjakan perubahan di beberapa berkas. Pengembang tidak selalu menulis setiap baris dari awal. Mereka menyampaikan tujuan, konteks, batasan, dan kriteria keberhasilan, lalu menilai hasil yang dibuat AI.

Perbedaan ini tidak berarti AI menggantikan seluruh proses rekayasa perangkat lunak. AI dapat mempercepat pembuatan kode, tetapi perangkat lunak produksi tetap memerlukan perencanaan, desain arsitektur, pengujian, pemeriksaan keamanan, penerapan, dan tata kelola. Perubahan utamanya terletak pada pembagian kerja: manusia semakin banyak menentukan maksud dan memverifikasi hasil, sedangkan AI menangani sebagian pekerjaan implementasi dan eksplorasi.

## Dari aturan eksplisit ke sistem berbasis data

Perangkat lunak tradisional mengikuti aturan yang ditentukan pengembang. Pola seperti `if`, `else`, fungsi, dan struktur data menerjemahkan kebutuhan bisnis menjadi perilaku yang dapat diprediksi. Kejelasan ini memberi pengembang kendali atas logika program dan memudahkan penelusuran kesalahan melalui kode. 

Sistem AI bekerja dengan karakter yang berbeda. Data pelatihan membantu model mempelajari pola, data validasi digunakan untuk menyesuaikan model, dan data pengujian menilai kinerjanya pada contoh yang tidak digunakan dalam pelatihan. Model pembelajaran mesin dapat membuat prediksi tanpa aturan untuk setiap kemungkinan ditulis satu per satu, sehingga lebih sesuai untuk tugas seperti pengolahan bahasa alami, gambar, dan data tidak terstruktur.

Namun, keluaran sistem AI tidak selalu setransparan program berbasis aturan. Model kompleks dapat berfungsi seperti kotak hitam: masukan dan keluaran terlihat, tetapi alasan internal yang menghasilkan keputusan sulit ditelusuri. Karena itu, pendekatan tradisional tetap lebih tepat ketika perilaku harus konsisten, dapat diaudit, dan sepenuhnya dikendalikan. AI lebih berguna ketika ruang masalah terlalu luas untuk dirumuskan sebagai kumpulan aturan tetap.

## AI sebagai asisten pengembangan

Dalam pengembangan perangkat lunak, AI paling berguna sebagai lapisan bantuan di atas praktik rekayasa yang sudah ada. IBM mendefinisikan *vibe coding* sebagai praktik memberi instruksi kepada AI untuk menghasilkan kode, bukan menulis seluruh kode secara manual. Prosesnya dimulai dengan menjelaskan maksud dan konteks, menghasilkan implementasi awal, kemudian meninjau dan menyempurnakannya secara iteratif.

Kualitas konteks menentukan kegunaan hasil. AI perlu mengetahui masalah bisnis, arsitektur sistem, konvensi repositori, kebijakan keamanan, serta persyaratan kinerja dan skalabilitas. Tanpa informasi tersebut, kode dapat benar secara teknis tetapi tidak cocok dengan sistem tempat kode itu akan digunakan.

Temuan internal Anthropic menunjukkan bahwa penggunaan AI dalam pekerjaan pengembangan paling sering diarahkan pada *debugging* dan pemahaman basis kode. Dalam survei terhadap 132 insinyur dan peneliti, 55 persen menggunakan Claude setiap hari untuk *debugging*, 42 persen untuk memahami kode, dan 37 persen untuk menerapkan fitur baru. Responden melaporkan penggunaan Claude pada sekitar 59 persen pekerjaan mereka dan memperkirakan kenaikan produktivitas rata-rata sebesar 50 persen, tetapi Anthropic menegaskan bahwa angka tersebut berasal dari pelaporan mandiri dan sulit diukur secara presisi.

AI juga memungkinkan pekerjaan yang sebelumnya tidak dianggap sepadan dengan waktu pengerjaannya. Responden Anthropic memperkirakan bahwa 27 persen pekerjaan berbantuan Claude tidak akan dikerjakan tanpa AI, termasuk dokumentasi, pengujian, alat internal kecil, dan eksperimen. Dalam data penggunaan Claude Code, 8,6 persen tugas berkaitan dengan perbaikan kecil yang biasanya ditunda, seperti refaktor untuk meningkatkan keterpeliharaan kode.

## Produktivitas bergantung pada jenis tugas

Klaim bahwa AI selalu mempercepat pengembangan perlu diperlakukan secara hati-hati. Studi acak terkontrol METR pada 16 pengembang berpengalaman mencakup 246 tugas nyata di repositori *open source* besar yang telah mereka tangani selama beberapa tahun. Dengan perangkat AI yang tersedia pada awal 2025, peserta membutuhkan waktu 19 persen lebih lama ketika penggunaan AI diizinkan, meskipun sebelum studi mereka memperkirakan penghematan waktu sebesar 24 persen.

Hasil tersebut tidak membuktikan bahwa AI memperlambat semua pengembang. METR membatasi kesimpulannya pada pengembang berpengalaman yang mengerjakan repositori besar, kompleks, dan sangat mereka kenal. Studi itu juga menyatakan bahwa hasil historis tersebut tidak lagi mencerminkan dampak model AI terbaru terhadap produktivitas pengembang *open source*.

Perbedaan hasil antara Anthropic dan METR menunjukkan bahwa produktivitas dipengaruhi oleh pemilihan tugas, tingkat penguasaan repositori, kualitas konteks, standar mutu, serta biaya untuk memeriksa keluaran. Wawancara Anthropic menemukan bahwa pengembang lebih sering mendelegasikan tugas yang mudah diverifikasi, terdefinisi dengan baik, berisiko rendah, repetitif, atau berada di luar keahlian inti mereka tetapi tidak terlalu kompleks. Lebih dari separuh responden hanya menilai 0 sampai 20 persen pekerjaan mereka dapat diserahkan sepenuhnya kepada Claude tanpa keterlibatan aktif. 

AI dapat menghemat waktu saat biaya menghasilkan solusi lebih besar daripada biaya memeriksanya. Sebaliknya, AI dapat menambah pekerjaan ketika pengembang harus membersihkan kode, memahami keputusan yang tidak mereka buat sendiri, atau memperbaiki implementasi yang tidak sesuai dengan pengetahuan implisit dalam repositori. Produktivitas karena itu lebih tepat dinilai per jenis tugas, bukan dari jumlah kode yang dihasilkan.

## Risiko kualitas, keamanan, dan pemeliharaan

Kode yang tampak masuk akal belum tentu benar. GitHub menyarankan agar kode buatan AI terlebih dahulu diperiksa melalui kompilasi, pengujian otomatis, analisis statis, pemindaian keamanan, dan pemeriksaan dependensi. Peninjau juga perlu memastikan bahwa implementasi sesuai dengan kebutuhan, arsitektur, pola desain, dan konvensi proyek.

Risiko khusus AI mencakup API yang tidak ada, logika yang salah, batasan yang diabaikan, dependensi mencurigakan, serta pengujian yang dihapus atau dilewati alih-alih diperbaiki. IBM turut mencatat risiko kata sandi atau kunci API yang ditanam langsung, injeksi SQL, API yang tidak aman, hak akses berlebihan, autentikasi yang lemah, dan pustaka pihak ketiga yang rentan.

Pemeliharaan menjadi sulit ketika tim menerima banyak kode tanpa memahami struktur dan alasan di baliknya. Anthropic menemukan kekhawatiran bahwa penggunaan AI dapat mengurangi latihan untuk menulis dan mengkritik kode secara mendalam. Sebagian karyawan juga melaporkan berkurangnya kesempatan bertanya kepada rekan kerja karena AI menjadi tempat pertama untuk mencari jawaban, yang dapat memengaruhi kolaborasi dan pendampingan pengembang junior.

## Pembagian kerja yang lebih tepat

Pengembangan tradisional unggul untuk komponen yang memerlukan perilaku deterministik, transparansi, kontrol ketat, dan audit yang jelas. Pengembangan berbantuan AI lebih sesuai untuk prototipe, kode repetitif, eksplorasi solusi, dokumentasi, pengujian awal, penjelasan kode, dan tugas yang hasilnya dapat diverifikasi dengan cepat.

Keputusan arsitektur, kebutuhan bisnis, keamanan, dan perubahan berisiko tinggi tetap memerlukan penilaian manusia. Dalam penelitian Anthropic, pengembang cenderung mempertahankan perencanaan tingkat tinggi dan keputusan desain yang membutuhkan konteks organisasi atau pertimbangan teknis yang sulit dirumuskan dalam instruksi. Batas delegasi dapat berubah seiring kemampuan model meningkat, tetapi tanggung jawab atas perangkat lunak tetap berada pada tim yang merancang, meninjau, dan mengoperasikannya.

Alur kerja yang lebih aman dapat dirumuskan sebagai berikut:

1. Tentukan masalah, kebutuhan, batasan, dan kriteria penerimaan sebelum meminta AI menulis kode.
2. Berikan konteks arsitektur, dokumentasi, konvensi repositori, serta kebijakan keamanan yang relevan.
3. Gunakan AI untuk menghasilkan rancangan awal atau mengerjakan unit kerja yang kecil dan mudah diperiksa.
4. Tinjau setiap perubahan terhadap maksud, keterbacaan, keterpeliharaan, dependensi, dan risiko keamanan.
5. Jalankan pengujian, *linting*, analisis statis, pemindaian keamanan, serta pemeriksaan integrasi sebelum perubahan digabungkan.
6. Dokumentasikan keputusan penting agar tim memahami kode dan dapat memeliharanya tanpa bergantung pada riwayat percakapan dengan AI.

AI tidak menghapus kebutuhan akan rekayasa perangkat lunak yang disiplin. Teknologi ini mengurangi biaya untuk menghasilkan alternatif dan implementasi awal, tetapi meningkatkan pentingnya perumusan masalah, penyediaan konteks, verifikasi, dan penilaian teknis. Pendekatan terbaik bukan memilih AI atau pengembangan tradisional secara mutlak, melainkan menempatkan masing-masing pada jenis pekerjaan yang sesuai dengan kekuatan dan keterbatasannya.


## Lihat juga

- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Generative AI for Frontend Development\|Generative AI for Frontend Development]]
- [[GitHub\|GitHub]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [What is Vibe Coding?](https://www.ibm.com/think/topics/vibe-coding)
- [How AI is transforming work at Anthropic](https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic)
- [AI vs Traditional Software Development](https://www.youtube.com/watch?v=P7lryCIvxgA)
- [Review AI-generated code](https://docs.github.com/en/copilot/tutorials/review-ai-generated-code)
- [Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study)
