# Activity Diagram

```mermaid
activityDiagram
    %%{init: {'theme': 'base'}}%%
    stateDiagram-v2
        [*] --> Warga_Login
        Warga_Login --> Warga_Dashboard: Login Sukses
        Warga_Dashboard --> Upload_Dokumen: Pilih Jenis Izin & Upload Syarat
        Upload_Dokumen --> Menunggu_Verifikasi_DPMPTSP: Submit Data
        
        Menunggu_Verifikasi_DPMPTSP --> DPMPTSP_Review: Petugas DPMPTSP Memeriksa
        
        state DPMPTSP_Review {
            [*] --> Cek_Kelengkapan
            Cek_Kelengkapan --> Ditolak_DPMPTSP: Tidak Lengkap
            Cek_Kelengkapan --> Diteruskan_Kesehatan: Lengkap
        }
        
        Ditolak_DPMPTSP --> Warga_Dashboard: Notifikasi Penolakan
        
        Diteruskan_Kesehatan --> Menunggu_Verifikasi_Dinas: Status Berubah
        Menunggu_Verifikasi_Dinas --> Dinas_Kesehatan_Review: Dinas Kesehatan Memeriksa
        
        state Dinas_Kesehatan_Review {
            [*] --> Tinjau_Berkas
            Tinjau_Berkas --> Tolak_Berkas: Syarat Teknis Tidak Memenuhi
            Tinjau_Berkas --> Buat_Rekomendasi: Syarat Teknis Memenuhi
            Buat_Rekomendasi --> Upload_BA_Rekomendasi
        }
        
        Tolak_Berkas --> Warga_Dashboard: Notifikasi Penolakan dari Dinas
        Upload_BA_Rekomendasi --> Menunggu_Konfirmasi_DPMPTSP: Status Berubah
        
        Menunggu_Konfirmasi_DPMPTSP --> DPMPTSP_Final_Review: Petugas DPMPTSP Memproses Izin
        
        state DPMPTSP_Final_Review {
            [*] --> Cek_Rekomendasi
            Cek_Rekomendasi --> Terbitkan_Izin: Valid
            Terbitkan_Izin --> Upload_Surat_Izin
        }
        
        Upload_Surat_Izin --> Izin_Diterima: Status Berubah
        Izin_Diterima --> Warga_Dashboard: Warga Download Izin
        Izin_Diterima --> [*]
```
