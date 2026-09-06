---
{"dg-publish":true,"dg-path":"Accessibility.md","permalink":"/accessibility/","title":"Accessibility","hideInFiletree":true,"tags":["references","development","ui","testing"],"noteIcon":"","dg-note-properties":{"title":"Accessibility","categories":["Web Accessibility","Web Development"],"tags":["references","development","ui","testing"],"sources":["_raw/articles/accessibility-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Accessibility adalah kualitas website, tool, dan teknologi yang memungkinkan penyandang disabilitas memahami serta menggunakannya. [W3C WAI](https://www.w3.org/WAI/fundamentals/accessibility-intro/) menempatkan kebutuhan pengguna sebagai tujuan, bukan sekadar kepatuhan atribut.

Disabilitas muncul dari interaksi kondisi seseorang dengan lingkungan dan hambatan. [WHO memperkirakan](https://www.who.int/news-room/fact-sheets/detail/disability-and-health) 1,3 miliar orang, atau 16 persen populasi dunia, mengalami disabilitas signifikan.

## Baseline teknis

WCAG mengelompokkan aksesibilitas ke empat prinsip: perceivable, operable, understandable, dan robust. Success criteria menyediakan persyaratan yang dapat diuji pada level A, AA, dan AAA.

WCAG bukan checklist empat item. Alt text, keyboard, contrast, dan captions mewakili kebutuhan penting, tetapi struktur, focus, forms, errors, timing, motion, serta compatibility juga menentukan pengalaman.

## Alternative text dan semantics

Alternative text menyampaikan tujuan non-text content. [WAI alt decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) membedakan gambar informative, functional, textual, complex, dan decorative karena konteks menentukan respons yang benar.

Gambar informative memerlukan deskripsi setara. Gambar functional harus menjelaskan aksi atau destination. Gambar decorative biasanya memakai `alt=""` agar screen reader tidak mengumumkan gangguan yang tidak bermakna.

Native HTML semantics memberi role, name, state, dan relationship yang dapat diproses browser serta assistive technology. ARIA melengkapi semantics yang kurang, tetapi tidak memperbaiki behavior atau keyboard interaction secara otomatis.

## Keyboard dan focus

[WCAG 2.1.1](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html) meminta semua functionality dapat dioperasikan melalui keyboard interface tanpa timing keystroke tertentu, kecuali fungsi yang memang bergantung pada path gerakan.

Operability membutuhkan lebih dari handler `click`. Focus order harus logis, focus indicator terlihat, custom widgets harus mengikuti interaction pattern, dan modal tidak boleh membiarkan focus tersesat ke background.

## Contrast dan media

[WCAG contrast minimum](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) menetapkan rasio 4.5:1 untuk text biasa dan 3:1 untuk large text, dengan pengecualian tertentu. Nilai ambang tidak boleh dibulatkan agar tampak lulus.

Color tidak boleh menjadi satu-satunya pembawa informasi. Status error, selection, dan chart memerlukan text, shape, pattern, atau indicator lain yang tetap tersedia saat color perception berubah.

[Captions](https://www.w3.org/WAI/media/av/captions/) memuat speech dan informasi audio non-speech yang diperlukan untuk memahami media. Captions harus akurat, tersinkronisasi, mengidentifikasi speaker, dan menandai suara penting.

## Pengujian

Automated audits cepat menemukan pola yang dapat dihitung, termasuk missing accessible name, contrast tertentu, invalid ARIA, dan label form. Pemeriksaan ini cocok untuk CI serta regression prevention.

Namun, [WAI menegaskan](https://www.w3.org/WAI/test-evaluate/) tidak ada tool tunggal yang menentukan sebuah site memenuhi accessibility standards. Keyboard flow, screen reader output, focus management, error recovery, serta task completion memerlukan evaluasi manusia.

Strategi yang sehat menggabungkan automated checks, review manual, pengujian keyboard, screen reader pada target platform, dan riset bersama penyandang disabilitas. [[References/Lighthouse\|Lighthouse]] berguna sebagai diagnostic, bukan sertifikat.

## Manfaat dan kewajiban

Accessibility juga membantu orang lanjut usia, cedera sementara, pengguna di bawah sinar terang, orang yang tidak dapat memutar audio, serta pengguna perangkat atau network terbatas.

[Business case W3C](https://www.w3.org/WAI/business-case/) mencakup market reach, innovation, brand, customer experience, productivity, dan legal risk. Dampak finansial tetap kontekstual dan tidak boleh diperlakukan sebagai uplift universal.

Kewajiban hukum berbeda menurut yurisdiksi dan sektor. Sebagai contoh terbatas, [aturan ADA Title II Amerika Serikat](https://www.ada.gov/resources/2024-03-08-web-rule/) memakai WCAG 2.1 Level AA untuk banyak web content dan mobile apps pemerintah negara bagian serta lokal.

Accessibility berhubungan dengan [[References/HTML\|HTML]], [[References/Design Systems\|Design Systems]], [[References/Web Components\|Web Components]], [[References/Testing Your Apps\|Testing Your Apps]], [[References/Chrome DevTools\|Chrome DevTools]], dan [[References/Aksesibilitas\|aksesibilitas React Native]].

## Lanjutan

- [[Semantics make accessibility resilient across interfaces\|Semantics make accessibility resilient across interfaces]]
- [[Keyboard access exposes pointer-only architecture\|Keyboard access exposes pointer-only architecture]]
- [[Alternative text must preserve purpose, not merely exist\|Alternative text must preserve purpose, not merely exist]]
- [[Automated audits prevent regressions but cannot certify accessibility\|Automated audits prevent regressions but cannot certify accessibility]]
