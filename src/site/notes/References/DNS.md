---
{"dg-publish":true,"dg-path":"DNS.md","permalink":"/dns/","title":"DNS","hideInFiletree":true,"tags":["network","dns","guide"],"dg-note-properties":{"title":"DNS","category":"references","tags":["network","dns","guide"],"sources":["_raw/articles/dns-howdns-works.md","_raw/articles/dns-cloudflare.md"],"created":"2026-08-21","updated":"2026-08-21","confidence":"medium"}}
---

DNS (Domain Name System) adalah phonebook Internet: sistem yang menerjemahkan domain names menjadi IP addresses sehingga browser bisa memuat website. Halaman ini menyintesis komik howdns.works (perjalanan resolver) dan penjelasan teknis Cloudflare.

## Perjalanan satu DNS lookupx
![2026-08-31-how-dns-works.webp\|How DNS Works](/img/user/Attachments/2026-08-31-how-dns-works.webp)

1. User mengetik `example.com` di browser. Browser minta OS menerjemahkan nama ke IP.
2. OS (stub resolver) cek cache lokal. Jika tidak ada, kirim query ke DNS resolver (biasanya milik ISP).
3. Resolver cek cache-nya sendiri. Tidak ada, perjalanan dimulai.
4. Resolver tanya root server. Root tidak tahu IP domain, tapi tahu alamat TLD server (.COM).
5. Resolver tanya .COM TLD server. TLD tidak tahu IP domain, tapi mengembalikan daftar authoritative name servers (ns1/ns2/ns3/ns4.dnsimple.com) plus IP-nya (glue records).
6. Resolver tanya authoritative name server. Ini otoritas mutlak untuk domain tersebut: tidak ada cache, jawaban asli.
7. Authoritative mengembalikan IP address domain (contoh: 50.31.213.210).
8. Resolver membawa jawaban pulang ke OS; OS dan browser meng-cache hasilnya.
9. Browser connect ke IP, web server mengirim HTML, halaman dirender.

## Empat server yang terlibat

1. **DNS recursor (resolver)**. Seperti pustakawan: menerima query client dan melacak record dengan request tambahan. Berada di awal pipeline.
2. **Root nameserver**. Seperti indeks perpustakaan: menunjuk ke lokasi lebih spesifik. Ada 13 nama logis ([a-m].root-servers.net), dioperasikan 12 organisasi, tersebar global dengan Anycast (banyak server fisik per nama logis).
3. **TLD nameserver**. Seperti rak buku spesifik: menyimpan bagian terakhir hostname (.com). ICANN mengoordinasikan kebanyakan TLD. Jenis: country code (.fr, .jp), internationalized (bahasa lokal), generic (.net, .org, .edu), infrastructure (.arpa untuk reverse DNS).
4. **Authoritative nameserver**. Seperti kamus: sumber kebenaran final untuk record domain. Berada di akhir pipeline dan menjawab dari datanya sendiri tanpa query sumber lain. Biasanya ada lebih dari satu per domain untuk load distribution dan availability. Untuk melihatnya: query WHOIS.

## Urutan lookup dan langkah

Dari Cloudflare, 8 langkah saat tidak ada cache: user → resolver → root → TLD → nameserver domain → IP kembali ke resolver → resolver → browser → HTTP request → webpage.

DNS lookup bisa di-skip oleh caching di beberapa level: browser cache → OS cache → resolver ISP cache. Time-to-live (TTL) menentukan berapa lama record disimpan di tiap lokasi.

## Jenis query DNS

1. **Recursive query**: client minta resolver menjawab dengan record ATAU error.
2. **Iterative query**: server mengembalikan jawaban terbaik, yaitu record atau referral ke server yang lebih rendah di namespace. Client terus query referral sampai tuntas.
3. **Non-recursive query**: jawaban langsung dari cache atau karena server authoritative. Lookup tanpa cache memakai kombinasi recursive dan iterative.

## Glue records

Masalahnya: bagaimana resolver menemukan `ns1.dnsimple.com` sebelum `dnsimple.com`? ns1 adalah subdomain; tidak bisa resolve subdomain tanpa domain, sehingga terjadi circular dependency loop.

Solusinya: saat TLD merespons daftar name servers, dilampirkan minimal satu IP address per name server (glue). Resolver mendapat nama dan IP sekaligus, dan loop terputus.

## Eksperimen

- [messwithdns.net](https://messwithdns.net): tool interaktif. Tiap user mendapat subdomain `*.messwithdns.com`, bisa menambah record DNS apa pun dan melihat live request. Cara aman belajar DNS dengan eksperimen tanpa konsekuensi.
- Perintah praktis: `whois domain.example` untuk melihat authoritative name servers; `dig` atau `nslookup` untuk query langsung.

## Lihat juga

- [[References/Domain Name\|Domain Name]]: apa itu domain, struktur TLD/SLD, registrasi; DNS lookup dijelaskan singkat
- [[References/How Does Internet Work\|How Does Internet Work]]: TCP/IP, ports, sockets, koneksi
- [[References/Web Hosting\|Web Hosting]]: DNS menghubungkan domain ke server hosting
- [[References/HTTP\|HTTP]]: protokol yang dijalankan setelah DNS selesai
- [[InterPlanetary Name System\|InterPlanetary Name System]]: alternatif penamaan terdesentralisasi