# ADR-0012: Hitung Mundur di `/agenda` untuk Semua Frekuensi Kecuali Harian dan Mingguan

- **Status:** Diterima
- **Tanggal:** 2026-09-28
- **Menggantikan:** [ADR-0005](0005-hitung-mundur-di-rekap.md)

## Konteks

ADR-0005 menempatkan hitung mundur di rekap harian otomatis, yang kini sudah tidak ada ([ADR-0010](0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md)). ADR-0005 juga meninggalkan pertanyaan terbuka tentang pengingat berfrekuensi sekali.

## Keputusan

- **Hitung mundur** adalah bagian di bawah hasil `/agenda` yang menampilkan pengingat yang akan datang dalam **7 hari** ke depan, misalnya *"Pajak motor: 5 hari lagi (25 Oktober)"*.
- Berlaku untuk **semua mode**, dan untuk **semua frekuensi kecuali harian dan mingguan**.
- Jendela 7 hari bersifat **tetap** dan disimpan di konfigurasi.

## Perbedaan dengan H-x

| | H-x | Hitung mundur |
| --- | --- | --- |
| Bentuk | Pesan terkirim otomatis | Daftar di dalam `/agenda` |
| Muncul kapan | Hanya di hari H-x | Setiap kali `/agenda` dibuka, selama masih dalam 7 hari |
| Berlaku untuk | Mode Notifikasi | Semua mode |

## Alasan

- Pengingat bermode Rekap tidak pernah terkirim otomatis. Tanpa hitung mundur, pengingat itu baru terlihat di `/agenda` tepat di hari H, saat sudah terlambat untuk bersiap.
- Pengingat **sekali** ikut dihitung mundur karena pengecualian dalam ADR-0005 hanya dimaksudkan untuk mencegah pengingat mingguan muncul setiap hari. Pengingat sekali tidak berulang, sehingga tidak punya masalah itu.

## Konsekuensi

- Pengingat harian dan mingguan tidak pernah muncul di bagian hitung mundur.
