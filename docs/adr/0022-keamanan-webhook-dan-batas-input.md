# ADR-0022: Verifikasi Webhook dan Batas Input

- **Status:** Diusulkan
- **Tanggal:** 2026-10-07

## Konteks

[ADR-0018](0018-hanya-melayani-nomor-pemilik.md) membatasi bot agar hanya melayani nomor pemilik. Namun alamat webhook adalah URL publik, dan nomor pengirim hanyalah isi dari data yang dikirim ke URL itu. Siapa pun yang mengetahui alamat webhook dapat mengirim data palsu yang **mengaku berasal dari nomor pemilik**, lalu mengakses atau menghapus semua pengingat.

Selain itu, input pemilik tetap perlu dibatasi untuk mencegah kesalahan, misalnya menempelkan teks sangat panjang atau perintah yang tidak sengaja terkirim berkali-kali.

## Usulan

**Verifikasi asal webhook:**

- Setiap webhook dari Meta membawa header `X-Hub-Signature-256`, yaitu tanda tangan HMAC-SHA256 dari isi pesan dengan **app secret** sebagai kuncinya. Sistem menghitung ulang tanda tangan itu dan **menolak** pesan yang tidak cocok.
- Pemeriksaan ini dilakukan **paling awal**, sebelum pemeriksaan nomor pemilik.

**Akses database:**

- Semua query memakai parameter (parameterized query atau ORM), tidak pernah menyusun SQL dengan menggabungkan teks dari pesan, sehingga SQL injection tidak mungkin terjadi.

**Batas input (angka awal, dapat disesuaikan):**

| Batas | Nilai |
| --- | --- |
| Panjang deskripsi pengingat | 100 karakter |
| Jumlah pengingat aktif | 100 |
| Jumlah nilai H-x per pengingat | 5 |
| Nilai H-x terbesar | 365 |

## Alasan Batas Input

- Deskripsi yang terlalu panjang membuat bubble sulit dibaca, dan parameter template WhatsApp juga memiliki batas panjang.
- Batas jumlah pengingat bukan untuk menghemat penyimpanan, melainkan sebagai pengaman jika terjadi bug atau perintah berulang yang tidak disengaja.

## Konsekuensi

- App secret disimpan sebagai rahasia di luar repositori, bersama token WhatsApp ([NFR-05](../kebutuhan.md)).
- Pesan kesalahan perlu disiapkan untuk setiap batas yang terlampaui.
