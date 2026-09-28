# ADR-0018: Bot Hanya Melayani Nomor Pemilik

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Siapa pun yang mengetahui nomor bot dapat mengirim pesan kepadanya, termasuk `/daftar` yang menampilkan seluruh pengingat, misalnya tagihan dan tanggal jatuh temponya.

## Keputusan

- Nomor WhatsApp utama pengguna disimpan di konfigurasi sebagai **satu-satunya pengirim yang dilayani**.
- Pesan dari nomor lain **diabaikan tanpa balasan**, sehingga pengirim bahkan tidak tahu bahwa bot aktif.

## Konsekuensi

- Pemeriksaan nomor dilakukan paling awal di webhook, sebelum pesan diproses lebih lanjut.
- Dukungan banyak pengguna di masa depan akan membutuhkan mekanisme pendaftaran, bukan sekadar menghapus pemeriksaan ini.
