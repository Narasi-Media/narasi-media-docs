# Deployment & Switching Guide: Cloudflare Workers <-> VPS

Buzzer Media dirancang secara **deployment-agnostic**. Kode domain dan aplikasi bisnis murni dipisahkan dari runtime server, memungkinkan Anda memulai dengan **Cloudflare Workers (Free Plan)** dan beralih ke **Standalone VPS (Docker / Caddy)** saat traffic atau volume pesanan meningkat, atau sebaliknya.

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
   wrangler d1 create buzzer-media-prod
   wrangler r2 bucket create buzzer-media-receipts
   ```
3. Salin `database_id` yang dihasilkan ke file `buzzer-media-infra/cloudflare/wrangler.toml`.
4. Jalankan skrip migrasi dan deployment otomatis:
   ```bash
   cd buzzer-media-infra/scripts
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
  * `buzzer.domain.com` $\rightarrow$ Web App Klien
  * `admin.buzzer.domain.com` $\rightarrow$ Admin Panel
  * `api.buzzer.domain.com` $\rightarrow$ Rust Backend

### Langkah-langkah:
1. Masuk ke server VPS Anda dan clone repositori.
2. Edit file `buzzer-media-infra/vps/Caddyfile` dan ganti nama domain dengan domain Anda.
3. Jalankan deployment otomatis:
   ```bash
   cd buzzer-media-infra/scripts
   chmod +x deploy-vps.sh
   ./deploy-vps.sh
   ```
*Caddy akan otomatis mendaftarkan dan memperbarui sertifikat SSL gratis (Let's Encrypt).*

---

## 3. Strategi Beralih (Switching Strategy)

Karena antarmuka HTTP dan skema database kompatibel:
1. **Ekspor Database D1 ke SQLite/PostgreSQL**:
   ```bash
   wrangler d1 export buzzer-media-prod --output=backup.sql
   ```
2. **Ubah Target API pada Frontend**:
   * Buka `buzzer-media-frontend-web-apps/vite.config.js` dan ganti proxy target dari URL Cloudflare Worker ke IP/Domain VPS.
3. Bangun ulang aplikasi frontend:
   ```bash
   npm run build
   ```
Logika bisnis, validasi pesanan, formula harga, dan verifikasi deposit manual QRIS tidak memerlukan perubahan kode apa pun.
