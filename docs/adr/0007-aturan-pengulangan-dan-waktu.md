# ADR-0007: Aturan Pengulangan Memakai RRULE dan Waktu Disimpan dalam UTC

- **Status:** Digantikan oleh [ADR-0013](0013-waktu-lokal-dan-zona-waktu.md)
- **Tanggal:** 2026-09-24

## Konteks

Aplikasi mendukung enam frekuensi: sekali, harian, mingguan, bulanan, tahunan, dan setiap N tahun. Ada beberapa kasus tepi yang sering baru ketahuan setelah aplikasi dipakai:

- **Tanggal 29, 30, 31:** pengingat bulanan "setiap tanggal 31" tidak memiliki tanggal di bulan Februari, April, Juni, September, dan November. Banyak library pengulangan **melewati** bulan tersebut tanpa peringatan, sehingga pengingat hilang diam-diam.
- **29 Februari:** masalah yang sama untuk pengulangan tahunan (misalnya ulang tahun).
- **Zona waktu:** server di cloud umumnya berjalan dalam UTC, sehingga jadwal bisa bergeser 7 jam dari WIB.

## Keputusan

- Aturan pengulangan disimpan dalam format **RRULE (RFC 5545)**, standar yang sama dengan Google Calendar. Contoh: setiap 5 tahun ditulis `FREQ=YEARLY;INTERVAL=5`.
- Tanggal yang tidak ada di suatu bulan dijadwalkan ke **hari terakhir bulan tersebut**.
- Pengingat 29 Februari dijadwalkan ke **28 Februari** pada tahun bukan kabisat.
- Semua waktu disimpan dalam **UTC** dan dikonversi ke zona waktu pengguna (default Asia/Jakarta) saat ditampilkan dan saat menghitung jadwal.

## Alternatif yang Dipertimbangkan

- **Format pengulangan buatan sendiri:** lebih sederhana di awal, tetapi menyulitkan integrasi kalender di masa depan dan rawan kasus tepi yang terlewat.

## Konsekuensi

- Kasus tepi di atas wajib memiliki unit test.
- Zona waktu disimpan sebagai pengaturan pengguna.
