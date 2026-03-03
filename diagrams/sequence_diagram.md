# Sequence Diagram (Standar Skripsi)

## 1. Sequence Diagram: Pengajuan Izin Baru oleh Warga

```mermaid
sequenceDiagram
    autonumber
    actor W as Warga
    participant UI as <<Boundary>>\\nHalaman Ajukan Izin
    participant Control as <<Control>>\\nProses Pengajuan\\n(submit_upload.php)
    participant DB as <<Entity>>\\nDatabase (Tabel documents)
    
    W->>UI: Mengakses halaman "Ajukan Dokumen"
    UI-->>W: Menampilkan formulir permohonan
    W->>UI: Memasukkan data detail dan unggah berkas (KTP, STR, Pas Foto)
    W->>UI: Menekan tombol "Submit"
    
    activate UI
    UI->>Control: POST file dan form data (multipart)
    activate Control
    
    Control->>Control: Validasi ukuran dan ekstensi file
    Control->>Control: Memindahkan file ke folder /uploads/
    Control->>Control: Memformat payload JSON (meta & formdata)
    
    Control->>DB: INSERT INTO documents (user_id, doc_name, data_json, status, uploaded_at)
    activate DB
    DB-->>Control: Berhasil menyimpan (return ID)
    deactivate DB
    
    Control-->>UI: Kirim respon (Status: Success)
    deactivate Control
    
    UI-->>W: Redirect ke Halaman Status Dokumen dengan alert "Berhasil"
    deactivate UI
```

## 2. Sequence Diagram: Verifikasi Awal oleh DPMPTSP

```mermaid
sequenceDiagram
    autonumber
    actor PT as Petugas DPMPTSP
    participant UI_DPT as <<Boundary>>\\nDashboard DPMPTSP
    participant C_View as <<Control>>\\nProses List Dokumen\\n(view_doc_dpmptsp.php)
    participant C_Act as <<Control>>\\nProses Update\\n(dpmptsp_action.php)
    participant DB as <<Entity>>\\nDatabase (Tabel documents)

    PT->>UI_DPT: Membuka Menu "Permohonan Masuk"
    activate UI_DPT
    UI_DPT->>C_View: Permintaan Daftar Dokumen (Status = Menunggu Verifikasi DPMPTSP)
    
    activate C_View
    C_View->>DB: SELECT * FROM documents WHERE status = 'Menunggu Verifikasi DPMPTSP'
    activate DB
    DB-->>C_View: Mengembalikan Kumpulan Record Dokumen
    deactivate DB
    C_View-->>UI_DPT: Kirim data render ke UI
    deactivate C_View
    
    UI_DPT-->>PT: Menampilkan Tabel Data
    
    PT->>UI_DPT: Membuka detail dokumen dan menekan "Teruskan ke Dinas"
    UI_DPT->>C_Act: POST action "forward", document_id, dinas_terkait
    activate C_Act
    
    C_Act->>DB: UPDATE documents SET status='Menunggu Verifikasi Dinas Kesehatan'
    activate DB
    DB-->>C_Act: Sukses update
    deactivate DB
    
    C_Act-->>UI_DPT: Respon pembaruan (Redirect + Pesan Sukses)
    deactivate C_Act
    
    UI_DPT-->>PT: Menampilkan pemberitahuan "Dokumen berhasil diteruskan"
    deactivate UI_DPT
```

## 3. Sequence Diagram: Proses Tim Teknis (Dinas) Mengunggah Rekomendasi

```mermaid
sequenceDiagram
    autonumber
    actor DN as Pegawai Dinas
    participant UID as <<Boundary>>\\nDashboard Dinas Kesehatan
    participant C_Act as <<Control>>\\nVerifikasi Action\\n(verify_action.php)
    participant DB as <<Entity>>\\nDatabase (Tabel documents)

    DN->>UID: Buka daftar permohonan yang diteruskan
    DN->>UID: Pilih Upload BA / Rekomendasi Dinas
    DN->>UID: Unggah file PDF Rekomendasi dan tekan Simpan
    
    activate UID
    UID->>C_Act: POST action "upload_rekomendasi", file_pdf, document_id
    activate C_Act
    
    C_Act->>C_Act: Proses pemindahan file ke path tujuan
    C_Act->>DB: UPDATE documents SET rekomendasi_filename=?, status='Menunggu Konfirmasi DPMPTSP'
    activate DB
    DB-->>C_Act: Sukses update
    deactivate DB
    
    C_Act-->>UID: Berhasil redirect
    deactivate C_Act
    
    UID-->>DN: Tampilkan "Berita Acara Berhasil diunggah"
    deactivate UID
```
