---
{"dg-publish":true,"dg-path":"Cara Kerja LLM.md","permalink":"/cara-kerja-llm/","title":"Cara Kerja LLM","hideInFiletree":true,"tags":["references","gpt","architecture","algorithms","research"],"noteIcon":"","dg-note-properties":{"title":"Cara Kerja LLM","category":"references","tags":["references","gpt","architecture","algorithms","research"],"sources":["_raw/articles/cara-kerja-llm-expanded.md"],"created":"2026-08-26","updated":"2026-09-02","confidence":"medium"}}
---

![Cara Kerja LLM](/img/user/Attachments/2026-08-26-Cara-Kerja-LLM.png)
LLM (*large language model*) adalah model pembelajaran mesin yang dilatih pada kumpulan teks berskala besar untuk mengenali pola bahasa dan menghasilkan teks.

Model ini dapat mendukung pembuatan teks, penerjemahan, peringkasan, tanya jawab, analisis sentimen, pencarian, layanan pelanggan, dan penulisan kode.

Penerapannya dalam perangkat lunak dibahas lebih lanjut di [[References/Generative AI for Frontend Development\|Generative AI for Frontend Development]] dan [[References/AI dan Pengembangan Perangkat Lunak Tradisional\|AI dan Pengembangan Perangkat Lunak Tradisional]].

Kemampuan tersebut berasal dari satu mekanisme umum: memperkirakan probabilitas token berdasarkan konteks yang tersedia.

## Teks diubah menjadi token

LLM tidak menerima kalimat sebagai rangkaian kata utuh. *Tokenizer* lebih dahulu memecah teks menjadi token, yaitu unit yang dapat berupa kata, bagian kata, atau karakter.

Banyak model modern menggunakan token subkata agar kosakata tetap efisien dan dapat menangani kata yang jarang muncul atau belum pernah dilihat dalam bentuk lengkap.

Setiap token dipetakan ke sebuah ID, lalu diubah menjadi vektor numerik yang disebut *embedding*. Vektor tersebut menjadi representasi yang dapat diproses oleh jaringan saraf.

Karena setiap *tokenizer* memiliki aturan berbeda, jumlah token untuk kalimat yang sama dapat berbeda antara model dan bahasa.

## Transformer membaca hubungan dalam konteks

Sebagian besar LLM modern menggunakan arsitektur Transformer, yaitu jaringan saraf yang mengandalkan mekanisme *attention* untuk memproses hubungan antartoken.

Dalam *self-attention*, setiap posisi dalam urutan menghitung relevansinya terhadap posisi lain, sehingga model dapat menghubungkan informasi yang letaknya berjauhan dalam teks.

Transformer menggantikan ketergantungan utama pada pemrosesan berulang seperti pada jaringan saraf rekuren dan memungkinkan lebih banyak komputasi dilakukan secara paralel ketika pelatihan.

Secara ringkas, setiap token menghasilkan representasi *query*, *key*, dan *value*. Kecocokan antara *query* dan *key* menentukan bobot perhatian, sedangkan gabungan berbobot dari *value* membentuk representasi kontekstual baru.

Mekanisme *multi-head attention* menjalankan beberapa operasi perhatian secara paralel agar model dapat menangkap jenis hubungan yang berbeda pada posisi dan ruang representasi yang berbeda.

Urutan token juga perlu direpresentasikan karena mekanisme perhatian tidak dengan sendirinya menyimpan posisi. Transformer asli menambahkan *positional encoding* pada *embedding* agar model dapat membedakan urutan token.

## Pelatihan membentuk parameter model

Pada tahap *pre-training*, model mempelajari pola dari korpus teks besar dengan mengerjakan tugas prediksi token.

Kesalahan antara prediksi dan target menghasilkan nilai *loss*, lalu proses *backpropagation* menyesuaikan parameter jaringan agar prediksi berikutnya lebih baik.

Pengulangan proses ini pada banyak contoh membentuk representasi statistik mengenai tata bahasa, hubungan semantik, gaya, dan pola yang sering muncul dalam data.

Istilah "large" merujuk pada skala data, komputasi, dan terutama jumlah parameter yang dipelajari model. Parameter bukan kumpulan kalimat yang dapat dicari seperti basis data.

Parameter adalah bobot numerik yang mengatur bagaimana masukan diubah menjadi prediksi.

Model dasar dapat disesuaikan melalui *fine-tuning* menggunakan contoh yang lebih khusus untuk suatu tugas.

*Fine-tuning* standar memperbarui bobot dan bias model, sedangkan metode yang efisien parameter hanya menyesuaikan sebagian parameter.

*Prompt engineering* bekerja secara berbeda: instruksi dan contoh diberikan saat inferensi tanpa mengubah parameter model. Praktik penyusunan, pengujian, dan pengamanan instruksi dibahas di [[References/Prompt Engineering\|Prompt Engineering]].

Kumpulan catatan terkait tersedia di [[Categories/Prompt\|Prompt]].

## Inferensi menghasilkan teks satu token pada satu waktu

Ketika pengguna mengirim *prompt*, model memproses seluruh token yang tersedia dalam jendela konteks dan menghasilkan skor untuk setiap token dalam kosakatanya.

Fungsi *softmax* mengubah skor keluaran menjadi distribusi probabilitas token berikutnya. Model tidak langsung memilih satu jawaban lengkap.

Model memilih satu token, menambahkannya ke urutan, lalu mengulangi perhitungan sampai mencapai token berhenti atau batas keluaran.

Pemilihan token tidak selalu dilakukan dengan mengambil probabilitas tertinggi. *Greedy search* memilih token yang paling mungkin pada setiap langkah, sedangkan *sampling* memilih token secara acak sesuai distribusi probabilitas.

Parameter *temperature* mengubah ketajaman distribusi, sementara *top-k* membatasi pemilihan pada sejumlah token dengan probabilitas tertinggi.

Karena proses pengambilan sampel dapat memilih jalur berbeda, *prompt* yang sama dapat menghasilkan jawaban berbeda.

Proses autoregresif ini menjelaskan mengapa sebuah keluaran dapat berubah arah setelah satu token yang kurang tepat terpilih. Setiap token baru menjadi bagian dari konteks bagi prediksi berikutnya.

Koherensi jawaban muncul dari pengulangan prediksi yang selalu dikondisikan pada urutan sebelumnya, bukan dari penyusunan seluruh paragraf sekaligus.

## Jendela konteks membatasi informasi aktif

Jendela konteks adalah jumlah token yang dapat dipertimbangkan model pada satu waktu.

Isinya mencakup *prompt* pengguna, instruksi sistem, riwayat percakapan, keluaran yang sedang dibuat, dan konteks eksternal yang disisipkan oleh sistem seperti RAG (*retrieval-augmented generation*).

Jika masukan melebihi batas tersebut, aplikasi perlu memangkas, meringkas, atau membagi teks sebelum memprosesnya.

Jendela yang lebih panjang dapat menampung dokumen dan percakapan yang lebih besar, tetapi tidak menjamin bahwa setiap informasi digunakan secara efektif.

Pada *self-attention* standar, kebutuhan komputasi bertambah secara kuadratik terhadap panjang urutan, sehingga konteks yang lebih panjang juga menambah biaya dan latensi.

## Mengapa satu mekanisme dapat menangani banyak tugas

Pembuatan teks, penerjemahan, peringkasan, dan tanya jawab dapat dirumuskan sebagai prediksi urutan token yang sesuai dengan instruksi dan konteks.

Selama pelatihan, model menemukan pola yang dapat dipakai kembali pada berbagai bentuk masukan. Saat inferensi, instruksi dalam *prompt* mengondisikan pola mana yang paling relevan tanpa mengubah parameter model.

Model juga dapat memperoleh informasi tambahan melalui RAG atau alat eksternal. Dalam RAG, sistem mengambil potongan informasi yang relevan dari sumber luar dan memasukkannya ke konteks sebelum model menghasilkan jawaban.

Pendekatan ini tidak menambahkan pengetahuan secara permanen ke parameter, tetapi menyediakan bukti yang dapat digunakan pada permintaan tersebut.

## Batas antara kelancaran dan kebenaran

Tujuan prediksi token adalah menghasilkan kelanjutan yang sesuai dengan pola bahasa, bukan memverifikasi kebenaran faktual terhadap dunia.

Akibatnya, LLM dapat menghasilkan pernyataan yang terdengar masuk akal tetapi salah, tidak relevan, atau sepenuhnya dibuat, yang dikenal sebagai halusinasi. Kualitas dan bias data pelatihan juga dapat memengaruhi perilaku model.

Karena itu, keluaran LLM sebaiknya diperlakukan sebagai hasil probabilistik yang perlu diverifikasi ketika menyangkut fakta, keputusan, atau bidang berisiko tinggi.

RAG, *fine-tuning* pada data yang dikurasi, *prompt* yang terstruktur, evaluasi berkala, dan peninjauan manusia dapat mengurangi risiko, tetapi tidak menghapusnya sepenuhnya.

LLM pada dasarnya mengubah teks menjadi token, membentuk representasi kontekstual dengan Transformer, menghitung distribusi probabilitas token berikutnya, lalu mengulangi proses tersebut sampai respons selesai.

Skala pelatihan membuat mekanisme ini cukup fleksibel untuk banyak tugas, sedangkan jendela konteks, strategi *decoding*, dan kualitas sumber menentukan keluaran yang dihasilkan.

## Sumber

- [Cloudflare: What is an LLM (large language model)?](https://www.cloudflare.com/en-gb/learning/ai/what-is-large-language-model/): definisi, Transformer, penggunaan, dan keterbatasan LLM.
- [Towards Data Science: New to LLMs? Start Here](https://towardsdatascience.com/new-to-llms-start-here/): model, agen, RAG, konteks, dan penyesuaian model.
- [Vaswani dkk.: Attention Is All You Need](https://arxiv.org/abs/1706.03762): arsitektur Transformer, *self-attention*, dan *multi-head attention*.
- [Hugging Face: Generation strategies](https://huggingface.co/docs/transformers/en/generation_strategies): *greedy search*, *sampling*, dan strategi generasi teks.
- [Google for Developers: Introduction to Large Language Models](https://developers.google.com/machine-learning/crash-course/llm): token, konteks, dan pemodelan probabilitas bahasa.
- [Google for Developers: Fine-tuning, distillation, and prompt engineering](https://developers.google.com/machine-learning/crash-course/llm/tuning): penyesuaian model dan rekayasa *prompt*.
- [IBM: What are AI hallucinations?](https://www.ibm.com/think/topics/ai-hallucinations): penyebab, dampak, dan mitigasi halusinasi.
- [IBM: What is a context window?](https://www.ibm.com/think/topics/context-window): tokenisasi, batas konteks, serta biaya komputasi.
