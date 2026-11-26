---
{"dg-publish":true,"dg-path":"Git.md","permalink":"/git/","title":"Git","hideInFiletree":true,"tags":["git","github","version-control","programming","tutorial","guide"],"noteIcon":"","dg-note-properties":{"title":"Git","category":"references","tags":["git","github","version-control","programming","tutorial","guide"],"sources":["_raw/articles/thenewstack-git-for-absolutely-everyone.md","_raw/articles/git-youtube-crash-course-transcript.md","_raw/articles/git-by-example-antonz-2026-11-26.md","_raw/articles/atlassian-what-is-version-control-2026-11-26.md"],"created":"2026-08-21","updated":"2026-11-26"}}
---


Git adalah software version control yang membuat kolaborasi proyek tetap tertib di command line.

## Apa itu Git dan version control

Git adalah program yang Anda install di komputer untuk mengelola version control. Git dibuat Linus Torvalds pada 2005, pembuat Linux yang sama yang menjalankan sebagian besar internet.

Version control bekerja dengan konsep snapshot. Anda membuat direktori proyek, Git mencatat setiap perubahan file di dalamnya. Ubah sedikit, ambil snapshot, ubah lagi, snapshot lagi, simpan kronologis. Ketika sesuatu berantakan, Anda bisa kembali ke versi terakhir yang masih berfungsi.

Git bukan satu-satunya sistem version control, tetapi yang paling banyak dipakai. Git juga menjadi syarat untuk memakai GitHub, platform hosting proyek paling populer. Git dan GitHub berbeda: Git adalah softwarenya, GitHub adalah hub tempat repo Git disimpan.

Git bersifat distributed atau decentralized. Perintah `push` mengirim perubahan Anda ke orang lain, `pull` menarik perubahan mereka ke Anda. GitHub menyimpan library master dari semua versi, sehingga tim tidak perlu menunggu satu laptop yang sedang dibawa pulang orang yang sakit.

Git bekerja dengan bahasa atau framework apa pun: situs HTML statis, aplikasi Node.js, Python, Java, C#, dan lainnya.

Artikel Atlassian merumuskan tiga manfaat yang harus diberikan VCS: riwayat perubahan jangka panjang untuk setiap file, termasuk pembuatan, penghapusan, rename, penulis, tanggal, dan catatan tujuan; branching dan merging agar alur kerja paralel tetap terpisah lalu digabung dengan verifikasi konflik; serta traceability yang menghubungkan perubahan ke tiket atau bug tracker. Ketiganya menjelaskan mengapa Git dipakai bukan hanya untuk menyimpan kode, tetapi juga untuk analisis akar masalah dan memahami desain lama.

## Cara kerja Git: tiga area

Setelah `git init`, Git menyiapkan tiga area yang dikelola otomatis:

- **Working Directory**: file proyek yang Anda lihat dan edit.
- **Index (staging area)**: tempat penampungan sementara untuk perubahan terbaru sebelum disimpan.
- **HEAD**: pointer ke commit terakhir, yaitu versi yang paling baru disimpan.

Semua perintah Git diawali `git`, formatnya `git <aksi>`, supaya terminal tahu Anda memanggil Git bukan program lain.

## Alur dasar: init sampai commit

Alur dari kedua sumber pada dasarnya sama. Langkah berikut menggabungkan keduanya.

### 1. Install dan siapkan identitas

- Download Git dari [git-scm.com](https://git-scm.com) untuk Mac, Linux, atau Windows. Di Linux bisa lewat package manager seperti `apt-get` atau `yum`.
- Di Windows, pakai Git Bash (lingkungan mirip Linux) yang terinstall bersama Git. Buka lewat Start menu atau klik kanan di folder lalu pilih Git Bash here. Alternatif terminal lain juga bisa, tetapi Git Bash disarankan untuk belajar cara kerja Git.
- Verifikasi instalasi dengan `git --version`.
- Atur identitas sekali untuk semua proyek:

```bash
git config --global user.name 'Nama Anda'
git config --global user.email 'email@domain.com'
git config --global color.ui 'auto'
```

Flag `--global` membuat Git mengingat identitas Anda selamanya. Baris terakhir opsional untuk membuat output berwarna dan lebih mudah dibaca. Atur `user.name` dan `user.email` sebelum mulai bekerja.

### 2. Buat repository

```bash
mkdir nama-proyek
cd nama-proyek
git init
```

`git init` membuat repository baru, sering disebut repo. Anggap sebagai folder proyek yang sekarang punya kemampuan Git. Di dalamnya ada file tersembunyi `.git` yang menyimpan riwayat revisi. Untuk melihatnya di Windows, aktifkan Show hidden files.

Buat file penjelas di awal:

```bash
touch README.md
```

README tidak perlu panjang, cukup fakta dasar tentang isi proyek. Tambahkan file proyek lain dengan cara yang sama.

### 3. Working copy dan clone

Untuk latihan aman, buat working copy dari repo lokal supaya Anda bisa bereksperimen tanpa konsekuensi:

```bash
git clone /path/to/repository
```

Jika repo berasal dari remote atau milik orang lain:

```bash
git clone username@host:/path/to/repository
# atau untuk GitHub:
git clone https://github.com/USERNAME/REPOSITORY.git
```

`clone` menyalin seluruh repo ke folder baru. Jika host memberi peringatan repo tampak kosong, itu karena Anda belum stage dan commit perubahan.

### 4. Cek status, stage, dan commit

```bash
git status
```

`git status` menampilkan file yang belum di-track. Di terminal berwarna, file baru muncul merah.

```bash
git add <file>
git add *.html
git add .
```

`git add` memindahkan file ke staging area. Setelah di-stage, warna berubah menjadi hijau. Untuk membatalkan stage tanpa menghapus file:

```bash
git rm --cached <file>
```

Periksa lagi dengan `git status`, lalu simpan snapshot:

```bash
git commit -m "Initial commit"
```

Tiap commit adalah snapshot yang diberi pesan dan ID heksadesimal unik, contoh `a4105ea`. Pesan menjelaskan apa yang berubah dan mengapa, sehingga Anda dan orang lain bisa paham di masa depan. Pesan pertama saat repo baru selalu `Initial commit`. Jika Anda menjalankan `git commit` tanpa `-m`, Git membuka editor (Vim) untuk menulis pesan, simpan dengan `:wq`. Tulis pesan yang berguna, referensi bagus ada di [The Art of the Commit](http://alistapart.com/article/the-art-of-the-commit). Prinsipnya: commit sering, dengan pesan yang membantu diri Anda di masa depan.

Cek kembali:

```bash
git status
# Nothing to commit, working tree clean
```

Pesan ini berarti snapshot sudah tersimpan dan working tree bersih.

### 5. Abaikan file dengan .gitignore

Buat file `.gitignore` untuk daftar file atau folder yang tidak perlu di-track:

```
*.txt
/logs/
.env
node_modules/
```

Tambahkan file itu sendiri ke staging lalu commit. Setelah itu, `git status` tidak lagi menampilkan file yang cocok pola tersebut. Lihat dokumentasi untuk pola lanjutan.

### 6. Cabang dan merge

Branch memungkinkan mengerjakan fitur terpisah tanpa mengganggu kode utama.

```bash
git branch <nama-branch>
git checkout <nama-branch>
git status  # melihat branch aktif
```

Membuat branch tidak otomatis pindah, Anda harus `checkout`. Commit di satu branch tidak memengaruhi branch lain sampai di-merge.

```bash
git checkout master
git merge <nama-branch>
```

Git akan meminta pesan commit saat merge.

### 7. Terhubung ke remote dan GitHub

Buat repository baru di GitHub. Pilih nama, deskripsi, dan visibilitas public atau private. Untuk repo baru, jangan centang Initialize with README atau license supaya tidak konflik.

Hubungkan repo lokal ke remote:

```bash
git remote              # daftar remote, awalnya kosong
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Kirim perubahan pertama:

```bash
git push -u origin master
```

Perintah ini butuh login GitHub. Jika memakai SSH dan gagal dengan `Repository not found`, ubah remote ke HTTPS dengan `git remote set-url origin https://github.com/USERNAME/REPOSITORY.git` lalu coba lagi. Untuk HTTPS, Git akan meminta username dan password atau token.

Memperbarui file README setelah push: buat file, edit dengan Markdown, lalu `git add`, `git commit`, dan `git push` lagi. Muat ulang halaman GitHub untuk melihat perubahan.

### 8. Menarik dan menggandakan pekerjaan orang lain

```bash
git pull                # tarik update terbaru dari remote
git clone <url>         # salin repo remote ke mesin lokal
```

`pull` menjaga local tetap sinkron dengan kerja tim. `clone` menarik seluruh isi repo ke folder baru. Di GitHub, tombol Clone or download memberi pilihan download zip atau URL clone.

## Operasi lanjutan

Panduan interaktif antonz.org menambahkan operasi di luar alur dasar:

- **Konsep**: working tree adalah isi proyek saat ini, staging area menampung perubahan yang disiapkan, repo lokal menyimpan kumpulan commit, dan remote adalah salinan bersama. Branch adalah versi alternatif realitas proyek, tag adalah state bernama untuk milestone, dan HEAD menunjuk commit yang sedang di-checkout.
- **Rename dan hapus**: `git mv lama baru` dan `git rm file` langsung menaruh perubahan di staging, sehingga `git diff` tidak menampilkannya; pakai `git diff --cached`. `git commit -am` hanya menyertakan file yang sudah pernah di-commit, bukan file baru.
- **Riwayat**: `git log --oneline --graph`, `git show HEAD~1`, `git grep "kata" HEAD~1`, `git log main..ohmypy`, dan `git log --pretty=format:'%h %an %s %d'`.
- **Branch work**: `git switch`, `git merge` (membuat merge commit otomatis bila tidak konflik), `git merge --no-ff`, `git rebase`, `git merge --squash` lalu commit, dan `git cherry-pick <hash>`.
- **Undo**: `git restore --staged file` melepas staging, `git restore file` membuang perubahan, `git reset --soft HEAD~` membatalkan commit tetapi menyisakan perubahan di staging, `git reset --hard HEAD~` membuang commit dan perubahan, `git revert HEAD --no-edit` membuat commit pembatalan untuk riwayat yang sudah di-push, dan `git reflog` plus `git reset --hard HEAD@{n}` mengembalikan repo ke state lama.
- **Stash**: `git stash` menyimpan perubahan ke stack, `git stash list` menampilkannya, `git stash pop` mengambil yang terakhir (LIFO), `git stash clear` mengosongkan stack.
- **Alat lanjutan**: `git worktree add` untuk checkout beberapa branch sekaligus, `git bisect` untuk mencari commit penyebab bug, `git sparse-checkout init --cone` dan `git sparse-checkout set <dir>` untuk partial checkout, `git clone --filter=blob:none` atau `--filter=tree:0` untuk partial clone, dan `git shortlog -ns` untuk jumlah commit per penulis.

## Koreksi terhadap panduan antonz.org

- **Arah rebase terbalik dari narasi.** Saat berada di `main`, `git rebase ohmypy` memindahkan commit `main` ke atas `ohmypy`, bukan memindahkan `ohmypy` ke `main` seperti yang ditulis. Arah yang dimaksud adalah `git switch ohmypy && git rebase main`. Diuji dengan git 2.43.0 lokal: main A-B dan ohmypy A-C menghasilkan grafik A-C-B' setelah `git rebase ohmypy` dijalankan dari main.
- **Arah squash juga terbalik.** Saat berada di `ohmypy`, `git merge --squash main` memasukkan perubahan `main` ke working tree `ohmypy`. Untuk memadatkan commit `ohmypy` sebelum masuk main, jalankan `git switch main && git merge --squash ohmypy`. Diuji lokal: file yang muncul di staging berasal dari main.
- **Push dari worktree tidak mengirim commit hotfix.** `git worktree add -b hotfix` membuat branch `hotfix`, jadi `git push --set-upstream origin main` dari worktree itu mengirim `main` lokal yang tidak berubah dan melaporkan "Everything up-to-date"; commit `hotfix` tetap lokal. Push `hotfix`, atau `git push origin hotfix:main`. Diuji lokal: remote `main` tetap di commit lama.
- **Amend setelah push bukan batas teknis.** `git commit --amend` tetap bekerja pada repo lokal; yang berubah adalah konsekuensinya, karena menulis ulang riwayat yang sudah di-push butuh force-push dan koordinasi dengan kolaborator. Peringatan artikel benar sebagai praktik aman, bukan larangan dari Git.
- **Versi perintah.** `git switch` dan `git restore` baru ada sejak Git 2.23; mesin ini memakai git 2.43.0. Contoh output artikel berasal dari sandbox bertanggal 14 Maret 2024, sehingga hash dan tanggalnya bukan state repo nyata.

## Koreksi terhadap artikel Atlassian

- **RCS bukan sinonim VCS.** Artikel menyebut VCS "sometimes known as SCM ... or RCS (Revision Control System)". RCS adalah tool spesifik (GNU RCS) yang mengelola revisi per file dengan model checkout dan lock, bukan nama lain untuk kategori VCS; SCM juga lebih luas karena mencakup alur kerja dan manajemen konfigurasi. Dicek dengan `apt-cache show rcs`: paket Ubuntu "The GNU Revision Control System" versi 5.10.1, deskripsi "manages multiple revisions of files".
- **File locking bukan otomatis buruk.** Artikel menilai mekanisme lock lebih buruk daripada merge. Locking masih dipakai untuk file biner atau aset yang tidak bisa di-merge, misalnya SVN lock atau Perforce; itu trade-off, bukan kelemahan universal. Untuk source code teks, merge memang lebih skalabel.
- **"Special kind of database" adalah penyederhanaan.** Git menyimpan snapshot commit di object database content-addressed (`.git/objects`), bukan log perubahan per baris. Perbedaan ini penting saat membandingkan Git dengan VCS yang menyimpan delta.
- **Konteks vendor.** Artikel berasal dari Atlassian dan mengarahkan pembaca ke Bitbucket dan Jira. Definisi dan manfaatnya netral, tetapi tautan produk di akhir adalah promosi, bukan perbandingan independen.

## Praktik yang disarankan

- Gunakan command line untuk belajar cara kerja Git, jangan hanya mengandalkan GUI.
- Selalu atur `user.name` dan `user.email` sebelum commit pertama.
- Buat `.gitignore` di awal, terutama untuk file sementara, log, dan dependency.
- Commit sering dengan pesan yang jelas. Pesan yang baik menjelaskan perubahan dan alasannya.
- Untuk pemula, fokus pada `add`, `commit`, `status`, `push`, `pull`, dan `clone` dulu. Branch bisa menyusul setelah alur dasar lancar.

## Tautan dan kelanjutan

- Versi update crash course Traversy Media (2025): [https://www.youtube.com/watch?v=vA5TTz6BXhY](https://www.youtube.com/watch?v=vA5TTz6BXhY)
- Penjelasan command line untuk yang belum familiar: [Command Line Crash Course](http://www.computervillage.org/articles/CommandLine.pdf)
- Tutorial lanjutan GitHub akan dibahas terpisah di seri The New Stack berikutnya.

## Lihat juga

- [[GitHub\|GitHub]]: platform hosting untuk repo Git, alur Hello World dan pull request
- [[References/HTTP\|HTTP]]: protokol yang dipakai saat push dan pull ke remote
- [[References/How Does Internet Work\|How Does Internet Work]]: fondasi jaringan tempat Git remote bekerja
- [[References/DNS\|DNS]] dan [[References/Domain Name\|Domain Name]]: cara nama domain GitHub diterjemahkan ke alamat server

## Sumber

- [Git by example: Interactive guide](https://antonz.org/git-by-example/), diakses 26 November 2026.
- [What is version control?](https://www.atlassian.com/git/tutorials/what-is-version-control), diakses 26 November 2026.
- `_raw/articles/thenewstack-git-for-absolutely-everyone.md`
- `_raw/articles/git-youtube-crash-course-transcript.md`
- Verifikasi perilaku: git 2.43.0 lokal, probe arah `rebase`, arah `merge --squash`, dan push dari worktree; `apt-cache show rcs` untuk klaim RCS.
