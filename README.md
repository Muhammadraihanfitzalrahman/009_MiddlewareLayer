# 009_MiddlewareLayer

Implementasi arsitektur **Middleware Layer** pada REST API menggunakan **Express.js** dan database **PostgreSQL** untuk pengelolaan data biodata mahasiswa.

Proyek ini dibangun untuk memenuhi tugas praktikum Pengembangan Web Service (PWS) dengan fokus pada pembuatan middleware kustom, validasi skema input, standardisasi respon API (Response Envelope), serta penanganan error terpusat.

---

## 🚀 Fitur & Komponen Middleware

1. **Middleware Logging Request**
   - Mencatat setiap request yang masuk secara otomatis.
   - Menampilkan informasi method HTTP, URL endpoint, status code response, dan durasi eksekusi request (dalam milidetik).

2. **Middleware Body Parsing**
   - Mendukung parsing request berformat JSON (`express.json()`).
   - Mendukung parsing request form-urlencoded (`express.urlencoded()`).

3. **Response Envelope Standardization**
   - Format standar untuk semua respon API:
     - **Respon Sukses:**
       ```json
       {
         "success": true,
         "message": "Deskripsi pesan sukses",
         "data": { ... },
         "error": null
       }
       ```
     - **Respon Error:**
       ```json
       {
         "success": false,
         "message": "Deskripsi pesan error",
         "data": null,
         "error": [ ... ]
       }
       ```

4. **Middleware Validasi Input (Joi Validation)**
   - **`validateBody(schema)`**: Memvalidasi integritas data payload request body sebelum masuk ke controller. Mengembalikan error 400 jika tidak sesuai skema.
   - **`validateParams(schema)`**: Memvalidasi route parameter (misal `:id`) agar bertipe angka integer positif.

5. **Middleware Penanganan Route Tidak Ditemukan (404 Handler)**
   - Menangani route yang tidak terdaftar dengan mengembalikan respon JSON 404 terstandar.

6. **Global Error Handling Middleware**
   - Menangani error tak terduga (internal server error) secara terpusat agar server tetap stabil.

---

## 📡 Endpoint API

| Method | Endpoint | Deskripsi | Middleware Validasi |
| :--- | :--- | :--- | :--- |
| **GET** | `/` | Mengambil seluruh daftar data biodata | - |
| **POST** | `/biodata` | Menambahkan data biodata mahasiswa baru | `validateBody(biodataSchema)` |
| **PUT** | `/biodata/:id` | Memperbarui data biodata berdasarkan ID | `validateParams(idSchema)`, `validateBody(biodataSchema)` |
| **DELETE** | `/biodata/:id` | Menghapus data biodata berdasarkan ID | `validateParams(idSchema)` |

---

## 🛠️ Instalasi & Menjalankan Proyek

### 1. Clone Repositori
```bash
git clone https://github.com/Muhammadraihanfitzalrahman/009_MiddlewareLayer.git
cd 009_MiddlewareLayer
```

### 2. Install Dependensi
```bash
npm install
```

### 3. Konfigurasi Database PostgreSQL
Pastikan PostgreSQL berjalan dan database `mahasiswa` dengan tabel `biodata` telah dibuat:
```sql
CREATE DATABASE mahasiswa;

CREATE TABLE biodata (
    id SERIAL PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    nim VARCHAR(20) NOT NULL,
    kelas VARCHAR(20) NOT NULL
);
```

### 4. Menjalankan Server
```bash
node index.js
```
Server akan berjalan pada: `http://localhost:3000`
