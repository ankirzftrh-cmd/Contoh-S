# Pengujian Black Box (Black Box Testing)

Pengujian Black Box berfokus pada fungsionalitas sistem. Pengujian ini menggunakan metode *Equivalence Partitioning* dan *Boundary Value Analysis* untuk memastikan bahwa setiap input memberikan _output_ yang sesuai tanpa melihat struktur kode internal.

## 1. Skenario Pengujian: Login Pengguna
**Tujuan:** Memastikan sistem hanya mengizinkan pengguna dengan kredensial yang valid untuk mengakses *dashboard*.

| No | Data Input | Skenario yang Diharapkan | Hasil yang Didapat | Kesimpulan |
|---|---|---|---|---|
| 1 | `username` valid, `password` valid | Sistem memproses data, mengatur *session*, dan mengarahkan pengguna ke *dashboard* sesuai dengan tipe *role*. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 2 | `username` salah, `password` valid | Sistem menolak _login_ dan menampilkan pesan "Username tidak ditemukan". | Sesuai dengan harapan | **[Valid / Lolos]** |
| 3 | `username` valid, `password` salah | Sistem menolak _login_ dan menampilkan pesan "Password salah". | Sesuai dengan harapan | **[Valid / Lolos]** |
| 4 | Kolom input dikosongkan | Sistem tidak melanjutkan proses (ditahan oleh validasi `required` HTML5) dan menampilkan peringatan. | Sesuai dengan harapan | **[Valid / Lolos]** |

## 2. Skenario Pengujian: Upload Permohonan Izin Warga
**Tujuan:** Memastikan bahwa pemohon (warga) dapat mengajukan permohonan dengan data dan file (berkas persyaratan) yang tepat.

| No | Data Input | Skenario yang Diharapkan | Hasil yang Didapat | Kesimpulan |
|---|---|---|---|---|
| 1 | Form lengkap, file syarat berformat `.pdf` atau `.png` dengan ukuran di bawah ambang batas (contoh: < 5MB). | Sistem berhasil menyimpan data dalam format JSON ke database, menyalin file ke folder tujuan, dan mengembalikan pesan "Selamat, dokumen perizinan Anda berhasil dibuat". | Sesuai dengan harapan | **[Valid / Lolos]** |
| 2 | Mengunggah file dengan format yang tidak diizinkan (contoh: `.exe`, `.msi`, `.php`). | Sistem menolak *upload* dengan alasan ekstensi tidak valid/berbahaya. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 3 | Mengunggah file melebihi batas ukuran *server / aplikasi*. | Sistem membuntukan proses *upload*, memberi pesan *error* batasan ukuran dokumen. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 4 | Salah satu _field_ wajib pada formulir JSON tidak diisi. | Sistem mencegah *submit* formulir dan memberi tahu kolom mana yang kosong. | Sesuai dengan harapan | **[Valid / Lolos]** |

## 3. Skenario Pengujian: Tindakan Verifikasi Petugas (DPMPTSP/Dinas)
**Tujuan:** Memastikan pembaruan status dapat mengubah *flow* kepemilikan dokumen.

| No | Data Input | Skenario yang Diharapkan | Hasil yang Didapat | Kesimpulan |
|---|---|---|---|---|
| 1 | Klik "Teruskan ke Dinas" (Action = _forward_) | Dokumen berubah status menjadi "Menunggu Verifikasi Dinas Kesehatan" dan hilang dari antrean tunggu verifikasi awal DPMPTSP. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 2 | Klik "Tolak Perizinan" tanpa mengisi form alasan. | Sistem mencegah aksi sebelum petugas menuliskan alasan penolakan. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 3 | Dinas mengunggah BA (Surat Rekomendasi). | Dokumen berubah status menjadi "Menunggu Konfirmasi DPMPTSP", dan file BA tersimpan di database kolom `rekomendasi_filename`. | Sesuai dengan harapan | **[Valid / Lolos]** |
| 4 | DPMPTSP menerbitkan Izin (Unggah scan PDF Izin). | Dokumen final menjadi "Izin Diterima", tergenerasi nilai `izin_filename`, dan terlihat *link download* di panel pengguna. | Sesuai dengan harapan | **[Valid / Lolos]** |

---
**Kesimpulan Umum Pengujian Black Box:**  
Seluruh pengujian fungsional berjalan dengan baik dan berhasil mendeteksi input negatif dengan mekanisme *error handling* (*validation logic*). Parameter fungsi sistem pada DPMPTSP ini sukses memenuhi batasan kebutuhan.
