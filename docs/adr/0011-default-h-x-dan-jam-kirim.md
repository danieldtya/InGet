# ADR-0011: Default H-x per Frekuensi dan Semua Notifikasi Dikirim Jam 00.05

- **Status:** Diterima
- **Tanggal:** 2026-09-28
- **Menggantikan:** [ADR-0004](0004-pengingat-h-x.md)

## Konteks

ADR-0004 menetapkan pesan H-x dikirim bersamaan dengan jam rekap harian dan H-0 di jam kegiatan. Setelah rekap otomatis dihapus ([ADR-0010](0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md)), acuan jam itu tidak ada lagi.

Selain itu, H-7 tidak masuk akal untuk pengingat harian, dan untuk pengingat mingguan jatuh tepat di hari kemunculan sebelumnya.

## Keputusan

- Setiap pengingat bermode Notifikasi dapat memiliki **lebih dari satu H-x**.
- **Default H-x:**

| Frekuensi | Default |
| --- | --- |
| Sekali, bulanan, bulanan-hari, tahunan, setiap N tahun | `7,2,0` |
| Harian, mingguan | `0` |

- **Semua pesan Notifikasi dikirim jam 00.05** waktu lokal pengguna, termasuk H-0. Jam kegiatan tidak memengaruhi jam kirim.
- H-x yang tanggalnya **sudah lewat saat pengingat dibuat dilewati**. Contoh: pengingat dibuat 1 Oktober untuk kegiatan 4 Oktober, maka H-7 (27 September) dilewati, sedangkan H-2 dan H-0 tetap terkirim.

## Alasan

- **00.05, bukan 00.00:** pemicu terjadwal tidak selalu tepat sampai hitungan detik. Jika pemicu berjalan di 23.59.59, sistem menganggap hari masih kemarin dan pengingat terlambat satu hari. Jeda lima menit menghilangkan risiko itu.
- Pengguna baru membaca notifikasi setelah bangun tidur, sehingga jam kirim yang tepat tidak penting selama pesan sudah ada sebelum pagi.
- Pesan yang dikirim di hari yang sama membuat pengiriman cukup berbasis **tanggal**, sehingga penjadwalan jauh lebih sederhana.

## Konsekuensi

- Kegiatan yang jamnya sebelum 00.05 akan menerima pesan H-0 setelah kegiatan berlangsung. Kasus ini dianggap jarang.
- Jam kirim tidak dapat diubah lewat perintah.
