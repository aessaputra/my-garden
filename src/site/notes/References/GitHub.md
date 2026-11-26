---
{"dg-publish":true,"dg-path":"GitHub.md","permalink":"/git-hub/","title":"GitHub","hideInFiletree":true,"tags":["git","github","version-control","programming","tutorial","guide"],"noteIcon":"","dg-note-properties":{"title":"GitHub","category":"references","tags":["git","github","version-control","programming","tutorial","guide"],"sources":["_raw/articles/github-hello-world.md","_raw/articles/github-skills-quickstart.md","_raw/articles/github-skills-content-model.md","_raw/articles/github-how-github-works-youtube.md","_raw/articles/github-home-2026-11-26.md","_raw/articles/github-platform-2026-11-26.md","_raw/articles/github-actions-features-2026-11-26.md","_raw/articles/github-copilot-features-2026-11-26.md","_raw/articles/github-codespaces-features-2026-11-26.md","_raw/articles/ooloo-github-flow-course-2026-11-26.md","_raw/articles/ooloo-git-workflows-2026-11-26.md","_raw/articles/ooloo-the-flow-2026-11-26.md","_raw/articles/ooloo-when-complicated-2026-11-26.md","_raw/articles/ooloo-merge-vs-squash-2026-11-26.md"],"created":"2026-08-21","updated":"2026-11-26"}}
---

GitHub adalah layanan hosting untuk repository Git dan tempat orang membangun software bersama.
## Apa itu GitHub

GitHub menyediakan tempat menyimpan repo, membahas pekerjaan lewat issue, mengisolasi eksperimen lewat branch, dan menggabungkan kontribusi lewat pull request. Platform ini juga menjalankan komunitas developer besar dan menyimpan riwayat lengkap setiap perubahan.

Untuk Hello World Anda tidak perlu install Git atau pakai command line. Cukup akun GitHub, semua langkah bisa lewat web.

## Hello World: alur 5 langkah

Dokumen Hello World mengajarkan konsep repo, branch, commit, dan pull request dalam satu latihan.

### Langkah 1: Buat repository

Repo seperti folder proyek yang berisi file, gambar, video, atau folder lain. Biasanya ada `README.md` berisi informasi proyek dalam Markdown, bahasa formatting yang mudah dibaca dan ditulis.

Via web:

1. Di sudut kanan atas halaman mana pun, buka menu lalu klik New repository.
2. Isi Repository name `hello-world` dan Description singkat seperti "This repository is for practicing the GitHub Flow".
3. Pilih Public atau Private.
4. Centang Add a README file.
5. Klik Create repository.

Repo `hello-world` ini bisa jadi tempat menyimpan ide, resource, atau diskusi. Opsi license ada, tetapi tidak wajib untuk latihan.

### Langkah 2: Buat branch

Secara default repo punya branch `main` sebagai versi definitif. Branch lain adalah snapshot `main` saat dibuat. Perubahan di branch tidak muncul di `main` sampai di-merge, sehingga aman untuk eksperimen. Jika `main` berubah saat Anda bekerja di branch, Anda bisa pull update tersebut.

Cara membuat `readme-edits`:

1. Buka tab Code di repo `hello-world`.
2. Klik dropdown yang bertuliskan main di atas daftar file.
3. Ketik `readme-edits`.
4. Klik Create branch: readme-edits from main.

Sekarang ada `main` dan `readme-edits` dengan isi yang sama pada awalnya. Diagram di docs menunjukkan `feature` yang bercabang dari `main`, melalui Commit changes, Submit pull request, dan Discuss proposed changes, lalu merge kembali.

### Langkah 3: Buat dan commit perubahan

Commit adalah perubahan yang disimpan dengan pesan yang menjelaskan alasan. Pesan ini membantu orang lain memahami apa dan mengapa.

1. Di branch `readme-edits`, klik file `README.md`.
2. Klik ikon pensil untuk edit.
3. Tulis sedikit tentang diri Anda.
4. Klik Commit changes, tulis commit message, lalu Commit changes lagi.

Perubahan hanya ada di `readme-edits`, belum di `main`.

### Langkah 4: Buka pull request

Pull request adalah inti kolaborasi di GitHub. Anda mengusulkan perubahan, meminta review, dan menampilkan diff dengan warna berbeda untuk tambahan dan penghapusan. PR bisa dibuka segera setelah commit, bahkan sebelum kode selesai.

1. Klik tab Pull requests.
2. Klik New pull request.
3. Di Example Comparisons, pilih `readme-edits` untuk dibandingkan dengan `main`.
4. Periksa diff di halaman Compare.
5. Klik Create pull request, beri judul dan deskripsi singkat. Anda bisa pakai emoji dan drag and drop gambar atau gif.
6. Klik Create pull request lagi.

Saat berkolaborasi dengan orang lain, di titik ini Anda meminta review. Kolaborator bisa komentar atau mengusulkan perubahan sebelum merge.

### Langkah 5: Merge pull request

Merge menggabungkan `readme-edits` ke `main`. Jika ada konflik dengan kode di `main`, GitHub akan mencegah merge sampai konflik diselesaikan lewat commit perbaikan atau diskusi di PR. Pada walkthrough ini tidak ada konflik.

1. Di bawah pull request, klik Merge pull request.
2. Klik Confirm merge. Anda mendapat pesan berhasil dan request tertutup.
3. Klik Delete branch. Setelah merge, perubahan sudah di `main`, branch bisa dihapus dengan aman. Untuk perubahan berikutnya buat branch baru dan ulangi proses.
4. Kembali ke tab Code untuk melihat perubahan di `main`.

Setelah selesai, cek contribution graph di profil. Untuk latihan lagi, coba kursus GitHub Skills "Introduction to GitHub". Langkah selanjutnya adalah personalisasi profil dan Markdown, dibahas di Managing your profile README.

## Cerita How GitHub works: issue ke merge

Video 3 menit dari channel GitHub memakai cerita traktor Harvester L700 untuk menjelaskan alur yang sama.

Eddie, yang bekerja di lapangan dengan traktor, punya ide berbagi data diagnostik sensor untuk meningkatkan panen. Ia membuka issue di GitHub. Issue adalah thread diskusi tempat orang melaporkan bug, meminta fitur, atau bertanya. Sam melihat issue itu dan menugaskan Vijay.

Vijay butuh tempat eksperimen yang tidak langsung sampai ke ladang, jadi ia membuat branch sebagai alternate timeline. Traktor lain di seluruh dunia tetap berjalan mengumpulkan moisture data tanpa terganggu.

GitHub melacak perubahan Vijay dan menyimpan snapshot progres. Ketika siap, Vijay membuka pull request untuk menunjukkan perubahan kepada tim. Rekan bisa membantu mengatasi hambatan dan menambah perbaikan langsung ke branch yang sama, seperti Melinda yang menambahkan commit. Semua kontribusi dan feedback tercatat di pull request, termasuk pesan dari sistem lain.

Setelah tim menyetujui, Vijay melakukan merge. Fitur baru langsung tersedia untuk semua petani. Video menutup dengan pesan bahwa GitHub adalah komunitas developer besar dan ide seperti milik Eddie bisa dipakai jauh di luar dugaan, entah oleh engineer, code enthusiast, atau petani.

## GitHub Flow dan Git Flow

GitHub Flow adalah alur ringan: branch, commit, push, pull request, review, merge ke `main`, lalu deploy. Kursus ooloo melewatkan deploy karena caranya berbeda tiap tim; dokumentasi GitHub memasukkannya. Alur ini setara dengan Merge Request di GitLab/Bitbucket. Git Flow lebih berat (`develop`, `release`, `hotfix`) dan cocok untuk software berversio, bukan web app yang deploy terus-menerus; penulisnya sendiri menambahkan catatan bahwa model itu kurang cocok untuk continuous delivery.

Di tim, beberapa branch dan PR hidup bersamaan; review bisa tertunda berhari-hari dan PR bisa saling bergantung. Beban utamanya menjaga branch tetap sinkron dengan `main`. Merge biasa mempertahankan commit per branch; squash merge menggabungkan satu PR menjadi satu commit di `main`, sehingga history linear tetapi granularitas commit hilang. Metode default mengikuti pengaturan repository, bukan selalu merge biasa. Konsep branch dan merge ada di [[References/Git\|Git]]; alur agen yang membuka PR dibahas di [[References/GitHub Copilot\|GitHub Copilot]].

## GitHub Skills dan Learn

Setelah Hello World, langkah yang disarankan adalah kursus interaktif di [skills.github.com](https://skills.github.com/), sekarang bernama GitHub Learn. Skills memakai repository template dan workflow Actions yang otomatis memajukan langkah saat Anda push atau trigger event.

Konten Skills mengikuti model yang sama: header berisi judul, gambar, dan deskripsi singkat yang menjawab mengapa kursus ini diambil. Start step menjelaskan tujuan, siapa target, apa yang dipelajari dan dibangun, prereq, serta durasi dan cara memulai. Workflow steps berjumlah 3 sampai 5, format konsisten: akui penyelesaian langkah sebelumnya dengan emphasis, jelaskan konsep dengan link ke docs, beri aktivitas `### :keyboard: Activity:` dan ordered list tugas, serta catat butuh sekitar 20 detik dan refresh untuk maju. Finish step merayakan penyelesaian, merekap apa yang dibuat, memberi next steps, dan gambar perayaan. Footer selalu ada, berisi bantuan, status page, license, dan Code of Conduct.

File di `.github/steps/` menyimpan konten per step, dan `.github/workflows/` menyimpan workflow bernama `N-brief-summary.yml`. Setiap workflow berisi header komentar, trigger event seperti `on: push branches: [main]` dan `workflow_dispatch`, job dengan `runs-on: ubuntu-latest` dan kondisi `if: !github.event.repository.is_template`, serta steps `actions/checkout` dan `skills/action-update-step` yang memindahkan `from_step` ke `to_step`.

Penulis kursus sebaiknya kenal Markdown, YAML, dan GitHub Actions, ditambah GitHub CLI bila perlu. Perencanaan kursus: tulis learning goal, outline 3 sampai 5 step yang bisa selesai 30 sampai 45 menit, dan pertimbangkan bahwa learner butuh waktu sekitar empat kali expert. Jika kursus panjang, pecah menjadi beberapa kursus. Selama setup, gunakan template, centang Template repository karena Actions mati di fork, tambah social image 1280x640, aktifkan auto-delete head branches, tambah LICENSE dan .gitignore, dan masukkan topik `skills-course`.

Bagian testing dan best practices menekankan komentar yang banyak, simpan semua di satu repo, dan jaga format tetap konsisten.

## Produk dan permukaan platform (November 2026)

Beranda github.com adalah portal produk, bukan dokumentasi. Klaim di bagian ini diambil dari halaman anak yang ditautkan beranda; angka komunitas adalah snapshot vendor.

### Platform

GitHub menyatukan repository, Issues dan Projects, pull request dan review, Actions, serta keamanan kode. Agen bisa dihubungkan lewat GitHub MCP server, API, atau CLI; issue bisa ditugaskan ke Copilot, dan Copilot bisa diminta menangani komentar atau check yang gagal. Dependabot memperbarui dependency rentan, sedangkan Copilot Autofix menangani alert code scanning. Sisi agen dibahas di [[References/GitHub Copilot\|GitHub Copilot]], dan model Git di belakang repository di [[References/Git\|Git]].

### Copilot

Copilot diposisikan sebagai AI coding agent, bukan hanya autocomplete: satu sesi bisa membawa konteks issue, diskusi, dan repository, lalu berujung di pull request. Permukaannya mencakup VS Code, Visual Studio, JetBrains, Xcode, Vim, Neovim, Zed, dan Eclipse; ditambah terminal/CLI, aplikasi desktop, web, mobile, Slack, dan Teams.

Plan yang tersedia: Free, Pro, Pro+, Max, Business, dan Enterprise. Chat dan sesi agen memakai AI Credits (1 credit = USD 0,01), sedangkan completion dan next edit tetap unlimited di plan berbayar. Free dibatasi 2.000 completion per bulan dan model entry-level. Business dan Enterprise mengatur batas pemakaian; jika tambahan berbayar tidak diizinkan, Copilot berhenti sampai siklus berikutnya.

Filter kode publik bersifat opsional dan menahan saran yang cocok dengan kode publik sepanjang 65 lexeme atau lebih (sekitar 150 karakter). Halaman Copilot sendiri menyatakan saran bisa mengandung pola tidak aman dan kualitas prompt non-Inggris lebih rendah; ini alat bantu, bukan pengganti developer.

### Actions

Actions menjalankan CI/CD dari event repository. Runner hosted mencakup Linux, macOS, Windows, ARM, GPU, dan container; matrix build menguji beberapa versi sekaligus; secret terenkripsi disimpan di level repository, environment, atau organisasi; multi-container test bisa memakai docker-compose. Marketplace menyediakan action siap pakai, dan self-hosted runner tersedia untuk kebutuhan khusus. Actions gratis untuk repository publik, tetapi menit runner dan batas plan tetap berlaku.

### Codespaces

Codespaces adalah lingkungan pengembangan cloud dari 2 sampai 32 core, dikonfigurasi lewat dev container dan dibuka dari browser, VS Code, atau IDE JetBrains. Codespaces tidak bisa self-host. Akun personal mendapat kuota gratis bulanan (120 core-hours atau 60 jam pada 2 core, plus 15 GB storage); organisasi harus mengaktifkan dan membayar sendiri. Untuk hosting statis, [[References/GitHub Pages\|GitHub Pages]] adalah jalur yang berbeda.

### Angka komunitas

Halaman platform menyebut 220 juta developer, 800 juta repository publik, dan 1,9 juta kontribusi open source dalam setahun. Beranda menampilkan angka developer yang lebih tinggi (sekitar 225 juta) dan 230 repository baru per menit. Angka-angka ini tidak konsisten antar halaman dan tidak diaudit, jadi perlakukan sebagai snapshot pemasaran.

## Koreksi dan catatan sumber

1. **Beranda bukan sumber teknis.** github.com adalah portal produk; isi yang diverifikasi diambil dari `/platform`, `/features/actions`, `/features/copilot`, dan `/features/codespaces`.
2. **Angka komunitas tidak konsisten.** 220 juta versus sekitar 225 juta developer, dan metrik "1,9 juta kontribusi" tanpa definisi. Jangan dikutip sebagai statistik pasti.
3. **FAQ Codespaces usang.** Halaman masih menyebut JetBrains/JupyterLab "in beta" dan akses gratis "through Feb 2023". Keduanya sudah lewat; status sekarang harus dicek di dokumentasi, bukan FAQ pemasaran.
4. **"Gratis untuk repository publik" bukan tanpa batas.** Actions tetap tunduk pada menit runner dan ketentuan plan; halaman yang sama menautkan tabel billing.
5. **"Dilatih pada semua bahasa" bukan jaminan kualitas.** Copilot menyatakan prompt non-Inggris bisa lebih buruk, dan saran kode bisa mengandung pola tidak aman.
6. **Filter kode publik bersifat opsional.** Filter 65 lexeme menahan kemiripan, tetapi tidak menghapus risiko lisensi; keputusan memakai saran tetap milik pengguna.
7. **Posisi "AI coding agent" adalah perubahan framing.** Halaman lama menekankan autocomplete; halaman 2026 menekankan sesi agen lintas repository. Plan dan harga cepat berubah, jadi catat tanggal akses.

## Koreksi terhadap kursus ooloo

1. **"GitHub mengganti master dengan main" dan "default-nya merge".** `main` hanya default repository baru sejak 2020; repo lama dan host lain bisa tetap `master`. Metode merge default juga pengaturan repository (merge commit, squash, atau rebase), bukan selalu merge biasa.
2. **Commit "disalin" ke main.** Objek commit-nya sama: fast-forward memindahkan pointer, merge commit menyatukan history. Tidak ada duplikasi commit.
3. **`git merge --squash` lokal tidak langsung membuat commit.** Perintah ini men-stage perubahan; commit dibuat terpisah. Probe git 2.43.0: setelah `git merge --squash feature`, `git status` menunjukkan `M a.txt` dan `git log` belum bertambah. GitHub "Squash and merge" yang membuat satu commit.
4. **`git checkout` dan "enam langkah".** `git switch` (Git 2.23+) lebih jelas untuk pindah branch; alur resmi GitHub mencakup deploy dan CI, yang dilewatkan kursus. `main` yang selalu bisa dirilis butuh test/check, bukan hanya merge.

## Praktik yang disarankan

- Buka issue untuk mulai diskusi, bukan langsung ubah `main`.
- Buat branch untuk eksperimen, tulis commit dengan pesan yang menjelaskan alasan.
- Buka pull request segera setelah commit pertama untuk mendapat feedback awal.
- Gunakan diff untuk memeriksa perubahan sebelum merge, dan selesaikan konflik dengan commit baru atau diskusi di PR.
- Jika ingin praktik terstruktur lagi, coba GitHub Skills "Introduction to GitHub" yang mengotomasi perpindahan langkah.
- Setelah Hello World, lanjut ke personalisasi profil dan Markdown.

## Tautan dan kelanjutan

- Hello World Docs: [https://docs.github.com/en/get-started/quickstart/hello-world](https://docs.github.com/en/get-started/quickstart/hello-world)
- GitHub Skills: [https://skills.github.com/](https://skills.github.com/), quickstart [https://skills.github.com/quickstart](https://skills.github.com/quickstart), content model [https://skills.github.com/content-model](https://skills.github.com/content-model)
- Video How GitHub works: [https://www.youtube.com/watch?v=w3jLJU7DT5E](https://www.youtube.com/watch?v=w3jLJU7DT5E)

## Lihat juga

- [[References/Git\|Git]]: software version control yang dipakai GitHub, konsep snapshot dan command line
- [[References/HTTP\|HTTP]]: protokol saat push dan pull ke remote
- [[References/How Does Internet Work\|How Does Internet Work]]: fondasi jaringan
- [[References/DNS\|DNS]] dan [[References/Domain Name\|Domain Name]]: penerjemahan nama domain GitHub ke alamat server
