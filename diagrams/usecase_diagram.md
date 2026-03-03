# Usecase Diagram

```mermaid
usecaseDiagram
    actor Warga as "Warga (Pemohon)"
    actor DPMPTSP as "Petugas DPMPTSP"
    actor DinasKesehatan as "Dinas Kesehatan"
    actor PenanamanModal as "Dinas Penanaman Modal"

    package "Sistem Pelayanan Perizinan DPMPTSP" {
        usecase "Register Akun" as UC1
        usecase "Login" as UC2
        usecase "Upload Permohonan Izin" as UC3
        usecase "Lihat Status Dokumen" as UC4
        usecase "Verifikasi Permohonan (Awal)" as UC5
        usecase "Teruskan ke Dinas Terkait" as UC6
        usecase "Tolak Permohonan" as UC7
        usecase "Verifikasi Rekomendasi" as UC8
        usecase "Upload Surat Rekomendasi/Penolakan" as UC9
        usecase "Terbitkan Surat Izin" as UC10
        usecase "Lihat Dashboard Investasi" as UC11
    }

    Warga --> UC1
    Warga --> UC2
    Warga --> UC3
    Warga --> UC4

    DPMPTSP --> UC2
    DPMPTSP --> UC4
    DPMPTSP --> UC5
    DPMPTSP --> UC6
    DPMPTSP --> UC7
    DPMPTSP --> UC10

    DinasKesehatan --> UC2
    DinasKesehatan --> UC4
    DinasKesehatan --> UC8
    DinasKesehatan --> UC9

    PenanamanModal --> UC2
    PenanamanModal --> UC11
```
