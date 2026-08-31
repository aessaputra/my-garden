---
{"dg-publish":true,"dg-path":"Claude Code.md","permalink":"/claude-code/","title":"Claude Code","hideInFiletree":true,"tags":["references","ai-agents","coding","programming","workflow","security","testing"],"dg-note-properties":{"title":"Claude Code","category":"references","tags":["references","ai-agents","coding","programming","workflow","security","testing"],"sources":["_raw/articles/claude-code-expanded.md"],"created":"2026-08-26","updated":"2026-08-26","confidence":"medium"}}
---

Claude Code adalah alat pemrograman berbasis agen dari Anthropic yang dapat membaca basis kode, mengubah berkas, menjalankan perintah, dan terhubung dengan perangkat pengembangan. Fungsinya melampaui pelengkapan kode dan memperluas pola kerja yang dibahas dalam [[References/AI dan Pengembangan Perangkat Lunak Tradisional\|AI dan Pengembangan Perangkat Lunak Tradisional]]: pengguna dapat memintanya menjelaskan arsitektur proyek, menemukan sumber galat, mengimplementasikan fitur lintas berkas, menulis pengujian, dan mengelola alur kerja [[References/Git\|Git]].

Kemampuan tersebut membuat Claude Code berguna pada beberapa tahap pengembangan perangkat lunak. Pengembang dapat memakainya untuk memahami proyek yang belum dikenal, mencari bug, memperbarui dokumentasi, menerjemahkan kode antarbahasa, atau mengotomatiskan pekerjaan berulang. Claude Code tersedia melalui terminal, ekstensi IDE, aplikasi desktop, dan peramban, sehingga pola kerja yang sama tidak terbatas pada satu antarmuka.

## Cara kerja berbasis agen

Perbedaan utama antara Claude Code dan chatbot pemrograman biasa terletak pada kemampuannya untuk bertindak. Setelah menerima tujuan, Claude Code dapat menelusuri berkas yang relevan, menyusun rencana, melakukan perubahan, menjalankan pengujian, membaca hasilnya, lalu memperbaiki implementasi jika pemeriksaan gagal. Video pengenalan Anthropic memperlihatkan alur tersebut pada aplikasi Next.js: agen mempelajari repositori, menemukan berkas yang perlu diubah tanpa diberi jalur secara eksplisit, menambahkan fitur, menjalankan pengujian, memperbaiki galat kompilasi, lalu menyiapkan commit dan mendorong perubahan ke [[GitHub\|GitHub]] setelah memperoleh izin.

Model kerja ini tidak menghapus peran pengembang. Pengguna tetap menentukan tujuan, batas perubahan, kriteria penerimaan, dan izin yang boleh digunakan agen. Hasil terbaik muncul ketika tugas memiliki pemeriksaan yang dapat dijalankan, seperti rangkaian pengujian, perintah build, linter, perbandingan keluaran, atau tangkapan layar antarmuka. Tanpa pemeriksaan semacam itu, agen hanya dapat menilai bahwa pekerjaan tampak selesai, sedangkan sinyal lulus atau gagal memungkinkannya mengulang proses sampai hasil memenuhi kriteria.

## Dari instruksi singkat ke konteks proyek

Claude Code menerima instruksi dalam bahasa alami, tetapi ketepatan hasil tetap bergantung pada mutu konteks. Prompt yang baik menyebutkan perilaku yang diinginkan, lokasi atau cakupan perubahan, batasan teknis, contoh pola yang harus diikuti, dan cara memverifikasi hasil. Alih alih menulis "perbaiki bug login", pengembang dapat menjelaskan gejalanya, langkah reproduksi, bagian kode yang kemungkinan terkait, serta meminta pengujian yang gagal sebelum perbaikan diterapkan.

Konteks yang berlaku untuk seluruh proyek dapat disimpan dalam `CLAUDE.md`. Claude Code membaca berkas ini pada awal percakapan untuk memperoleh perintah build dan test, aturan gaya, keputusan arsitektur, serta kebiasaan kerja repositori. Tutorial roadmap.sh menggunakan `CLAUDE.md` untuk menjelaskan tujuan aplikasi, tumpukan teknologi, perintah yang dapat dijalankan, mode ketat TypeScript, penanganan galat, dan berkas yang tidak boleh dihapus.

`CLAUDE.md` sebaiknya ringkas. Dokumentasi Anthropic menyarankan agar berkas tersebut hanya memuat informasi yang sulit disimpulkan dari kode dan benar benar berlaku lintas sesi, sebab instruksi yang terlalu panjang dapat menenggelamkan aturan penting. Pengetahuan yang hanya diperlukan pada tugas tertentu lebih tepat ditempatkan dalam *skill*, sedangkan tindakan yang wajib selalu berjalan, misalnya lint setelah setiap penyuntingan, lebih cocok diterapkan melalui *hook*.

## Alur kerja yang lebih terkendali

Untuk perubahan yang luas atau belum jelas, pola yang disarankan adalah eksplorasi, perencanaan, implementasi, lalu verifikasi. Dalam Plan Mode, Claude Code dapat membaca berkas dan menyusun rencana tanpa mengubah kode sampai pengguna menyetujuinya. Tahap ini memberi ruang untuk memeriksa berkas yang akan disentuh, dependensi yang terlibat, risiko migrasi, dan pengujian yang perlu ditambahkan sebelum pekerjaan dimulai.

Tutorial roadmap.sh menerapkan pola tersebut untuk membangun pengelola tugas berbasis CLI dengan Node.js dan TypeScript. Proyek dipecah menjadi langkah kecil: membuat struktur dan perutean perintah, menambahkan penyimpanan JSON, mengimplementasikan operasi `add`, `list`, `complete`, dan `delete`, memperbaiki validasi input, kemudian menjalankan uji manual dan pengujian otomatis. Pemecahan semacam ini menjaga ruang lingkup setiap perubahan tetap dapat diperiksa dan memudahkan pemulihan bila agen menyimpang.

Perencanaan tidak perlu dipaksakan pada setiap pekerjaan. Anthropic menyatakan bahwa perubahan kecil dengan cakupan yang jelas, seperti memperbaiki salah ketik atau mengganti nama variabel, dapat dikerjakan langsung, sedangkan Plan Mode paling bermanfaat untuk perubahan lintas berkas, pendekatan yang belum pasti, atau basis kode yang belum dikenal.

## Mengelola kesalahan dan konteks

Pengguna dapat menghentikan proses ketika arah pekerjaan mulai keliru, memberikan koreksi yang sempit, atau kembali ke keadaan sebelumnya dengan `/rewind`. Untuk percakapan panjang, `/compact` merangkum konteks sambil mempertahankan keputusan penting, sedangkan `/clear` menghapus konteks percakapan ketika pengguna berpindah ke tugas yang tidak berkaitan. Dokumentasi Anthropic menekankan bahwa kualitas respons dapat menurun ketika ruang konteks dipenuhi pembacaan berkas, keluaran perintah, dan percobaan yang tidak lagi relevan.

Praktik yang lebih aman adalah mengoreksi penyimpangan sejak awal dan memulai sesi bersih setelah beberapa upaya perbaikan gagal. Eksplorasi besar juga dapat dipindahkan ke subagen agar hasil ringkasnya kembali ke percakapan utama tanpa memenuhi konteks dengan seluruh pembacaan berkas. Untuk pekerjaan paralel, Claude Code mendukung sesi terpisah dan Git worktree agar perubahan pada cabang berbeda tidak saling bertabrakan.

## Keamanan dan batas izin

Karena Claude Code dapat mengubah berkas dan menjalankan perintah, izin harus diperlakukan sebagai bagian dari rancangan alur kerja, bukan sebagai gangguan yang selalu dilewati. Dalam Manual mode, operasi baca di dalam direktori kerja umumnya dapat berjalan tanpa persetujuan, sedangkan perintah shell, perubahan berkas, dan akses web meminta izin sesuai aturan yang berlaku. Pengguna dapat mengatur aturan `allow`, `ask`, dan `deny`, dengan urutan evaluasi `deny`, kemudian `ask`, lalu `allow`.

Sandbox memberi lapisan tambahan dengan membatasi akses sistem berkas dan jaringan untuk perintah shell. Anthropic menyarankan penggunaan izin dan sandbox secara bersamaan: aturan izin mencegah alat mencoba operasi terlarang, sedangkan batas tingkat sistem operasi mengurangi dampak jika instruksi berbahaya lolos dari penilaian model. Mode `bypassPermissions` sebaiknya hanya digunakan dalam lingkungan terisolasi seperti container atau mesin virtual karena mode tersebut melewati sebagian besar permintaan persetujuan.

Repositori atau konten eksternal juga dapat membawa instruksi berbahaya melalui *prompt injection*. Pedoman keamanan Anthropic meminta pengguna meninjau perintah sebelum menyetujuinya, menghindari pengaliran konten yang tidak dipercaya langsung ke agen, memeriksa perubahan pada berkas penting, dan memakai mesin virtual untuk eksekusi berisiko. Server MCP pun perlu dipilih secara hati hati karena Anthropic tidak melakukan audit keamanan terhadap setiap server MCP yang dapat digunakan Claude Code.

## Penggunaan yang tepat

Claude Code paling efektif ketika pengembang mampu mendefinisikan hasil dan menyediakan cara untuk membuktikan bahwa hasil tersebut benar. Untuk eksplorasi, agen dapat menjelaskan struktur dan hubungan antarmodul. Untuk implementasi, agen dapat bekerja lintas berkas sambil mengikuti pola repositori. Untuk pemeliharaan, agen dapat menjalankan pengujian, memperbaiki lint, menyelesaikan konflik, memperbarui dependensi, dan menyiapkan catatan rilis.

Namun, keluaran yang tampak meyakinkan belum cukup untuk dikirim ke produksi. Pengembang tetap perlu membaca diff, memeriksa perintah yang akan dijalankan, menilai risiko keamanan, dan meminta bukti berupa hasil test atau build. Claude Code mempercepat pekerjaan ketika tujuan, konteks, izin, dan verifikasi dirancang dengan baik. Ia bukan pengganti penilaian teknis, melainkan agen yang memperluas kemampuan pengembang untuk memahami dan mengubah perangkat lunak secara lebih sistematis.

## Lihat juga

- [[References/AI dan Pengembangan Perangkat Lunak Tradisional\|AI dan Pengembangan Perangkat Lunak Tradisional]]: konteks penggunaan AI dalam rekayasa perangkat lunak.
- [[References/Git\|Git]]: dasar kontrol versi yang digunakan dalam alur kerja Claude Code.
- [[GitHub\|GitHub]]: pengelolaan repositori, pull request, dan kolaborasi.
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]: dasar teknis model bahasa yang mendukung agen pemrograman.

## Sumber

- [Vibe coding tutorial: Build your first app with Claude Code](https://roadmap.sh/vibe-coding/tutorial): tutorial membangun aplikasi CLI dengan Claude Code.
- [Introducing Claude Code](https://www.youtube.com/watch?v=AJpK3YTTKZ4): demonstrasi resmi alur kerja berbasis agen.
- [Claude Code overview](https://docs.anthropic.com/en/docs/claude-code/overview): kemampuan, antarmuka, dan integrasi resmi.
- [Best practices for Claude Code](https://www.anthropic.com/engineering/claude-code-best-practices): praktik pengelolaan konteks, perencanaan, dan verifikasi.
- [Claude Code security](https://docs.anthropic.com/en/docs/claude-code/security): model keamanan dan tanggung jawab pengguna.
- [Configure Claude Code permissions](https://docs.anthropic.com/en/docs/claude-code/permissions): mode serta aturan izin Claude Code.
