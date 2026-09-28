# ADR-0017: Tanggal Akhir, Arsip 30 Hari, dan Aktivasi Ulang

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Pengingat sekali akan menumpuk di `/daftar` setelah hari H lewat. Pengingat berulang seperti cicilan juga punya akhir yang jelas, dan tanpa tanggal akhir pengguna harus ingat sendiri untuk menghapusnya. Selain itu, pengguna bisa melewatkan kegiatan sekali karena urusan mendesak dan perlu menjadwalkannya ulang.

## Keputusan

- Pengingat berulang boleh memiliki **tanggal akhir** opsional (`UNTIL` dalam RRULE). Batas berupa jumlah kemunculan (`COUNT`) tidak dipakai, karena tanggal pembayaran terakhir lebih mudah diketahui daripada sisa jumlah cicilan.
- Pengingat yang **selesai** (sekali yang hari H-nya lewat, atau berulang yang melewati tanggal akhir) ditandai selesai dan hilang dari `/daftar` dan `/agenda`.
- **`/arsip`** menampilkan pengingat yang selesai dalam 30 hari terakhir.
- Pengingat di arsip dapat **diaktifkan kembali** dengan `/ubah` ke tanggal di masa depan.
- Pengingat yang selesai lebih dari **30 hari** dihapus otomatis oleh proses harian.

## Alasan

- Arsip hanya berguna jika bisa dilihat. Tanpa `/arsip`, arsip sama saja dengan "dihapus 30 hari kemudian".
- Penghapusan tidak dilakukan untuk menghemat penyimpanan (ukuran satu baris hanya beberapa ratus byte), melainkan untuk menjaga arsip tetap ringkas.

## Konsekuensi

- Tabel pengingat membutuhkan kolom status dan waktu selesai.
