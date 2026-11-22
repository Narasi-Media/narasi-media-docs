# API Specification & SMM v2 Protocol Reference

Dokumen ini memuat spesifikasi antarmuka pemrograman aplikasi (REST API) **Buzzer Media**, serta format integrasi protokol standar **SMM v2 API** ke provider hulu.

---

## 1. Endpoints Klien & Publik

### A. Health Check
* **Endpoint**: `GET /health`
* **Response**:
```json
{
  "success": true,
  "message": "Healthy",
  "data": "Buzzer Media Backend is running"
}
```

### B. Dashboard Pengguna
* **Endpoint**: `GET /api/dashboard/:user_id`
* **Response**:
```json
{
  "success": true,
  "message": "Stats retrieved",
  "data": {
    "balance": 250000,
    "balance_formatted": "Rp 250.000",
    "total_orders": 14,
    "total_spent": 1250000,
    "total_deposit": 1500000,
    "username": "regiami",
    "full_name": "Regi Ami"
  }
}
```

### C. Daftar Kategori & Layanan
* **Endpoint**: `GET /api/categories`
* **Endpoint**: `GET /api/services`
* **Response**:
```json
{
  "success": true,
  "data": [
    {
      "id": "SRV-101",
      "category_id": "cat-ig",
      "name": "Instagram Followers REAL Indonesia [Maks 50K] [Garansi 30 Hari]",
      "price_user": { "amount": 15500 },
      "min_order": 100,
      "max_order": 50000,
      "refill_supported": true,
      "refill_days": 30,
      "description": "Akun real aktif Indonesia. Kecepatan 1.000 - 5.000 / hari.",
      "is_active": true
    }
  ]
}
```

### D. Pembuatan Pesanan Baru (Single Order)
* **Endpoint**: `POST /api/orders`
* **Request Body**:
```json
{
  "user_id": "regiami",
  "service_id": "SRV-101",
  "target": "https://instagram.com/akun_target",
  "quantity": 1000,
  "custom_comments": null
}
```
* **Response (HTTP 201)**:
```json
{
  "success": true,
  "message": "Pesanan berhasil dibuat",
  "data": {
    "id": "c864459f-d312-4c57-8e62-c1c4f4efae28",
    "provider_order_id": "SMM-4A2B1C9D",
    "status": "processing",
    "quantity": 1000,
    "total_price": { "amount": 15500 }
  }
}
```

### E. Pembuatan Pesanan Masal (Mass Order)
* **Endpoint**: `POST /api/orders/mass`
* **Request Body**:
```json
{
  "user_id": "regiami",
  "orders_raw": "SRV-101|https://instagram.com/target1|1000\nSRV-102|https://instagram.com/p/123|500"
}
```

### F. Pengajuan Deposit Saldo (Manual QRIS)
* **Endpoint**: `POST /api/deposits`
* **Request Body**:
```json
{
  "user_id": "regiami",
  "method": "manual_qris",
  "amount": 50000
}
```
* **Response (HTTP 201)**:
```json
{
  "success": true,
  "message": "Tiket deposit berhasil dibuat",
  "data": {
    "id": "4fc73e86-1d17-48f5-a3d8-2b8109bf1a42",
    "requested_amount": { "amount": 50000 },
    "unique_code": 387,
    "total_to_pay": { "amount": 50387 },
    "balance_credited": { "amount": 52500 },
    "status": "pending"
  }
}
```

### G. Unggah Bukti Pembayaran Struk QRIS
* **Endpoint**: `POST /api/deposits/:deposit_id/proof`
* **Request Body**:
```json
{
  "sender_name": "Budi Santoso (BCA QRIS)",
  "proof_image_url": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
}
```

---

## 2. Endpoints Admin Command Center

### A. Daftar Deposit Menunggu Verifikasi
* **Endpoint**: `GET /api/admin/deposits/pending`

### B. Menyetujui Deposit (Approve)
* **Endpoint**: `POST /api/admin/deposits/:deposit_id/approve`
* *Mengkredit saldo pengguna secara otomatis dan mencatat log mutasi.*

### C. Menolak Deposit (Reject)
* **Endpoint**: `POST /api/admin/deposits/:deposit_id/reject`
* **Request Body**:
```json
{
  "reason": "Dana belum masuk ke mutasi rekening."
}
```

---

## 3. Protokol Standar SMM Provider API (v2)

Buzzer Media berkomunikasi dengan provider SMM hulu menggunakan spesifikasi standar industri:

### Order Dispatch
```http
POST https://provider-domain.com/api/v2
Content-Type: application/x-www-form-urlencoded

key=PROVIDER_API_KEY&action=add&service=101&link=https://instagram.com/target&quantity=1000
```

### Status Check
```http
POST https://provider-domain.com/api/v2
Content-Type: application/x-www-form-urlencoded

key=PROVIDER_API_KEY&action=status&order=12345
```

### Refill Trigger
```http
POST https://provider-domain.com/api/v2
Content-Type: application/x-www-form-urlencoded

key=PROVIDER_API_KEY&action=refill&order=12345
```
