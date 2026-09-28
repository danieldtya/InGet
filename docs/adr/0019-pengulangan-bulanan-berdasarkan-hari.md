# ADR-0019: Pengulangan Bulanan Berdasarkan Urutan Hari

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Beberapa kegiatan berulang berdasarkan urutan hari, misalnya "Jumat pertama setiap bulan", bukan tanggal tetap. Bot berbasis perintah teks tidak punya antarmuka visual, sehingga sintaksnya tidak boleh membingungkan.

## Keputusan

- Frekuensi baru **`bulanan-hari`**. Pengguna tidak mengetik nama hari atau urutannya. Bot **menyimpulkannya dari tanggal kemunculan pertama**:

| Tanggal | Urutan |
| --- | --- |
| 1 sampai 7 | Pertama |
| 8 sampai 14 | Kedua |
| 15 sampai 21 | Ketiga |
| 22 sampai 28 | Keempat |
| 29 sampai 31 | Terakhir |

- Contoh: `bulanan-hari` dengan tanggal 02/10/2026 (Jumat) menghasilkan "Jumat pertama setiap bulan" (`FREQ=MONTHLY;BYDAY=1FR`), yaitu 2 Okt, 6 Nov, 4 Des, 1 Jan, 5 Feb.
- Bot **selalu menuliskan kembali tafsirannya** di bubble konfirmasi, misalnya "Setiap bulan, Jumat pertama".

## Alternatif yang Dipertimbangkan

- **Mengetik hari dan urutan secara eksplisit** (misalnya `jumat-1`): lebih fleksibel, tetapi menambah sintaks yang harus diingat.
- **Mingguan di beberapa hari sekaligus** (Senin dan Kamis): ditunda. Alternatifnya membuat beberapa pengingat mingguan terpisah.

## Konsekuensi

- "Terakhir" hanya dapat disimpulkan dari tanggal 29 sampai 31. Memilih hari terakhir di bulan yang hanya memiliki empat hari tersebut akan ditafsirkan sebagai "keempat". Karena tafsiran selalu ditampilkan, hal ini tidak terjadi diam-diam.
