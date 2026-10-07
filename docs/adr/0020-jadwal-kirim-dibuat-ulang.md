# ADR-0020: Jadwal Kirim Disimpan sebagai Tabel dan Dibuat Ulang Setiap Ada Perubahan

- **Status:** Diterima
- **Tanggal:** 2026-10-07

## Konteks

Pengingat bermode Notifikasi menghasilkan beberapa pesan per kemunculan (misalnya H-7, H-2, H-0). Pengingat bisa berubah kapan saja: diubah, diabaikan satu kemunculannya, dihapus, atau diaktifkan kembali dari arsip. Setiap perubahan bisa membuat jadwal pesan yang lama menjadi salah.

## Keputusan

- Setiap pesan yang harus terkirim disimpan sebagai **satu baris jadwal kirim**, berisi pengingat asalnya, tanggal kemunculan, nilai H-x, tanggal kirim (waktu lokal), dan status.
- Jadwal kirim hanya dibuat untuk **kemunculan berikutnya**. Setelah pesan H-0 terkirim, jadwal untuk kemunculan selanjutnya dibuat.
- Setiap perubahan pada pengingat (ubah, abaikan, aktifkan kembali, hapus) **menghapus semua jadwal kirim yang belum terkirim** milik pengingat itu, lalu membuatnya ulang dari data terbaru. Jadwal yang sudah terkirim tidak disentuh.
- Mengganti zona waktu **tidak** perlu membuat ulang jadwal, karena tanggal kirim disimpan dalam waktu lokal dan pemicu selalu membandingkannya dengan "hari ini" di zona pemilik ([ADR-0013](0013-waktu-lokal-dan-zona-waktu.md)).

## Alternatif yang Dipertimbangkan

- **Menghitung langsung setiap hari tanpa tabel:** lebih sedikit data, tetapi tidak ada catatan pesan mana yang sudah terkirim, sehingga sulit mencegah pesan ganda dan mendeteksi pesan yang terlewat ([ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md)).
- **Mengedit baris jadwal yang lama satu per satu:** rawan kasus yang terlewat, misalnya daftar H-x yang berubah dari `7,2,0` menjadi `3,0`. Menghapus lalu membuat ulang selalu menghasilkan kondisi yang benar.

## Konsekuensi

- Logika pembuatan jadwal cukup ditulis di satu fungsi, dan dipanggil dari semua jenis perubahan.
- Aturan "H-x yang sudah lewat dilewati" ([ADR-0011](0011-default-h-x-dan-jam-kirim.md)) juga berlaku saat jadwal dibuat ulang.
