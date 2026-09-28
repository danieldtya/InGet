# ADR-0001: Memakai WhatsApp Cloud API Resmi dengan Nomor Khusus

- **Status:** Diterima
- **Tanggal:** 2026-09-24

## Konteks

Masalah utama yang diselesaikan adalah pengingat yang terabaikan karena harus membuka aplikasi terlebih dahulu. WhatsApp adalah satu-satunya aplikasi yang notifikasinya selalu diperhatikan oleh pengguna, sehingga menjadi saluran utama.

Ada dua cara membuat bot WhatsApp: API resmi dari Meta (WhatsApp Cloud API), atau library tidak resmi yang meniru WhatsApp Web (misalnya Baileys atau whatsapp-web.js).

## Keputusan

Memakai **WhatsApp Cloud API resmi**, dengan **nomor telepon kedua** yang khusus didaftarkan untuk bot.

## Alternatif yang Dipertimbangkan

- **Library tidak resmi:** gratis dan tanpa aturan template, tetapi melanggar ketentuan layanan WhatsApp dan nomornya berisiko diblokir. Untuk sistem pengingat, keandalan adalah fitur utama, sehingga risiko ini tidak dapat diterima.
- **Bot Telegram:** gratis dan API-nya paling mudah, tetapi notifikasinya jarang diperhatikan pengguna. Saluran yang tidak diperhatikan menggagalkan tujuan utama aplikasi.
- **PWA dengan Web Push, email, atau SMS:** memiliki masalah yang sama dengan aplikasi kalender, atau berbayar.

## Konsekuensi

- Pesan yang dikirim di luar jendela layanan 24 jam (sejak pesan terakhir dari pengguna) harus memakai **template pesan** yang disetujui Meta. Template pengingat masuk kategori *utility*.
- Ada biaya per pesan template, tetapi untuk satu pengguna jumlahnya sangat kecil.
- Nomor bot tidak boleh aktif di aplikasi WhatsApp atau WhatsApp Business biasa saat didaftarkan.
- Perlu akun Meta Developer dan konfigurasi webhook.
