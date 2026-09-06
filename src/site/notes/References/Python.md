---
{"dg-publish":true,"dg-path":"Python.md","permalink":"/python/","title":"Python","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"Python","categories":["Programming Languages"],"tags":["references","programming"],"sources":["_raw/articles/python-research-packet.md"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Python adalah bahasa pemrograman high-level dan general-purpose dengan dynamic typing. Python mendukung pendekatan object-oriented, procedural, dan functional, serta memiliki sintaks yang menekankan keterbacaan. [FAQ resmi](https://docs.python.org/3/faq/general.html) menjelaskan kemampuan ini; istilah sederhana bukan jaminan bahwa setiap program mudah dipelihara.

## Bahasa, interpreter, dan modules

Python menyediakan modules, classes, exceptions, dan struktur data tingkat tinggi. Standard library mencakup pengolahan teks, protokol internet, testing, logging, profiling, filesystem, dan sockets. Kemampuan tersebut mendukung scripting, otomasi, dan integrasi sistem tanpa membatasi Python pada satu bidang. [FAQ Python](https://docs.python.org/3/faq/general.html#what-is-python-good-for) membedakan standard library dari third-party extensions.

Sebutan interpreted tidak berarti tanpa kompilasi. CPython dapat menyimpan compiled modules sebagai `.pyc` dalam `__pycache__`. [Dokumentasi modules](https://docs.python.org/3/tutorial/modules.html#compiled-python-files) menyatakan bahwa cache mempercepat loading, bukan kecepatan eksekusi program yang sama. Konsekuensinya dibahas dalam [[Python bytecode caches reduce loading work rather than execution work\|Python bytecode caches reduce loading work rather than execution work]].

## Ekosistem dan penggunaan

[Python.org](https://www.python.org/) mencantumkan contoh ekosistem berikut:

| Bidang | Contoh |
|---|---|
| Web development | Django dan Flask |
| Scientific computing dan analisis data | pandas dan SciPy |
| AI dan machine learning | TensorFlow dan PyTorch |
| Administrasi sistem | Ansible dan Salt |

Paket tersebut bukan fitur bawaan bahasa. Untuk array numerik, [NumPy](https://numpy.org/doc/stable/user/whatisnumpy.html) menyediakan `ndarray` multidimensi dan operasi seperti linear algebra, statistik dasar, serta transformasi array. Banyak operasi berjalan di compiled code. [[NumPy vectorization moves loops into compiled operations\|NumPy vectorization moves loops into compiled operations]] menjelaskan mekanismenya tanpa menganggap semua kode Python otomatis cepat.

Dalam web development, Python beserta framework-nya merupakan pilihan implementasi. Tanggung jawab kontrak layanan dan pengelolaan data tetap dibahas pada [[References/Backend Development\|Backend Development]], bukan ditentukan oleh nama bahasa.