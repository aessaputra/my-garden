---
{"dg-publish":true,"permalink":"/leet-code-1-two-sum/","title":"LeetCode 1 Two Sum","hideInFiletree":true,"tags":["programming"],"dg-note-properties":{"title":"LeetCode 1 Two Sum","tags":["programming"],"created":"2026-09-01","updated":"2026-09-01"}}
---

## Masalah

Diberikan array integer `nums` dan integer `target`. Cari indeks dua elemen berbeda yang ejumlahnya sama dengan `target`. Setiap input dijamin mempunyai tepat satu jawaban.

## Pola utama: hash map

Untuk setiap nilai `num`, nilai pasangannya adalah:

```text
complement = target - num
```

Alih-alih mencari complement dengan loop kedua, simpan nilai yang sudah dilewati dalam hash map:

```text
seen[number] = index
```

Hash map memberi lookup rata-rata O(1), sehingga seluruh array hanya perlu dipindai sekali.

## Algoritma

1. Buat hash map kosong bernama `seen`.
2. Iterasi setiap `num` beserta indeks `i`.
3. Hitung `complement = target - num`.
4. Jika `complement` ada di `seen`, kembalikan `[seen[complement], i]`.
5. Jika belum ada, simpan `seen[num] = i`.

## Implementasi Python

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen = {}

        for i, num in enumerate(nums):
            complement = target - num

            if complement in seen:
                return [seen[complement], i]

            seen[num] = i

        return []
```

## Dry run

Input:

```text
nums = [3, 2, 4]
target = 6
```

| i | num | complement | seen sebelum pemeriksaan | Hasil |
|---:|---:|---:|---|---|
| 0 | 3 | 3 | `{}` | Simpan `3: 0` |
| 1 | 2 | 4 | `{3: 0}` | Simpan `2: 1` |
| 2 | 4 | 2 | `{3: 0, 2: 1}` | Temukan `2`, kembalikan `[1, 2]` |

## Mengapa pemeriksaan dilakukan sebelum penyimpanan?

Untuk input `[3, 3]` dengan target `6`, angka pertama disimpan sebagai `{3: 0}`. Saat angka kedua diproses, complement `3` ditemukan pada indeks `0`, sehingga hasilnya `[0, 1]`.

Urutan ini mencegah elemen saat ini dipasangkan dengan dirinya sendiri.

## Kompleksitas

| Pendekatan | Waktu | Ruang |
|---|---:|---:|
| Brute force, dua loop | O(n²) | O(1) |
| Hash map, satu lintasan | O(n) | O(n) |

## Pola yang perlu diingat

Saat soal meminta pasangan dengan jumlah tertentu:

1. Hitung nilai yang masih dibutuhkan.
2. Simpan nilai sebelumnya dalam hash map.
3. Periksa apakah nilai yang dibutuhkan sudah pernah ditemukan.

Pola ini dapat diterapkan dalam bahasa lain seperti [[References/JavaScript\|JavaScript]]. Pengujiannya dapat disusun dengan framework seperti [[References/Jest\|Jest]].

## Ringkasan wawancara

Pindai array satu kali. Simpan setiap angka sebelumnya beserta indeksnya. Untuk angka saat ini, hitung `target - num`. Jika hasilnya sudah ada di hash map, kembalikan indeks tersimpan dan indeks saat ini. Kompleksitasnya O(n) waktu dan O(n) ruang.
