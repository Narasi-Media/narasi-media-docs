# Buzzer Media UI/UX Design Specification (`design.md`)

## 1. Executive Summary & Design Vision

**Buzzer Media** adalah evolusi generasi berikutnya dari platform Social Media Marketing (SMM) dan Buzzer Service di Indonesia. Menggantikan antarmuka monolitik klasik yang kaku, Buzzer Media dirancang dengan filosofi **Neo-Fintech & Cyber-Minimalism**: antarmuka sekelas aplikasi perbankan modern yang cepat, ultra-responsif, transparan, dan memberikan rasa aman bagi pengguna pemula maupun agen/reseller skala besar.

Platform ini terbagi menjadi dua produk antarmuka utama:
1. **Client Web Apps (`buzzer-media-frontend-web-apps`)**: Portal pesanan, top-up saldo, pelacakan real-time, dan manajemen akun member.
2. **Admin Command Center (`buzzer-media-frontend-admin-panel`)**: Dasbor eksekutif untuk verifikasi deposit manual QRIS, manajemen provider SMM upstream, pengaturan margin harga, dan kontrol operasional.

---

## 2. Design Tokens & Design System

### A. Palet Warna (Color Palette)

| Token Name | Hex Code | Deskripsi & Kegunaan |
| :--- | :--- | :--- |
| `primary-600` | `#4F46E5` | Royal Indigo: Warna aksi utama, CTA primer, header aksen |
| `primary-500` | `#6366F1` | Indigo Terang: Status hover, link aktif, gradient stops |
| `secondary-500` | `#7C3AED` | Royal Violet: Aksen sekunder, banner promo, badge reseller |
| `accent-emerald` | `#10B981` | Emerald Success: Status Success/Completed, saldo positif, verifikasi |
| `accent-amber` | `#F59E0B` | Warning Amber: Status Pending, Processing, kode unik transfer |
| `accent-rose` | `#F43F5E` | Danger Rose: Status Error/Canceled, validasi gagal, tombol reject |
| `accent-cyan` | `#06B6D4` | Info Cyan: Status In-Progress, tooltip, informasi kecepatan server |
| `bg-dark` | `#0B0F19` | Deep Midnight: Latar belakang kanvas mode gelap |
| `surface-dark` | `#111827` | Slate Dark Surface: Kartu kontainer, modal, sidebar |
| `surface-border`| `rgba(255, 255, 255, 0.08)` | Border halus untuk efek glassmorphism |
| `text-primary` | `#F9FAFB` | Teks judul utama dan angka metrik penting |
| `text-secondary`| `#9CA3AF` | Teks keterangan, label form, deskripsi layanan |

### B. Tipografi & Skala Teks
* **Font Family**: Inter, Plus Jakarta Sans, atau Segoe UI. Angka menggunakan fitur font `tabular-nums` untuk perataan nominal harga yang presisi.
* **Skala**:
  * Heading 1 (Hero / Metrics): `2.25rem` (36px), font-weight 700.
  * Heading 2 (Card Title): `1.5rem` (24px), font-weight 600.
  * Heading 3 (Section Header): `1.125rem` (18px), font-weight 600.
  * Body Text: `0.875rem` (14px), font-weight 400 & 500.
  * Small / Badges: `0.75rem` (12px), font-weight 600, uppercase tracking.

### C. Elevasi & Efek Visual
* **Glassmorphism**: `backdrop-blur-md bg-gray-900/60 border border-white/10`.
* **Micro-interactions**: Efek transisi halus (`transition-all duration-200 ease-out`), tombol hover dengan sedikit peningkatan elevasi (`shadow-lg shadow-indigo-500/25`), dan skeleton loaders saat pengambilan data async.

---

## 3. UI/UX Spesifikasi: Client Web Apps

### A. Navigation & Top Header
* **Sticky Navigation Bar**:
  * Logo **Buzzer Media** dengan badge indikator server "Live".
  * **Saldo Widget Interaktif**: Menampilkan saldo format mata uang rupiah (`Rp 250.000`) dengan tombol cepat `+ Top Up` yang memicu modal deposit.
  * Navigasi Utama: *Dashboard*, *Pesan Baru*, *Pesanan Masal*, *Riwayat*, *Katalog Layanan*, *Deposit*, *Tiket*.
  * User Avatar & Dropdown: *Pengaturan Akun*, *Mutasi Saldo (Ledger)*, *Keluar*.

### B. Dashboard Overview
1. **Financial & Order Summary Metrics**:
   * *Sisa Saldo*: Nominal besar dengan persentase pertumbuhan bulanan.
   * *Total Pesanan*: Lifetime counter pesanan sukses.
   * *Pengeluaran Bulan Ini*: Total akumulasi biaya order.
   * *Tiket Aktif*: Indikator tiket yang sedang direspon admin.
2. **Interactive Spending Chart**:
   * Grafik garis interaktif 30 hari terakhir (Pengeluaran vs Jumlah Pesanan).
3. **Papan Pengumuman & Rekomendasi Layanan**:
   * Carousel kartu berita: Layanan terbaik hari ini (kecepatan kilat), diskon harga, dan catatan server maintenance.

### C. Alur Pesanan Baru (Single Order UX)
Formulir pesanan baru didesain bertingkat (*progressive disclosure*) untuk mencegah kesalahan input:
1. **Langkah 1: Pemilihan Kategori**:
   * Grid tombol kategori berlogo sosial media (Instagram, TikTok, YouTube, Twitter/X, Facebook, Telegram, Google Review).
   * Kategori yang dipilih memicu animasi filter instan.
2. **Langkah 2: Pemilihan Layanan**:
   * Dropdown searchable yang menampilkan: `[ID] Nama Layanan | Rp [Harga/1K] | [Badge Garansi]`.
   * Setelah layanan dipilih, muncul **Kartu Informasi Spek Layanan**:
     * Minimal Pesan: e.g. 100
     * Maksimal Pesan: e.g. 500.000
     * Kecepatan: Rata-rata 1.000 - 5.000 / jam
     * Garansi: 30 Hari Refill Guarantee (atau Tanpa Garansi)
     * Deskripsi lengkap tata cara input link (misal: "Harus akun publik, bukan private").
3. **Langkah 3: Input Target & Kalkulator Harga**:
   * Input `Target`: Otomatis memvalidasi format link sesuai platform (misal regex URL instagram).
   * Input `Jumlah`: Slider atau input angka.
   * Input `Komentar Kustom` (kondisional jika layanan komentar): Textarea baris-per-baris dengan kalkulator baris otomatis.
   * **Real-time Price Preview Box**:
     $$\text{Total Bayar} = \left(\frac{\text{Jumlah}}{1.000}\right) \times \text{Harga per 1K}$$
   * Indikator validasi saldo: Menampilkan peringatan jika `Saldo < Total Bayar` dengan CTA tombol cepat top-up.

### D. Alur Pesanan Masal (Mass Order UX)
* Textarea lebar monospace dengan nomor baris otomatis.
* Format: `ID_Layanan|Target_URL|Jumlah`.
* **Live Syntax Checker**: Memvalidasi tiap baris secara real-time. Jika baris 3 salah format, diberikan indikator highlight merah dengan penjelasan error sebelum user menekan tombol submit.
* Ringkasan Total Akumulasi Biaya sebelum konfirmasi.

### E. Alur Riwayat Pesanan (Order History UX)
* Tab Filter Cepat: *Semua*, *Pending*, *Processing*, *Success*, *Partial*, *Canceled/Error*.
* Kolom DataGrid Lengkap:
  * ID Pesanan (dengan tombol klik untuk salin).
  * Waktu transaksi (format waktu relatif: *2 menit lalu* / absolut).
  * Layanan (dengan logo platform).
  * Target (klik untuk membuka link di tab baru).
  * Jumlah, Jumlah Awal (*Start Count*), Sisa (*Remains*).
  * Harga Total.
  * Status Badge berwarna.
  * **Tombol Aksi**:
    * Tombol *Refill* (aktif jika layanan mendukung refill dan status *Success*).
    * Tombol *Bantuan* (langsung membuka form tiket dengan ID pesanan otomatis terisi).

### F. Alur Deposit (Manual QRIS & Payment Gateway)
* **Pilihan 1: Manual QRIS (Prioritas Pertama)**:
  1. Pengguna memasukkan nominal deposit (misal `Rp 50.000`).
  2. Sistem menghasilkan **Kode Unik 3 Digit Acak** (misal `Rp 50.387`) untuk identifikasi.
  3. Menampilkan **Gambar Barcode QRIS Resmi Buzzer Media** yang jelas dan dapat diunduh / di-scan langsung dari aplikasi m-banking atau e-wallet (BCA, Mandiri, Dana, GoPay, OVO, ShopeePay).
  4. Instruksi pembayaran jelas: "Transfer tepat hingga 3 digit terakhir!".
  5. Formulir Konfirmasi & Upload Bukti:
     * Nama Pengirim Rekening / Akun E-Wallet.
     * Drag-and-drop / upload file foto struk/screenshot pembayaran (preview gambar instan).
     * Tombol "Kirim Bukti Pembayaran".
  6. Status Invoice: Tampilan status invoice real-time (*Menunggu Pembayaran* $\rightarrow$ *Menunggu Verifikasi Admin* $\rightarrow$ *Saldo Berhasil Masuk*).
* **Pilihan 2: Otomatis (Instant Payment Gateway - TriPay / Xendit)**:
  * Opsi Virtual Account (BCA, Mandiri, BNI, BRI) dengan nomor VA yang dapat disalin 1-klik.

---

## 4. UI/UX Spesifikasi: Admin Command Center

### A. Executive Overview
* **KPI Header Cards**: Total Omset Hari Ini (GMV), Estimasi Margin Laba Bersih, Antrean Deposit Menunggu Verifikasi, dan Saldo Akun Provider Utama.
* **Server Health Monitor**: Status koneksi API ke upstream SMM providers (Latency, Error Rate, Quota).

### B. Verifikasi Deposit Manual (Manual QRIS Approvals)
Halaman kerja utama staf keuangan admin:
* **Tabel Antrean Verifikasi**:
  * ID Deposit, Tanggal & Jam, Username Member, Nominal Transfer, Kode Unik, Total Ditagih, Nama Pengirim.
  * Thumbnail Bukti Transfer: Klik untuk membuka modal lightbox gambar resolusi tinggi untuk memverifikasi keaslian bukti transfer.
  * Tombol Aksi Cepat:
    * **Setujui (Approve)**: Memicu SweetAlert konfirmasi $\rightarrow$ Saldo pengguna otomatis bertambah $\rightarrow$ Status berubah menjadi `success` $\rightarrow$ Pencatatan otomatis ke `balance_logs`.
    * **Tolak (Reject)**: Menampilkan pilihan alasan penolakan (misal: "Struk tidak valid", "Dana belum masuk mutasi", "Nominal tidak sesuai").

### C. Manajemen Pesanan & Sinkronisasi
* Tabel seluruh pesanan lintas pengguna.
* Kemampuan melakukan *Manual Status Override* jika provider mengalami kendala.
* Fitur *Auto-Refund Calculator* untuk pesanan berstatus *Partial*: Sistem menghitung sisa kuota yang belum selesai dan mengembalikan proporsi saldo yang adil secara instan ke akun pengguna.

### D. Manajemen Layanan & Markup Margin Harga
* Konfigurasi integrasi SMM Provider API (URL API & API Key).
* Tombol *Sync Catalog from Provider*: Mengimpor ribuan layanan langsung dari server penyedia.
* **Markup Pricing Engine**:
  * Rule global: Tambahkan profit margin (misal: markup +25% dari harga asli modal provider).
  * Custom markup per kategori atau per layanan spesifik.
  * Tombol sakelar cepat (*Enable / Disable Service*).

### E. Pengaturan QRIS & Rekening Manual
* Formulir upload gambar QRIS resmi (PNG/JPG/SVG).
* Pengaturan daftar rekening bank tujuan manual (BCA, Mandiri, BRI, BNI).
* Pengaturan persentase promo bonus saldo (misal: "Bonus Deposit 5%").

---

## 5. Responsivitas & Desain Mobile-First

* Seluruh komponen dirancang menggunakan kisi fleksibel CSS Grid dan Flexbox.
* Pada layar mobile (< 768px):
  * Sidebar bertransformasi menjadi Bottom Navigation Bar untuk navigasi satu tangan yang nyaman.
  * Tabel DataGrid bertransformasi menjadi format Card Stack yang rapi dan mudah di-scroll.
  * QRIS barcode otomatis memenuhi lebar layar agar mudah di-screenshot atau di-scan ponsel lain.
