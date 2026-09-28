# ADR-0005: Hitung Mundur di Rekap Hanya untuk Frekuensi Bulanan ke Atas

- **Status:** Digantikan oleh [ADR-0012](0012-hitung-mundur-di-agenda.md)
- **Tanggal:** 2026-09-24

## Konteks

Kegiatan yang jarang terjadi (bulanan, tahunan, setiap N tahun) mudah terlupa karena jarak antar kemunculannya jauh. Menampilkannya di rekap harian beberapa hari sebelumnya memberi waktu persiapan tanpa perlu mengatur H-x satu per satu.

Jika aturan yang sama diterapkan pada pengingat mingguan dengan jendela 7 hari, pengingat itu akan muncul di rekap **setiap hari tanpa henti**, karena jarak antar kemunculannya memang 7 hari.

## Keputusan

- Rekap harian memiliki bagian **hitung mundur**, misalnya *"Pajak motor: 5 hari lagi"*.
- Bagian ini hanya berlaku untuk frekuensi **bulanan, tahunan, dan setiap N tahun**, apa pun modenya.
- Jendela hitung mundur default **7 hari** dan dapat diubah lewat pengaturan.

## Alternatif yang Dipertimbangkan

- **Berlaku untuk semua frekuensi:** menimbulkan masalah pengingat mingguan yang muncul setiap hari.
- **Menentukan angka tetap (misalnya H-4) sekarang:** angka yang pas lebih baik ditentukan setelah aplikasi dipakai beberapa minggu.

## Konsekuensi

- Nilai jendela hitung mundur disimpan sebagai pengaturan pengguna.

## Pertanyaan Terbuka

Pengingat berfrekuensi **sekali** yang jauh di masa depan (misalnya janji dokter tiga minggu lagi) tidak mendapat hitung mundur menurut aturan ini. Perlu diputuskan apakah frekuensi sekali juga ikut, misalnya jika jaraknya lebih dari 7 hari saat pengingat dibuat.
