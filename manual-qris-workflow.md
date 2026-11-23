# Alur Kerja Deposit QRIS Manual & Verifikasi Admin

Dokumen ini menguraikan secara rinci siklus hidup fitur **Deposit QRIS Manual** pada platform Narasi Media, yang dirancang tanpa ketergantungan pada pihak ketiga pada tahap awal peluncuran.

---

## 1. Diagram Alur Proses (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor User as Member Klien
    participant WebApp as narasi-media-web-apps
    participant Backend as narasi-media-backend
    participant DB as Basis Data (D1/SQLite)
    actor Admin as Staf Admin Keuangan
    participant AdminPanel as narasi-media-admin-panel

    User->>WebApp: Masukkan Nominal (misal Rp 50.000)
    WebApp->>Backend: POST /api/deposits (nominal=50.000)
    Backend->>Backend: Generate Kode Unik 3 Digit Acak (misal 387)
    Backend->>Backend: Total Bayar = Rp 50.387, Saldo Masuk = Rp 52.500 (+5%)
    Backend->>DB: Simpan Deposit (Status = Pending)
    Backend-->>WebApp: Kembalikan Data Invoice & Barcode QRIS

    WebApp->>User: Tampilkan Barcode QRIS & Instruksi Transfer Tepat Rp 50.387
    User->>User: Scan QRIS via Mobile Banking / E-Wallet (BCA/Dana/GoPay/dll)
    User->>WebApp: Input Nama Pengirim & Upload Foto Struk Bukti Transfer
    WebApp->>Backend: POST /api/deposits/:id/proof
    Backend->>DB: Update Status = WaitingVerification & Simpan Foto
    Backend-->>WebApp: Tampilkan Status Invoice Sedang Diverifikasi

    AdminPanel->>Backend: GET /api/admin/deposits/pending
    Backend-->>AdminPanel: Daftar Deposit Menunggu Verifikasi
    Admin->>AdminPanel: Periksa Foto Struk & Mutasi Rekening
    Admin->>AdminPanel: Klik Tombol "Setujui (Approve)"
    AdminPanel->>Backend: POST /api/admin/deposits/:id/approve
    Backend->>DB: Update Deposit (Status = Success)
    Backend->>DB: Kredit Saldo User (+Rp 52.500)
    Backend->>DB: Catat Entri Baru ke balance_logs
    Backend-->>AdminPanel: Konfirmasi Sukses

    User->>WebApp: Saldo Otomatis Bertambah di Header Web App!
```

---

## 2. Keunggulan Mekanisme Kode Unik 3-Digit
1. **Pembeda Transaksi Identik**: Jika 10 pengguna mentransfer `Rp 50.000` di waktu bersamaan, sistem memberikan kode unik berbeda (misal `Rp 50.123`, `Rp 50.456`, `Rp 50.789`).
2. **Mempermudah Verifikasi Admin**: Admin hanya perlu mencocokkan nominal 3 digit terakhir pada notifikasi mutasi rekening m-banking.
3. **Pemberian Promo Bonus**: Seluruh kode unik tetap menjadi hak pengguna atau ditambahkan bonus promo saldo (misal +5%), memberikan insentif positif bagi pengguna.
