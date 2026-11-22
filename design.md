# Buzzer Media UI/UX Design Specification (`design.md`)
*Evolusi Desain Neo-Fintech & Adaptive Dual-Theme (Dark & Light Mode)*

---

## 1. Visi Desain & Tipografi Baru

**Buzzer Media** mengadopsi standar visual **Neo-Fintech & Cyber-Minimalism** yang terinspirasi dari aplikasi finansial global modern (seperti Linear, Stripe, dan Revolut). Sistem antarmuka dirancang **Adaptive Dual-Theme**: berawal secara default dalam **Dark Mode (Midnight Obsidian)** yang futuristik, dan dapat beralih secara instan ke **Light Mode (Clean Slate / White Mode)** yang terang, bersih, dan profesional.

### Pilihan Font Style & Tipografi
* **Primary Sans-Serif**: **Plus Jakarta Sans** (Google Fonts: 400, 500, 600, 700, 800)
  * Font geometris modern dengan sudut halus (*humanist touch*) yang memberikan keterbacaan tinggi pada teks navigasi, kartu layanan, dan formulir input.
* **Monospace & Numerical**: **JetBrains Mono**
  * Digunakan untuk semua angka nominal saldo, harga pesanan, kode unik deposit, dan format syntax pesanan masal dengan fitur `tabular-nums` untuk perataan vertikal yang presisi.

---

## 2. Adaptive Dual-Theme Design Tokens (Dark & Light Mode)

Sistem token warna dirancang dengan kontras rasio minimal **4.5:1 (WCAG AA Standard)** pada kedua mode:

| Token Name | Dark Mode (Default) | Light / White Mode | Kegunaan |
| :--- | :--- | :--- | :--- |
| **Canvas Background** | `#0B0F19` (Midnight Obsidian) | `#F8FAFC` (Slate Porcelain) | Latar belakang halaman aplikasi |
| **Surface Card** | `rgba(17, 24, 39, 0.75)` | `#FFFFFF` (Pure White) | Kontainer kartu, modal, dan formulir |
| **Card Border** | `rgba(255, 255, 255, 0.08)` | `#E2E8F0` (Border Slate-200) | Garis pemisah kartu & grid |
| **Header Surface** | `rgba(11, 15, 25, 0.85)` | `rgba(255, 255, 255, 0.90)` | Sticky top header dengan backdrop blur |
| **Text Primary** | `#F9FAFB` (Slate-50) | `#0F172A` (Slate-900) | Judul header, angka metrik, teks utama |
| **Text Secondary** | `#9CA3AF` (Gray-400) | `#64748B` (Slate-500) | Sub-judul, keterangan spek, label form |
| **Text Muted** | `#6B7280` (Gray-500) | `#94A3B8` (Slate-400) | Waktu relatif, placeholder, hint |
| **Brand Primary** | `#6366F1` (Indigo-500) | `#4F46E5` (Indigo-600) | Warna tombol aksi utama (CTA), active tab |
| **Brand Primary Hover**| `#4F46E5` (Indigo-600) | `#4338CA` (Indigo-700) | State hover tombol primer |
| **Success Emerald** | `#10B981` (Emerald-500) | `#059669` (Emerald-600) | Saldo akun, status Success, tombol bayar |
| **Warning Amber** | `#F59E0B` (Amber-500) | `#D97706` (Amber-600) | Kode unik 3 digit, status Pending |
| **Danger Rose** | `#F43F5E` (Rose-500) | `#E11D48` (Rose-600) | Peringatan saldo kurang, status Error |
| **Input Background** | `#111827` (Gray-900) | `#F1F5F9` (Slate-100) | Input field data target, select, textarea |

---

## 3. Fitur Theme Switcher (Dark $\leftrightarrow$ White Mode)

* **Tombol Pengalih di Header**:
  * Ikon dinamis: Ikon **Bulan (Moon)** saat mode terang aktif, dan Ikon **Matahari (Sun)** saat mode gelap aktif.
  * Dilengkapi animasi rotasi mikro halus (`rotate-180 transition-transform duration-300`).
* **Penyimpanan Preferensi**:
  * Nilai tema disimpan secara persisten di `localStorage.theme` (`'dark'` atau `'light'`).
  * Jika pengguna belum memilih, sistem otomatis memprioritaskan mode gelap (`dark`) sebagai nilai bawaan.

---

## 4. UI/UX Spesifikasi: Client Web Apps

### A. Navigation & Top Header
* **Brand Logo**: Logo gradien ungu-indigo dengan indikator status server `v2.0` yang berdenyut (*pulsing dot*).
* **Saldo Widget Interaktif**:
  * Menampilkan saldo dengan format Rupiah `tabular-nums` yang tebal.
  * Tombol cepat `+ Top Up` yang memicu modal QRIS.
* **Theme Toggle Switch**: Tombol 1-klik untuk beralih antara Dark Mode dan White Mode tanpa me-reload halaman.
* **Profil Pengguna**: Avatar singkatan username dengan badge status "VIP Member".

### B. Dashboard Overview
* **Pengumuman Sistem**: Kartu pengumuman dengan border samping indigo yang memuat update kecepatan server dan promo bonus deposit.
* **Grid 4 Kartu KPI**:
  1. *Sisa Saldo*: Nominal saldo dengan tombol shortcut top-up.
  2. *Total Pesanan*: Akumulasi jumlah pesanan sukses.
  3. *Total Pengeluaran*: Akumulasi belanja seumur hidup.
  4. *Total Deposit*: Akumulasi deposit yang berhasil masuk.

### C. Alur Pemesanan Baru (Single Order)
1. **Pemilihan Kategori**: Tombol pil kategori (Instagram, TikTok, YouTube, Twitter/X) dengan transisi highlight halus.
2. **Dropdown Layanan**: Menampilkan ID layanan, nama lengkap, harga per 1.000, batas min/max, dan garansi refill.
3. **Kartu Informasi Spesifikasi Layanan**: Menampilkan estimasi kecepatan dan instruksi format link.
4. **Input Target & Jumlah**: Input kuota pesanan dengan **Kalkulator Harga Real-time**:
   $$\text{Total Bayar} = \left(\frac{\text{Jumlah}}{1.000}\right) \times \text{HargaPerK}$$
5. **Validasi Saldo Instan**: Menampilkan peringatan merah halus jika saldo pengguna kurang dari estimasi total bayar.

### D. Alur Pesanan Masal (Mass Order)
* Textarea font monospace dengan nomor baris otomatis.
* Syntax standar: `ID_Layanan|Target_Link|Jumlah`.
* Ringkasan hasil batch memisahkan pesanan yang sukses dan baris yang gagal dengan alasan error yang jelas.

### E. Riwayat Pesanan & Refill Garansi
* Filter status: *Semua, Pending, Processing, Success, Partial, Error*.
* Kolom DataGrid lengkap: ID Pesanan, Waktu Transaksi, Layanan, Target Link, Jumlah/Sisa, Harga Total, Status Badge berwarna, dan Tombol `🛡️ Refill`.

### F. Modal Deposit QRIS Manual
1. **Pemilihan Nominal**: Tombol cepat Rp 20.000 s/d Rp 500.000 dengan preview bonus promo +5%.
2. **Barcode QRIS Resmi**: Render QRIS vector resolusi tinggi dengan kode unik 3 digit acak (misal Rp 50.387).
3. **Upload Bukti Struk**: Form nama pengirim rekening dan upload file foto dengan preview gambar instan.
4. **Status Antrean**: Feedback status bahwa invoice sedang diverifikasi oleh admin.

---

## 5. UI/UX Spesifikasi: Admin Command Center

* **Antrean Verifikasi Deposit**: Tabel deposit pending dengan nominal yang ditagih, kode unik, nama pengirim, dan thumbnail bukti transfer.
* **Modal Lightbox**: Menampilkan foto struk bukti transfer ukuran penuh untuk verifikasi visual admin.
* **Aksi Cepat 1-Klik**:
  * Tombol **Setujui (Approve)**: Otomatis mengkredit saldo pengguna secara instan dan mencatat ke log mutasi.
  * Tombol **Tolak (Reject)**: Menolak invoice dengan alasan tertentu.
