# ADR-0002: Fitur Alarm Tidak Masuk MVP

- **Status:** Diterima
- **Tanggal:** 2026-09-24

## Konteks

Rancangan awal menginginkan pengingat berprioritas tertinggi yang berbunyi seperti alarm. Namun notifikasi WhatsApp tetap tunduk pada mode senyap dan Do Not Disturb, sehingga tidak dapat menggantikan alarm.

Pilihan yang tersedia untuk meniru alarm:

1. Otomasi di HP (misalnya MacroDroid atau Tasker) yang membunyikan alarm saat notifikasi bot berisi kata kunci tertentu.
2. Panggilan WhatsApp dari bot melalui Calling API, yang membutuhkan izin pengguna, berbiaya per durasi, dan penanganan audio (WebRTC atau SIP).
3. Pesan dengan tombol konfirmasi yang dikirim ulang sampai ditanggapi.

## Keputusan

Fitur alarm dan eskalasi **tidak masuk MVP**. Pengingat cukup mengandalkan notifikasi WhatsApp biasa. Tombol konfirmasi dan pengiriman ulang juga tidak dibutuhkan.

## Alternatif yang Dipertimbangkan

Ketiga pilihan di atas ditunda. Opsi otomasi HP tetap bisa dipakai kapan saja tanpa perubahan kode, karena hanya membaca isi notifikasi bot.

## Konsekuensi

- Ruang lingkup MVP jauh lebih kecil dan realistis.
- Pengguna menerima risiko sesekali melewatkan notifikasi.
- Perbedaan antar tingkat prioritas menjadi tidak relevan, lihat [ADR-0003](0003-dua-mode-notifikasi.md).
