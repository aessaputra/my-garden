---
{"dg-publish":true,"permalink":"/leet-code-2-add-two-numbers/","title":"LeetCode 2 Add Two Numbers","hideInFiletree":true,"tags":["programming"],"dg-note-properties":{"title":"LeetCode 2 Add Two Numbers","tags":["programming"],"created":"2026-09-02","updated":"2026-09-02"}}
---

## Masalah

Dua linked list menyimpan dua bilangan bulat non-negatif. Setiap node berisi satu digit dan urutannya dibalik. Jumlahkan kedua bilangan lalu kembalikan hasilnya sebagai linked list dengan urutan yang sama.

```text
l1 = [2, 4, 3]  # 342
l2 = [5, 6, 4]  # 465
hasil = [7, 0, 8]  # 807
```

## Pola utama: linked list dan carry

Karena digit satuan berada di node pertama, kedua linked list dapat dijumlahkan langsung dari depan. Simpan kelebihan hasil penjumlahan dalam `carry` untuk digit berikutnya.

```text
total = digit1 + digit2 + carry
carry, digit = divmod(total, 10)
```

`digit` dimasukkan ke node hasil. `carry` dibawa ke iterasi berikutnya.

## Algoritma

1. Buat dummy node dan pointer `tail` untuk menyusun hasil.
2. Mulai `carry` dengan nilai `0`.
3. Ulangi selama `l1`, `l2`, atau `carry` masih ada.
4. Ambil nilai node saat ini. Gunakan `0` jika salah satu list sudah habis.
5. Hitung digit hasil dan carry dengan `divmod(total, 10)`.
6. Tambahkan digit hasil sebagai node baru.
7. Geser pointer yang masih memiliki node.
8. Kembalikan `dummy.next`.

## Implementasi Python

```python
class Solution:
    def addTwoNumbers(self, l1, l2):
        dummy = tail = ListNode()
        carry = 0

        while l1 or l2 or carry:
            digit1 = l1.val if l1 else 0
            digit2 = l2.val if l2 else 0
            carry, digit = divmod(digit1 + digit2 + carry, 10)

            tail.next = ListNode(digit)
            tail = tail.next

            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

        return dummy.next
```

## Dry run

Input:

```text
l1 = [2, 4, 3]
l2 = [5, 6, 4]
```

| Langkah | digit1 | digit2 | carry masuk | total | digit hasil | carry keluar |
|---:|---:|---:|---:|---:|---:|---:|
| 1 | 2 | 5 | 0 | 7 | 7 | 0 |
| 2 | 4 | 6 | 0 | 10 | 0 | 1 |
| 3 | 3 | 4 | 1 | 8 | 8 | 0 |

Hasilnya adalah `[7, 0, 8]`, yang mewakili angka `807`.

## Kasus penting

Untuk `[9, 9] + [1]`:

```text
9 + 1 + 0 = 10  → digit 0, carry 1
9 + 0 + 1 = 10  → digit 0, carry 1
0 + 0 + 1 = 1   → digit 1, carry 0
```

Hasilnya `[0, 0, 1]`. Kondisi `while l1 or l2 or carry` memastikan carry terakhir tidak hilang.

## Mengapa memakai dummy node?

Dummy node menghilangkan kondisi khusus untuk node hasil pertama. Semua digit ditambahkan dengan cara yang sama melalui `tail.next`, lalu hasil sebenarnya dimulai dari `dummy.next`.

## Kompleksitas

Jika panjang kedua linked list adalah `m` dan `n`:

- Waktu: O(max(m, n))
- Ruang tambahan selain hasil: O(1)
- Ruang linked list hasil: O(max(m, n))

## Pola yang perlu diingat

Saat menjumlahkan data digit per digit:

1. Proses kedua sumber secara bersamaan.
2. Gunakan nilai netral `0` jika salah satu sumber lebih pendek.
3. Pisahkan digit dan carry dengan pembagian serta modulo.
4. Jangan berhenti sebelum carry terakhir selesai diproses.

## Ringkasan wawancara

Telusuri kedua linked list secara bersamaan. Pada setiap posisi, jumlahkan kedua digit dan carry. Simpan `total % 10` sebagai node hasil dan gunakan `total // 10` sebagai carry berikutnya. Lanjutkan sampai kedua list dan carry habis. Kompleksitasnya O(max(m, n)) waktu dan O(1) ruang tambahan di luar hasil.
