---
{"dg-publish":true,"dg-permalink":"craft/fc-barcelona-next-match-monitor","permalink":"/craft/fc-barcelona-next-match-monitor/","title":"N8N: FC Barcelona Next Match Monitor","hideInFiletree":true,"tags":["craft"],"noteIcon":"","dg-note-properties":{"title":"N8N: FC Barcelona Next Match Monitor","tags":["craft"],"created":"2026-09-19","updated":"2026-09-19"}}
---

![FC-Barcelona-Next-Match-Monitor-oFxtgZaG.webp](/img/user/Attachments/FC-Barcelona-Next-Match-Monitor-oFxtgZaG.webp)
Saya membangun workflow automation di [[n8n\|n8n]] self-hosted untuk memantau jadwal FC Barcelona. Tujuannya sederhana: dapat pengingat sebelum pertandingan tanpa harus bolak-balik mengecek jadwal.
![20260919_211823_ntfy.webp\|163](/img/user/Attachments/20260919_211823_ntfy.webp)
Workflow ini mengambil jadwal pertandingan terdekat dari Footballdata.io, lalu menyiapkan pengingat lewat [[ntfy\|ntfy]]: 24 jam sebelum pertandingan, satu jam sebelumnya, dan saat kick-off. Notifikasinya berisi kedua tim, waktu pertandingan dalam WIB, serta nama kompetisi jika tersedia. Prioritas pesan naik seiring mendekatnya waktu pertandingan, dan notifikasi saat kick-off menyertakan tombol "Streaming".

Saya juga menambahkan pengecekan agar workflow tidak memproses pertandingan yang sudah lewat, ditunda, atau dibatalkan. Kalau pengingat terlambat lebih dari lima menit, workflow melewatinya dan mengambil ulang jadwal.

Setelah semua pengingat selesai, workflow mencari pertandingan berikutnya. Kalau [[References/APIs\|API]] bermasalah atau jadwal belum tersedia, workflow menunggu sebelum mencoba lagi. Saya tinggal menunggu notifikasi masuk.