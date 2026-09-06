---
{"dg-publish":true,"dg-path":"Java.md","permalink":"/java/","title":"Java","hideInFiletree":true,"tags":["references","programming","backend","security"],"noteIcon":"","dg-note-properties":{"title":"Java","categories":["Programming Languages"],"tags":["references","programming","backend","security"],"sources":["_raw/articles/java-research-packet.md"],"created":"2026-09-07","updated":"2026-09-07","confidence":"high"}}
---

Java adalah bahasa general-purpose, concurrent, class-based, dan object-oriented menurut [spesifikasi bahasa](https://docs.oracle.com/javase/specs/jls/se25/html/jls-1.html).

Sintaksnya terkait C dan C++, tetapi menghilangkan aspek tidak aman dan menambah gagasan dari bahasa lain.

## Pengetikan statis dan bytecode

Java memakai static typing yang kuat. Error compile-time dibedakan dari kegagalan runtime secara eksplisit.

Compile umumnya menerjemahkan program ke bytecode mesin-independen. Aktivitas runtime mencakup loading, linking, code generation, dan optimasi dinamis.

## JVM dan class file

[Spesifikasi virtual machine](https://docs.oracle.com/javase/specs/jvms/se25/html/jvms-1.html) menyatakan JVM hanya mengenal format biner class file, bukan bahasa Java.

Setiap class file mendeklarasikan versi major dan minor. Versi itu mengikat file pada rilis Java SE tertentu dan memengaruhi interpretasi runtime.

Bahasa apa pun yang dapat diekspresikan sebagai class file valid dapat dihosting oleh JVM.

## Memori dan garbage collection

Java memakai automatic storage management, biasanya garbage collector. Ini menghindari masalah dealokasi eksplisit seperti `free` atau `delete`.

Bahasa ini tidak menyertakan konstruksi unsafe seperti akses array tanpa pengecekan indeks.

HotSpot menyediakan beberapa kolektor untuk kebutuhan berbeda. [Panduan tuning](https://docs.oracle.com/en/java/javase/25/gctuning/introduction-garbage-collection-tuning.html) menyatakan GC menghapus sebagian kelas error dengan biaya overhead runtime.

Java SE memilih kolektor default berdasarkan kelas komputer. Aplikasi besar dengan banyak thread dan transaksi tinggi biasanya perlu memilih dan menyetel kolektor secara eksplisit.

## OpenJDK dan distribusi

[OpenJDK](https://openjdk.org/) adalah tempat kolaborasi implementasi Java Platform secara open-source.

Oracle menyediakan biner OpenJDK berlisensi GPL di jdk.java.net. Biner JDK komersial berbasis kode yang sama tersedia terpisah di situs Oracle.

Keduanya berbagi basis kode, tetapi lisensi dan jalur distribusinya berbeda.

## Android

Android tidak memakai runtime Java SE biasa. [Dokumentasi runtime](https://source.android.com/docs/core/runtime) menyatakan ART mengeksekusi format DEX beserta spesifikasinya.

Kode Java atau Kotlin untuk Android melewati toolchain menuju DEX. Asumsi class file, opsi kolektor, dan tooling SE harus diperiksa ulang pada platform ini.

## Keamanan

[Gambaran keamanan Java](https://docs.oracle.com/en/java/javase/25/security/java-security-overview1.html) mencakup kriptografi, public key infrastructure, komunikasi aman, autentikasi, dan access control.

API dan verifikasi tersebut memberi kerangka kerja, bukan bukti keamanan aplikasi. Keamanan akhir tetap bergantung pada kode, konfigurasi, dan deployment.

## Batas riset

Enam dokumen resmi diambil penuh melalui 9Router pada 7 September 2026. Java SE 25 dipakai sebagai baseline dokumenter, bukan klaim versi terbaru.

Tidak ada statistik adopsi, benchmark, pengujian runtime, audit keamanan, atau nasihat lisensi. Tanggal publikasi sumber tidak diverifikasi independen.

Bukti, kutipan, tanggal, konflik, dan teks lengkap tersimpan dalam [[_raw/articles/java-research-packet\|paket riset Java]].

Java berhubungan dengan [[References/TypeScript\|TypeScript]], [[References/Type Checkers\|Type Checkers]], [[References/Mobile Apps\|Mobile Apps]], [[Shared mobile code still needs platform boundaries\|Shared mobile code still needs platform boundaries]], dan [[Dynamic inputs require runtime validation despite static types\|Dynamic inputs require runtime validation despite static types]].

## Lanjutan

- [[Static typing moves Java errors to compile time without proving runtime behavior\|Static typing moves Java errors to compile time without proving runtime behavior]]
- [[JVM executes versioned class files rather than Java source directly\|JVM executes versioned class files rather than Java source directly]]
- [[Garbage collection trades manual memory errors for runtime tuning work\|Garbage collection trades manual memory errors for runtime tuning work]]
- [[Android ART executes DEX bytecode, not Java SE class files\|Android ART executes DEX bytecode, not Java SE class files]]
