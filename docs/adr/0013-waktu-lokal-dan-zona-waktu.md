# ADR-0013: Aturan Disimpan dalam Waktu Lokal dan Zona Waktu Dapat Diubah Lewat Perintah

- **Status:** Diterima
- **Tanggal:** 2026-09-28
- **Menggantikan:** [ADR-0007](0007-aturan-pengulangan-dan-waktu.md)

## Konteks

ADR-0007 menetapkan semua waktu disimpan dalam UTC. Setelah semua notifikasi dikirim pada satu jam tetap ([ADR-0011](0011-default-h-x-dan-jam-kirim.md)), sistem cukup bekerja dengan **tanggal lokal**. Pengguna juga ingin bisa mengganti zona waktu tanpa mengubah kode.

## Keputusan

**Aturan pengulangan** (tetap dari ADR-0007):

- Disimpan dalam format **RRULE (RFC 5545)**.
- Tanggal yang tidak ada di suatu bulan (29, 30, 31) dijadwalkan ke **hari terakhir bulan tersebut**.
- Pengingat 29 Februari dijadwalkan ke **28 Februari** pada tahun bukan kabisat.

**Waktu:**

- Tanggal dan jam kegiatan disimpan sebagai **waktu lokal** (jam dinding), bukan UTC. Kegiatan jam 08.00 tetap tertulis 08.00 walaupun zona waktu diganti.
- Jam kegiatan bersifat **opsional**, hanya dipakai untuk tampilan dan urutan di `/agenda`.
- Jadwal kirim disimpan sebagai **tanggal kirim** lokal.
- Setiap kali kode membutuhkan "hari ini", tanggal diambil **dengan zona waktu pengguna secara eksplisit**, tidak memakai jam server.

**Zona waktu:**

- Default **WIB**. Dapat diubah dengan `/atur zona`, pilihan dibatasi **WIB, WITA, dan WIT** untuk mencegah salah ketik.
- Zona waktu disimpan di database sebagai pengaturan pengguna.

## Alasan

Server di cloud umumnya berjalan dalam UTC, tujuh jam di belakang WIB. Pada 00.05 WIB, jam server masih menunjukkan hari sebelumnya. Jika kode memakai tanggal server, pengingat terlambat satu hari tanpa ada error apa pun.

## Konsekuensi

- Perintah `/atur` ada di MVP dengan satu pilihan: zona waktu.
- Pemicu terjadwal tidak bisa diatur pada satu jam UTC tetap, karena "00.05" jatuh di jam UTC yang berbeda untuk tiap zona. Lihat [ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md).
- Kasus tepi tanggal (29, 30, 31, dan 29 Februari) wajib memiliki unit test.
