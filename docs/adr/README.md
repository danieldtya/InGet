# Architecture Decision Records (ADR)

Folder ini mencatat keputusan desain penting beserta alasannya, supaya di kemudian hari jelas **mengapa** sistem dibangun seperti ini, bukan hanya **apa** yang dibangun.

## Status

| Status | Arti |
| --- | --- |
| Diusulkan | Masih bisa berubah, belum diterapkan |
| Diterima | Disepakati dan menjadi acuan |
| Digantikan | Diganti oleh ADR yang lebih baru (tautannya dicantumkan) |

ADR yang sudah Diterima **tidak diedit isinya**. Jika keputusan berubah, buat ADR baru yang menggantikannya, lalu ubah status ADR lama menjadi Digantikan. ADR berstatus Diusulkan boleh diperbarui langsung.

## Indeks

| No | Judul | Status |
| --- | --- | --- |
| [0001](0001-whatsapp-cloud-api-resmi.md) | Memakai WhatsApp Cloud API resmi dengan nomor khusus | Diterima |
| [0002](0002-alarm-tidak-masuk-mvp.md) | Fitur alarm tidak masuk MVP | Diterima |
| [0003](0003-dua-mode-notifikasi.md) | Dua mode notifikasi menggantikan tiga kuadran prioritas | Digantikan oleh 0010 |
| [0004](0004-pengingat-h-x.md) | Pengingat H-x yang bisa dipilih lebih dari satu | Digantikan oleh 0011 |
| [0005](0005-hitung-mundur-di-rekap.md) | Hitung mundur di rekap hanya untuk frekuensi bulanan ke atas | Digantikan oleh 0012 |
| [0006](0006-input-mvp-perintah-teks.md) | Input MVP memakai perintah teks, tanpa LLM | Diterima |
| [0007](0007-aturan-pengulangan-dan-waktu.md) | Aturan pengulangan memakai RRULE dan waktu disimpan dalam UTC | Digantikan oleh 0013 |
| [0008](0008-arsitektur-tidak-terikat-saluran.md) | Arsitektur tidak terikat pada satu saluran | Diterima |
| [0009](0009-rencana-stack-teknologi.md) | Rencana stack teknologi | Diusulkan |
| [0010](0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md) | Mode Notifikasi terkirim otomatis, mode Rekap hanya sesuai permintaan | Diterima |
| [0011](0011-default-h-x-dan-jam-kirim.md) | Default H-x per frekuensi dan semua notifikasi dikirim jam 00.05 | Diterima |
| [0012](0012-hitung-mundur-di-agenda.md) | Hitung mundur di `/agenda` untuk semua frekuensi kecuali harian dan mingguan | Diterima |
| [0013](0013-waktu-lokal-dan-zona-waktu.md) | Aturan disimpan dalam waktu lokal dan zona waktu dapat diubah lewat perintah | Diterima |
| [0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md) | Pemicu berjalan setiap jam dan pengingat terlewat tetap dikirim | Diterima |
| [0015](0015-tampilan-bubble-dan-agenda.md) | Satu bubble per kegiatan dan rentang `/agenda` | Diterima |
| [0016](0016-abaikan-dan-ubah-satu-kemunculan.md) | Abaikan dan ubah satu kemunculan | Diterima |
| [0017](0017-siklus-hidup-pengingat.md) | Tanggal akhir, arsip 30 hari, dan aktivasi ulang | Diterima |
| [0018](0018-hanya-melayani-nomor-pemilik.md) | Bot hanya melayani nomor pemilik | Diterima |
| [0019](0019-pengulangan-bulanan-berdasarkan-hari.md) | Pengulangan bulanan berdasarkan urutan hari | Diterima |
| [0020](0020-jadwal-kirim-dibuat-ulang.md) | Jadwal kirim disimpan sebagai tabel dan dibuat ulang setiap ada perubahan | Diterima |
| [0021](0021-idempotensi-pesan-masuk-dan-keluar.md) | Idempotensi pesan masuk dan pesan keluar | Diterima |
| [0022](0022-keamanan-webhook-dan-batas-input.md) | Verifikasi webhook dan batas input | Diusulkan |
| [0023](0023-bahasa-perintah-dan-dokumentasi.md) | Perintah dalam bahasa Inggris, dokumentasi dalam bahasa Indonesia | Diterima |

## Menulis ADR Baru

Salin [`0000-template.md`](0000-template.md), beri nomor berikutnya, lalu tambahkan ke indeks di atas.
