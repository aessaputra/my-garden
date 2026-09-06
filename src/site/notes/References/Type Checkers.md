---
{"dg-publish":true,"dg-path":"Type Checkers.md","permalink":"/type-checkers/","title":"Type Checkers","hideInFiletree":true,"tags":["references","programming","testing","development","ci-cd"],"noteIcon":"","dg-note-properties":{"title":"Type Checkers","categories":["Code Quality","Developer Tools"],"tags":["references","programming","testing","development","ci-cd"],"sources":["_raw/articles/type-checkers-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Type checker adalah alat analisis yang memeriksa apakah nilai, operasi, argument, return value, dan assignment konsisten dengan tipe yang dideklarasikan atau diinferensikan. Static type checker menjalankan pemeriksaan tanpa mengeksekusi program.

[TypeScript](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html) menyebut pemeriksaan ini sebagai pendeteksian error berdasarkan jenis nilai yang dioperasikan. [Mypy](https://mypy.readthedocs.io/en/stable/getting_started.html) menerapkan prinsip serupa pada Python.

## Model pemeriksaan

Checker membangun representasi tipe dari annotation, deklarasi library, stub, generic, dan inference. Ia kemudian menilai compatibility pada assignment, function call, return value, property access, narrowing control flow, dan operasi lain.

Hasil analisis adalah diagnostic, bukan eksekusi. Program Python tetap dapat berjalan walaupun mypy melaporkan error. TypeScript juga mempertahankan perilaku runtime JavaScript dan dapat menjalankan pemeriksaan tanpa menghasilkan JavaScript melalui [`noEmit`](https://www.typescriptlang.org/tsconfig/noEmit.html).

## Annotation dan inference

Annotation menyatakan kontrak secara eksplisit. Inference menyimpulkan tipe dari initializer, return statement, callback context, kondisi, dan penggunaan lain agar setiap local variable tidak perlu diberi annotation.

[TypeScript](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) menganjurkan inference ketika konteks sudah cukup. [Mypy](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html) juga melakukan local dan contextual inference, tetapi fungsi Python tanpa annotation dapat tetap menjadi dynamically typed.

Boundary module, public API, callback, container kosong, recursive definition, dan kontrak stabil sering membutuhkan annotation. [Flow](https://flow.org/en/docs/lang/annotation-requirement/) menggunakan local inference dan meminta annotation tertentu pada boundary untuk mendukung pemeriksaan paralel.

## Contoh alat

TypeScript menyatukan bahasa, checker, dan compiler untuk JavaScript. Mypy serta [Pyright](https://microsoft.github.io/pyright/) memeriksa type hints Python tanpa mengubah aturan runtime Python. Flow menganalisis JavaScript dengan sistem tipe dan strategi inference sendiri.

Checker dapat terintegrasi dengan editor, command line, build, pre-commit, dan [[CI\|CI]]. Diagnostic editor memberi feedback cepat, sedangkan CI memastikan versi, konfigurasi, strictness, serta cakupan file diterapkan secara konsisten.

## Strictness dan gradual typing

Manfaat checker bergantung pada strictness dan coverage. `any`, `Any`, cast, ignore comment, module tanpa tipe, atau konfigurasi permisif dapat memutus aliran informasi dan menyembunyikan error.

Gradual typing memungkinkan adopsi bertahap pada codebase lama. [Panduan mypy](https://mypy.readthedocs.io/en/stable/existing_code.html) menyarankan baseline kecil yang lulus, konfigurasi bersama, pemeriksaan CI, annotation pada kode baru, lalu peningkatan strictness per module.

## Batasan

Type checker memeriksa konsistensi terhadap model tipe, bukan seluruh kebenaran program. Ia tidak membuktikan algoritme memenuhi kebutuhan, query mengembalikan data benar, jaringan tersedia, concurrency aman, atau UI dapat digunakan.

Data eksternal juga tidak menjadi tepercaya karena annotation. [Python](https://docs.python.org/3/library/typing.html) tidak menegakkan annotation function dan variable pada runtime. Input jaringan, file, environment, dan pengguna membutuhkan parsing serta runtime validation.

Type checking karena itu melengkapi [[References/Linters dan Formatters\|Linters dan Formatters]], [[References/Testing Your Apps\|Testing Your Apps]], runtime validation, code review, dan observability. Ia mengurangi kelas error tertentu sebelum eksekusi, tetapi tidak menggantikan pemeriksaan perilaku.

## Lanjutan

- [[Type checking catches contradictions before execution, not every runtime failure\|Type checking catches contradictions before execution, not every runtime failure]]
- [[Inference should remove annotation noise without hiding public contracts\|Inference should remove annotation noise without hiding public contracts]]
- [[Dynamic inputs require runtime validation despite static types\|Dynamic inputs require runtime validation despite static types]]
- [[Type checking belongs in CI but cannot replace behavioral tests\|Type checking belongs in CI but cannot replace behavioral tests]]
