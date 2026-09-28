# ADR-0014: Pemicu Berjalan Setiap Jam dan Pengingat Terlewat Tetap Dikirim

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Semua notifikasi dikirim jam 00.05 waktu lokal ([ADR-0011](0011-default-h-x-dan-jam-kirim.md)), dan zona waktu dapat berubah ([ADR-0013](0013-waktu-lokal-dan-zona-waktu.md)). Selain itu, server bisa gagal atau mati, sehingga ada jadwal kirim yang terlewat.

Aturan sebelumnya adalah "tetap kirim selama hari H belum lewat". Aturan ini punya celah: jika server mati sepanjang hari H, pengingatnya **hilang tanpa pemberitahuan**, padahal aplikasi ini dibuat justru karena pengguna sulit mengingat.

## Keputusan

- Pemicu terjadwal membangunkan bot **setiap jam**. Setiap kali bangun, bot memeriksa apakah di zona waktu pengguna sudah lewat 00.05 dan hari itu belum diproses.
- Pemrosesan bersifat **idempoten**: menjalankannya dua kali tidak mengirim pesan ganda.
- Jadwal kirim yang tertunda tetapi **hari H-nya belum lewat** dikirim seperti biasa.
- Jadwal kirim yang **hari H-nya sudah lewat** dikumpulkan dan dikirim sebagai **satu pesan "Pengingat yang terlewat"**, bukan dibuang.

## Alasan

- Pemicu per jam tidak bergantung pada jam UTC tertentu, sehingga tetap benar setelah zona waktu diganti.
- Jika satu pemicu gagal, pemicu berikutnya satu jam kemudian mencoba lagi dengan sendirinya.

## Konsekuensi

- Lebih banyak pemanggilan pemicu per hari, tetapi masing-masing sangat ringan.
- Perlu penanda "hari terakhir yang sudah diproses" di database.
