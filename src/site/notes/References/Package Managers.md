---
{"dg-publish":true,"dg-path":"Package Managers.md","permalink":"/package-managers/","title":"Package Managers","hideInFiletree":true,"tags":["javascript","programming","npm","nodejs","package-manager","development"],"dg-note-properties":{"title":"Package Managers","category":"references","tags":["javascript","programming","npm","nodejs","package-manager","development"],"sources":["https://en.wikipedia.org/wiki/Package_manager","https://docs.npmjs.com/","https://yarnpkg.com/advanced/architecture","https://deepwiki.com/pnpm/pnpm/2-architecture","https://flox.dev/blog/package-managers-and-package-management-a-guide-for-the-perplexed"],"created":"2026-08-23","updated":"2026-08-23"}}
---

Package manager mengotomasi instalasi, pembaruan, penghapusan, dan pengelolaan dependensi perangkat lunak. Contoh yang umum dipakai adalah [[References/npm\|npm]] untuk JavaScript, pip untuk Python, yarn, pnpm, Cargo untuk Rust, dan apt untuk sistem operasi. Dengan menyelesaikan dependensi transitif, mengunci versi lewat lockfile, dan memverifikasi integritas artefak, package manager memungkinkan berbagi kode dan setup proyek yang konsisten antar mesin. Perannya menjadi fondasi workflow pengembangan modern yang kolaboratif.

## Apa yang diotomasi

Tanpa package manager, pustaka ditambahkan manual: unduh file, letakkan di folder, lalu atur urutan tag `<script>` di HTML. Cara ini rapuh saat ada pembaruan dan mudah menimbulkan konflik versi. Package manager menggantinya dengan deklarasi dependensi di manifest, resolusi otomatis, dan instalasi yang dapat direproduksi di setiap mesin.

## Komponen utama

Arsitektur umumnya terdiri dari beberapa lapisan yang saling terkait. Jumlah dan penamaannya bervariasi menurut implementasi, tetapi pola berikut hampir selalu ada.

### 1. Manifest

File deklarasi dependensi langsung seperti `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, atau `Gemfile`. File ini berisi rentang versi (`^18.2.0`, `>=1.0 <2.0`), bukan versi konkret. Developer mengelola file ini sebagai input utama resolver.

### 2. Registry

Server HTTP yang menyimpan metadata dan artefak, misalnya npm registry, PyPI, crates.io, Maven Central, dan RubyGems. Di npm, metadata ini disebut packument dan berisi daftar versi, dist-tag seperti `latest`, daftar dependensi, URL tarball, serta hash integritas.

### 3. Resolver

Mesin yang menyelesaikan constraint versi. Resolver menelusuri dependency graph secara breadth-first, mengumpulkan semua constraint untuk satu nama paket, lalu memilih versi yang memenuhi semuanya. Jika terjadi konflik, misalnya paket A butuh `lodash@^3` sementara B butuh `lodash@^4`, resolver akan memasang dua salinan terpisah atau melaporkan error. Strateginya berbeda-beda, dari lowest applicable version dan direct-dependency-wins hingga resolusi yang sadar semver seperti di Cargo dan NuGet.

### 4. Fetcher dan cache

Lapisan ini mengunduh tarball dan memverifikasi hash. Cache yang agresif mempercepat instalasi berikutnya (`~/.npm/_cacache`, `~/.pnpm-store`). pnpm memakai Content-Addressable File System (CAFS): file diindeks dengan hash SHA-512 dan dibagikan antar proyek lewat hard link atau reflink, sehingga hemat disk dan waktu.

### 5. Linker

Lapisan yang menempatkan file agar runtime dapat menemukannya. Ada dua pendekatan. Pendekatan berorientasi database (apt/dpkg, pip) mengizinkan paket menaruh file di lokasi mana saja dan mencatat pemetaannya di database lokal. Pendekatan berorientasi filesystem (RubyGems, Nix, pnpm strict) menempatkan tiap versi di subtree terisolasi (`node_modules/.pnpm`) dan hanya mengekspos symlink di tingkat atas, sehingga mencegah phantom dependency.

### 6. Lockfile

Output resolver yang dibekukan: `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `Cargo.lock`, `Gemfile.lock`, `poetry.lock`, `go.sum`. File ini mencatat versi konkret, URL `resolved`, hash `integrity` (SHA-512), dan dependensi transitif. Fungsinya adalah melewati resolusi ulang, memverifikasi integritas, dan menjaga build tetap reproduktif. Untuk aplikasi, commit lockfile ke repositori dan jangan edit manual. Di CI, gunakan `npm ci`, `pnpm install --frozen-lockfile`, atau `cargo --locked`.

### 7. Database lokal dan konfigurasi

Database paket terinstal (misalnya status dpkg, rpmdb) dan sistem konfigurasi berlapis dengan urutan prioritas: argumen CLI, environment variable, `.npmrc`, lalu `pnpm-workspace.yaml`.

### 8. CLI

Antarmuka perintah seperti `install`, `update`, `remove`, dan `audit`. Pada Yarn Berry, inti `@yarnpkg/core` dipisah dari plugin resolver, fetcher, dan linker.

## Jenis package manager

| Jenis | Cakupan | Contoh |
|---|---|---|
| **OS-level** | System-wide, binary, layanan | `apt` (Debian/Ubuntu), `dnf` (Fedora), `pacman` (Arch), `apk` (Alpine), `brew` (macOS), `winget` (Windows) |
| **Language-level** | Satu ekosistem bahasa | `npm`/`yarn`/`pnpm`/`bun` (JS), `pip`/`poetry`/`uv` (Python), `cargo` (Rust), `go mod` (Go), `maven`/`gradle` (Java), `nuget` (C#), `bundler` (Ruby) |
| **Universal** | Multi-bahasa, environment | `conda`, `nix`/`guix` (fungsional, reproduktif), `snap`/`flatpak` (runtime terisolasi) |
| **Application-level** | App store, tanpa resolusi dependensi | App Store, Google Play |

Kombinasi yang umum adalah `apt` untuk compiler dan library C, `uv` untuk Python, dan `nvm`+`npm` untuk Node. `nix` atau `flox` dapat menggantikan keduanya dengan lingkungan deklaratif lintas bahasa.

## Alur kerja

Urutan umumnya adalah: parse manifest, query registry (packument), resolver membangun dependency graph, tulis lockfile, fetch dan verifikasi hash, link ke proyek, lalu jalankan lifecycle hook. Instalasi berikutnya cukup membaca lockfile tanpa resolusi ulang.

## Fitur inti

Fitur yang biasanya tersedia meliputi resolusi dependensi, rentang versi dan semver, peer dependency, enforcement lockfile, verifikasi integritas, strategi hoisting versus isolasi strict, workspaces untuk monorepo, audit keamanan, dan script hook.

## Kapan memakai apa

Pakai package manager level OS untuk sistem, compiler, dan library. Pakai yang level bahasa untuk dependensi proyek. Untuk reproduktibilitas lintas mesin CI dan produksi, andalkan lockfile dan perkakas yang terintegrasi seperti `uv`, `poetry`, `volta`, atau `bun`/`deno`, atau pendekatan deklaratif seperti `nix`. Hindari klaim file ganda antara dua manager di path yang sama.

## Lihat juga

- [[References/npm\|npm]]: manajer paket JavaScript, `package.json` dan `package-lock.json`
- [[References/JavaScript\|JavaScript]]: bahasa yang dikelola npm, termasuk modul dan bundling
- [[References/Git\|Git]]: version control yang biasanya dipakai bersama manifest dependensi
- [[GitHub\|GitHub]]: hosting repositori yang menjalankan `npm install` di CI/CD
- [[Node.js\|Node.js]]: runtime yang membawa npm secara bawaan
- [[References/Web Browser\|Web Browser]]: runtime yang mengonsumsi bundle hasil package manager

## Sumber

- Wikipedia: [Package manager](https://en.wikipedia.org/wiki/Package_manager)
- [Dokumentasi npm](https://docs.npmjs.com/)
- [Yarn Architecture](https://yarnpkg.com/advanced/architecture)
- [pnpm Architecture (DeepWiki)](https://deepwiki.com/pnpm/pnpm/2-architecture)
- Flox: [Package Managers and Package Management Guide](https://flox.dev/blog/package-managers-and-package-management-a-guide-for-the-perplexed)
- [Cargo Resolver Reference](https://doc.rust-lang.org/cargo/reference/resolver.html)
- ArXiv: [The Design Space of Lockfiles Across Package Managers (2505.04834)](https://arxiv.org/abs/2505.04834)
