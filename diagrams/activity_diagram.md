# Activity Diagram (Standar Skripsi)

## 1. Activity Diagram Pengajuan dan Verifikasi Izin

```mermaid
flowchart TD
    %% Define Styles
    classDef startEnd fill:#000,stroke:#333,stroke-width:2px,color:#fff,rx:20,ry:20;
    classDef process fill:#fff,stroke:#333,stroke-width:2px,rx:10,ry:10;
    classDef decision fill:#fff,stroke:#333,stroke-width:2px,shape:diamond;

    %% Swimlanes
    subgraph Warga [Warga / Pemohon]
        direction TB
        Start(((Mulai))):::startEnd
        LoginW[Melakukan Login]:::process
        PilihMenu[Memilih Menu Pengajuan Izin]:::process
        IsiForm[Mengisi Form dan Upload Syarat]:::process
        SubmitData[Menekan Tombol Submit]:::process
        
        LihatStatus[Melihat Status Dokumen]:::process
        DownloadIzin[Mengunduh Surat Izin]:::process
        End(((Selesai))):::startEnd
    end

    subgraph Sistem [Sistem Perizinan DB]
        direction TB
        ValidasiLogin[Validasi Kredensial]:::process
        SimpanPermohonan[Menyimpan Permohonan\\nStatus: Menunggu Verifikasi DPMPTSP]:::process
        UpdateStatusDinas[Update Status:\\nMenunggu Verifikasi Dinas]:::process
        SimpanRekomendasi[Menyimpan Rekomendasi\\nUpdate Status: Konfirmasi DPMPTSP]:::process
        TerbitkanIzinSistem[Menyimpan Surat Izin\\nUpdate Status: Izin Diterima]:::process
    end

    subgraph DPMPTSP [Petugas DPMPTSP]
        direction TB
        LoginDPMPTSP[Admin Login & Buka Dokumen]:::process
        CekAwal{Dokumen\\nLengkap?}:::decision
        TolakAwal[Memberi Catatan Penolakan]:::process
        TeruskanDinas[Meneruskan ke Dinas Terkait]:::process
        
        FinalCheck[Cek Rekomendasi Dinas]:::process
        ValidasiAkhir{Rekomendasi\\nValid?}:::decision
        TolakAkhir[Membatalkan Izin]:::process
        UploadIzin[Mengunggah Dokumen Surat Izin]:::process
    end

    subgraph Dinas [Dinas Kesehatan Tim Teknis]
        direction TB
        LoginDinas[Tim Teknis Login & Buka Dokumen]:::process
        CekTeknis{Syarat Teknis\\nMemenuhi?}:::decision
        TolakTeknis[Memberi Catatan Penolakan Teknis]:::process
        UploadRekom[Mengunggah BA & Rekomendasi]:::process
    end

    %% Flow
    Start --> LoginW
    LoginW --> ValidasiLogin
    ValidasiLogin -->|Valid| PilihMenu
    PilihMenu --> IsiForm
    IsiForm --> SubmitData
    SubmitData --> SimpanPermohonan
    SimpanPermohonan --> LihatStatus
    SimpanPermohonan --> LoginDPMPTSP
    
    LoginDPMPTSP --> CekAwal
    CekAwal -->|TIDAK| TolakAwal
    TolakAwal --> LihatStatus
    CekAwal -->|YA| TeruskanDinas
    TeruskanDinas --> UpdateStatusDinas
    
    UpdateStatusDinas --> LoginDinas
    LoginDinas --> CekTeknis
    CekTeknis -->|TIDAK| TolakTeknis
    TolakTeknis --> LihatStatus
    CekTeknis -->|YA| UploadRekom
    UploadRekom --> SimpanRekomendasi
    
    SimpanRekomendasi --> FinalCheck
    FinalCheck --> ValidasiAkhir
    ValidasiAkhir -->|TIDAK| TolakAkhir
    TolakAkhir --> LihatStatus
    ValidasiAkhir -->|YA| UploadIzin
    UploadIzin --> TerbitkanIzinSistem
    
    TerbitkanIzinSistem --> DownloadIzin
    DownloadIzin --> End
```

---
*Diagram ini menggunakan Swimlane flowchart untuk memperlihatkan pembagian tugas antar aktor sesuai standar pemodelan UML formal.*
