# Narasi Media Monorepo & Multi-Repository Architecture

Selamat datang di repositori arsitektur platform **Narasi Media** — platform Social Media Marketing (SMM) dan Digital Media Service generasi berikutnya yang dibangun dengan filosofi **Neo-Fintech**, **Domain-Driven Design (DDD)**, **Test-Driven Development (TDD)**, dan **Deployment-Agnostic** (Cloudflare Workers Free Tier & Standalone VPS).

---

## 📁 Struktur Repositori Independen (Multi-Repo)

Seluruh folder di bawah ini didesain sebagai repositori mandiri yang siap dihubungkan ke GitHub:

| Nama Repositori / Folder | Teknologi | Deskripsi |
| :--- | :--- | :--- |
| **`narasi-media-frontend-web-apps`** | Svelte 4/5 + Tailwind CSS | Aplikasi web portal member client (Dashboard, Pesan Baru, Pesanan Masal, Riwayat, Deposit QRIS). |
| **`narasi-media-backend`** | Rust + Tokio + Axum | Backend performa tinggi dengan arsitektur DDD, port & adapters, serta unit/integration tests lengkap. |
| **`narasi-media-frontend-admin-panel`**| Svelte 4/5 + Tailwind CSS | Dasbor operasional staf/admin untuk verifikasi bukti transfer QRIS manual, pesanan, dan markup harga. |
| **`narasi-media-infra`** | Docker, Caddy, Wrangler (CF) | Konfigurasi infrastruktur Cloudflare Worker (D1, R2, KV) dan VPS Docker Compose multi-container. |
| **`narasi-media-docs`** | Markdown Documentation | Dokumentasi komprehensif mencakup arsitektur, panduan deploy, API specs, dan UI/UX design. |

---

## 🚀 Panduan Memulai Cepat (Quick Start)

### 1. Menjalankan Backend (Rust)
```bash
cd narasi-media-backend

# Menjalankan test suite (TDD)
cargo test

# Menjalankan server HTTP lokal (Port 8080)
cargo run --bin narasi-server
```
*Server otomatis menyuntikkan data demo akun: username `regiami` dengan saldo awal Rp 500.000.*

### 2. Menjalankan Client Web Apps (Svelte)
```bash
cd narasi-media-frontend-web-apps
npm install
npm run dev
```
*Aplikasi web klien akan aktif di `http://localhost:3000`.*

### 3. Menjalankan Admin Panel (Svelte)
```bash
cd narasi-media-frontend-admin-panel
npm install
npm run dev
```
*Admin panel akan aktif di `http://localhost:3001`.*

---

## 📚 Indeks Dokumentasi Lengkap
* 🎨 **[design.md](./design.md)**: Spesifikasi visual UI/UX, design tokens, komponen glassmorphism, dan alur interaktif.
* 🏛️ **[architecture-ddd.md](./architecture-ddd.md)**: Dokumentasi arsitektur Domain-Driven Design (DDD) & Hexagonal.
* ☁️ **[deployment-guide.md](./deployment-guide.md)**: Panduan beralih antara Cloudflare Workers dan VPS.
* 🔌 **[api-specification.md](./api-specification.md)**: Dokumentasi endpoint REST API dan protokol standar SMM v2.
* 💳 **[manual-qris-workflow.md](./manual-qris-workflow.md)**: Alur kerja deposit QRIS manual dan verifikasi staf admin.
