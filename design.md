# Buzzer Media UI/UX Design Specification (`design.md`)
*Non-AI-Slop Architecture: Linear & Shadcn-Svelte Minimalist Design System*

---

## 1. Filosofi Desain: Anti-"AI Slop"

Platform **Buzzer Media** mengadopsi standar desain produk kelas atas yang terinspirasi dari produk rekayasa perangkat lunak modern terbaik dunia seperti **Linear.app**, **Vercel**, **Supabase**, dan **shadcn-svelte**.

### Mengapa Desain Ini Berbeda dari Template AI Biasa ("Anti AI Slop")?
1. **Tidak Ada Gradien Ungu/Pink Klise**: Menghilangkan gradien ungu jenuh (*cliché AI gradient*) yang sering dihasilkan secara instan oleh model AI.
2. **True Neutral Palette (Zinc / Matte Black)**: Menggunakan skala warna netral murni Zinc (`#09090b` untuk dark mode, `#fafafa` untuk white mode).
3. **1px Micro-Border Architecture**: Menggunakan garis pemisah 1px yang tajam (`border-zinc-800` pada dark mode dan `border-zinc-200` pada light mode), bukan efek blur tebal yang mengaburkan teks.
4. **Kepadatan Informasi Tinggi (Data Density)**: Setiap tabel, metrik KPI, dan form dirancang padat, rapi, dan efisien seperti dashboard finansial profesional (Stripe / Retool).
5. **Tipografi Presisi Swiss**: Menggunakan **Inter** untuk keterbacaan antarmuka digital dan **JetBrains Mono** untuk angka nominal harga dan kode unik QRIS.

---

## 2. Sistem Warna & Tokens (Zinc Design Tokens)

| Token | Dark Mode (`#09090b`) | Light / White Mode (`#fafafa`) | Fungsi |
| :--- | :--- | :--- | :--- |
| `background` | `#09090b` (Matte Zinc-950) | `#fafafa` (Paper Zinc-50) | Kanvas utama aplikasi |
| `card` | `#121215` (Deep Zinc-900) | `#ffffff` (Pure White) | Kontainer kartu, modal, dan tabel |
| `border` | `#27272a` (Zinc-800) | `#e4e4e7` (Zinc-200) | Garis pemisah 1px antar elemen |
| `inset` | `#18181b` (Zinc-900) | `#f4f4f5` (Zinc-100) | Panel kalkulator, box instruksi, badge |
| `text-primary`| `#f4f4f5` (Zinc-100) | `#09090b` (Zinc-950) | Judul utama, nominal saldo, teks penting |
| `text-muted` | `#71717a` (Zinc-500) | `#71717a` (Zinc-500) | Label form, deskripsi layanan, timestamp |
| `accent-primary`| `#f4f4f5` (Solid White) | `#09090b` (Solid Black) | Tombol aksi primer (CTA) berdaya kontras tinggi |
| `indicator-green`| `#10b981` (Emerald-500) | `#059669` (Emerald-600) | Status sukses, saldo bertambah |
| `indicator-amber`| `#f59e0b` (Amber-500) | `#d97706` (Amber-600) | Kode unik 3 digit, status pending |
| `indicator-rose`| `#f43f5e` (Rose-500) | `#e11d48` (Rose-600) | Peringatan saldo kurang, status error |

---

## 3. Komponen Antarmuka & Micro-Interactions

### A. Navigation Bar (Linear Style)
* Logo minimalis berbentuk poligon geometris dengan badge versi monospaced `v2.0`.
* Segmented control tab navigasi (`bg-zinc-100 dark:bg-zinc-900`) dengan tab aktif berlatar kontras.
* Pill saldo padat dengan tombol cepat `+ Top Up` dan tombol pengalih tema (Sun ☀️ / Moon 🌙).

### B. Formulir Pemesanan Baru
* **Kategori Platform**: Tombol chip outline minimalis. Saat dipilih, chip berubah menjadi solid black (light mode) atau solid white (dark mode).
* **Kalkulator Harga Inset**: Kotak inset abu-abu netral yang menghitung biaya per 1.000 secara *real-time*.
* **Tombol Submit Solid**: Menggunakan kontras mutlak khas Vercel (Hitam pekat di mode terang, putih bersih di mode gelap).

### C. Tabel Riwayat Pesanan (High Density)
* Header tabel abu-abu netral dengan tipografi font-medium ringkas.
* Status badge menggunakan titik status minimalis (`w-1.5 h-1.5 rounded-full`) yang elegan.
* Tombol aksi `Refill` outline minimalis.

### D. Modal Deposit QRIS (Stripe-Invoice Style)
* Menampilkan nominal bersih dan 3-digit kode unik tanpa ornamen berlebih.
* Barcode QRIS diapit dalam frame putih bersih dengan kode NMID standar nasional.
* Dropzone unggah foto struk yang rapi dan ringkas.

### E. Admin Command Center
* Antrean verifikasi keuangan padat dengan tombol aksi instan **Setujui** dan **Tolak**.
* Lightbox foto struk transfer yang bersih dan responsif.
