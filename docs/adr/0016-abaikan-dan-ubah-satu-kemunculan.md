# ADR-0016: Abaikan dan Ubah Satu Kemunculan

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Kadang satu kemunculan pengingat berulang perlu dilewati (misalnya olahraga minggu ini batal) atau diubah (tagihan bulan ini jatuh di tanggal lain), tanpa mengubah kemunculan lainnya.

## Keputusan

- **Abaikan** (`/abaikan`): satu tanggal kemunculan dilewati. Disimpan sebagai pengecualian tanggal, setara `EXDATE` dalam RFC 5545, yang didukung langsung oleh `python-dateutil`.
- **Ubah satu kemunculan:** dibangun dari dua langkah yang sudah ada, yaitu mengabaikan kemunculan aslinya, lalu **otomatis membuat pengingat "sekali" baru** dengan data yang sudah diubah.
- Mengubah pengingat berulang **tanpa** menyebut tanggal kemunculan tetap berlaku untuk **semua kemunculan berikutnya**.
- Kedua fitur hanya berlaku untuk kemunculan yang **belum terjadi**.

## Alasan

Cukup satu mekanisme pengecualian yang perlu dibangun. "Ubah satu kemunculan" tidak memerlukan struktur penyimpanan baru.

## Konsekuensi

- Tabel pengingat membutuhkan daftar tanggal pengecualian.
- Jadwal kirim untuk kemunculan yang diabaikan harus ikut dihapus.
