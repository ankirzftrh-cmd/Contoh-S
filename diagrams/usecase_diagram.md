# Usecase Diagram (Standar Skripsi)

## 1. Diagram Usecase

```mermaid
usecaseDiagram
    actor "Warga (Pemohon)" as Warga
    actor "Petugas DPMPTSP" as DPMPTSP
    actor "Dinas Kesehatan" as DinasKesehatan
    actor "Dinas Penanaman Modal" as PenanamanModal

    package "Sistem Pelayanan Perizinan Terpadu" {
        usecase "UC-01 Mengelola Akun" as UC1
        usecase "UC-02 Mengelola Permohonan Izin" as UC2
        usecase "UC-03 Melacak Status Izin" as UC3
        usecase "UC-04 Memverifikasi Berkas (Awal)" as UC4
        usecase "UC-05 Meneruskan Berkas" as UC5
        usecase "UC-06 Memverifikasi Berkas (Teknis)" as UC6
        usecase "UC-07 Mengunggah Rekomendasi/Penolakan" as UC7
        usecase "UC-08 Menerbitkan Surat Izin" as UC8
        usecase "UC-09 Melihat Dashboard Investasi" as UC9
        usecase "Login" as Login
    }

    Warga --> UC1
    Warga --> UC2
    Warga --> UC3

    DPMPTSP --> UC3
    DPMPTSP --> UC4
    DPMPTSP --> UC5
    DPMPTSP --> UC8

    DinasKesehatan --> UC3
    DinasKesehatan --> UC6
    DinasKesehatan --> UC7

    PenanamanModal --> UC9

    UC1 ..> Login : <<include>>
    UC2 ..> Login : <<include>>
    UC3 ..> Login : <<include>>
    UC4 ..> Login : <<include>>
    UC5 ..> Login : <<include>>
    UC6 ..> Login : <<include>>
    UC7 ..> Login : <<include>>
    UC8 ..> Login : <<include>>
    UC9 ..> Login : <<include>>
```

---

## 2. Deskripsi Skenario Usecase (Contoh Utama)

### **UC-02 Mengelola Permohonan Izin**
| Komponen | Deskripsi |
|---|---|
| **Aktor Utama** | Warga (Pemohon) |
| **Prakondisi** | Pemohon telah memiliki akun dan berhasil melakukan *login* ke dalam sistem. |
| **Alur Normal (Basic Flow)** | 1. Pemohon memilih menu "Pengajuan Izin".<br>2. Sistem menampilkan formulir pengajuan izin terkait.<br>3. Pemohon mengisi formulir permohonan dan mengunggah berkas persyaratan.<br>4. Pemohon menekan tombol "Submit".<br>5. Sistem memvalidasi kelengkapan data.<br>6. Sistem menyimpan data ke *database* dengan status "Menunggu Verifikasi DPMPTSP".<br>7. Sistem menampilkan pesan berhasil. |
| **Alur Alternatif (Alternate Flow)** | 5a. Jika format atau ukuran *file* tidak sesuai, Sistem menampilkan pesan *error* dan meminta Pemohon mengunggah ulang dokumen yang valid.<br>5b. Jika data kurang lengkap, Sistem menandai kolom yang wajib diisi. |
| **Pasca-kondisi** | Permohonan izin tersimpan di *database* dan siap untuk diverifikasi oleh Petugas DPMPTSP. |

### **UC-04 Memverifikasi Berkas (Awal)**
| Komponen | Deskripsi |
|---|---|
| **Aktor Utama** | Petugas DPMPTSP |
| **Prakondisi** | Petugas DPMPTSP telah *login* ke dalam sistem. Terdapat dokumen dengan status "Menunggu Verifikasi DPMPTSP". |
| **Alur Normal (Basic Flow)** | 1. Petugas membuka menu "Daftar Dokumen Masuk".<br>2. Sistem menampilkan daftar permohonan izin.<br>3. Petugas mengklik permohonan tertentu untuk melihat detail dan pratinjau dokumen persyaratan.<br>4. Petugas memeriksa kelengkapan administrasi.<br>5. Petugas menyetujui dokumen dan meneruskan dokumen ke instansi teknis (Dinas Kesehatan).<br>6. Sistem mengubah status permohonan menjadi "Menunggu Verifikasi Dinas Kesehatan". |
| **Alur Alternatif (Alternate Flow)** | 5a. Jika hasil periksa menyatakan syarat tidak lengkap, Petugas menekan tombol tolak dan mengisi alasan penolakan.<br>5b. Sistem mentransformasikan status menjadi Ditolak dan mengirim pesan terkait alasan tolak pada pemohon. |
| **Pasca-kondisi** | Permohonan izin memiliki status diteruskan ke tim teknis atau ditolak. |
