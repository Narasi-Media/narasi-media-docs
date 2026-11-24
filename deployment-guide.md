# Deployment & Switching Guide: Cloudflare Workers <-> VPS

Narasi Media dirancang secara **deployment-agnostic**. Kode domain dan aplikasi bisnis murni dipisahkan dari runtime server, memungkinkan Anda memulai dengan **Cloudflare Workers (Free Plan)** dan beralih ke **Standalone VPS (Docker / Caddy)** saat traffic atau volume pesanan meningkat, atau sebaliknya.

---

## 0. Git Branching & Mandatory Production Update Rule (ATURAN UTAMA WORKSPACE)

Ekosistem Narasi Media menerapkan aturan deployment berkelanjutan (**Continuous Deployment**):
* **Default Branch**: `development` adalah basis pengerjaan aktif seluruh repositori.
* **Production Branch**: `production` adalah branch stabil yang selalu siap produksi.
* **Mandatory PROD Sync**:
  * Setiap perubahan, fitur baru, dan perbaikan bug yang telah dibuat di branch `development` **wajib selalu dimerge ke branch `production`**.
  * Setiap kali merge ke `production`, sistem **wajib dideploy langsung ke Production (PROD)** di Cloudflare Workers:
    * `narasi-media-admin`: `https://narasi-media-admin.narasi-media.workers.dev`
    * `narasi-media-web`: `https://narasi-media-web.narasi-media.workers.dev`
    * `narasi-media-api`: `https://narasi-media-api.narasi-media.workers.dev`
  * Tidak boleh ada kode yang tertahan hanya di development. Lingkungan Production harus selalu up-to-date.

---

## 1. Opsi A: Deployment di Cloudflare Workers (Free Tier)

Cloudflare Workers Free Tier menyediakan kuota harian yang cukup untuk tahap peluncuran awal:
* 100.000 requests/hari gratis.
* Database D1: 5GB penyimpanan, 5 juta baris baca/hari gratis.
* Penyimpanan R2: 10GB penyimpanan bukti struk QRIS gratis.

### Langkah-langkah:
1. Pastikan Anda telah menginstal Wrangler CLI:
   ```bash
   npm install -g wrangler
   wrangler login
   ```
2. Buat database D1 dan R2 bucket:
   ```bash
   wrangler d1 create narasi-media-prod
   wrangler r2 bucket create narasi-media-receipts
   ```
3. Salin `database_id` yang dihasilkan ke file `narasi-media-infra/cloudflare/wrangler.toml`.
4. Jalankan skrip migrasi dan deployment otomatis:
   ```bash
   cd narasi-media-infra/scripts
   chmod +x deploy-cloudflare.sh
   ./deploy-cloudflare.sh
   ```

---

## 2. Opsi B: Deployment di Standalone VPS (Docker Compose + Caddy)

Opsi ini direkomendasikan ketika traffic harian melebihi kuota gratis Cloudflare atau saat Anda menginginkan kontrol server mandiri.

### Persyaratan VPS:
* Ubuntu 22.04 LTS / Debian 12 (RAM minimal 1GB, 1 vCPU).
* Docker & Docker Compose telah terpasang.
* Domain / Subdomain telah diarahkan ke IP publik VPS:
  * `narasi.domain.com` $\rightarrow$ Web App Klien
  * `admin.narasi.domain.com` $\rightarrow$ Admin Panel
  * `api.narasi.domain.com` $\rightarrow$ Rust Backend

### Langkah-langkah:
1. Masuk ke server VPS Anda dan clone repositori.
2. Edit file `narasi-media-infra/vps/Caddyfile` dan ganti nama domain dengan domain Anda.
3. Jalankan deployment otomatis:
   ```bash
   cd narasi-media-infra/scripts
   chmod +x deploy-vps.sh
   ./deploy-vps.sh
   ```
*Caddy akan otomatis mendaftarkan dan memperbarui sertifikat SSL gratis (Let's Encrypt).*

---

## 3. Strategi Beralih (Switching Strategy)

Karena antarmuka HTTP dan skema database kompatibel:
1. **Ekspor Database D1 ke SQLite/PostgreSQL**:
   ```bash
   wrangler d1 export narasi-media-prod --output=backup.sql
   ```
2. **Ubah Target API pada Frontend**:
   * Buka `narasi-media-frontend-web-apps/vite.config.js` dan ganti proxy target dari URL Cloudflare Worker ke IP/Domain VPS.
3. Bangun ulang aplikasi frontend:
   ```bash
   npm run build
   ```
Logika bisnis, validasi pesanan, formula harga, dan verifikasi deposit manual QRIS tidak memerlukan perubahan kode apa pun.
