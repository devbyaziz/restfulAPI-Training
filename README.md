# RESTful API Sederhana dengan Node.js

API backend untuk sistem manajemen produk dengan fitur autentikasi pengguna. Cocok untuk project e-commerce sederhana, inventory, atau aplikasi CRUD lainnya.

## Apa yang Bisa Dilakukan API Ini?

- Registrasi pengguna baru
- Login dan mendapatkan token akses
- Melihat profil pengguna
- Tambah, lihat, edit, dan hapus produk
- Setiap produk terhubung dengan pembuat/pemiliknya

## Teknologi yang Digunakan

- Node.js - Platform JavaScript untuk server
- Express.js - Framework untuk membuat API
- SQLite - Database ringan (tidak perlu install MySQL/PostgreSQL)
- Prisma - Tool untuk mengelola database
- JWT - Sistem token untuk keamanan login
- bcryptjs - Enkripsi password

## Struktur Folder

```
restful-api/
├── prisma/
│   ├── schema.prisma       (Struktur database)
│   └── migrations/         (History perubahan database)
├── src/
│   ├── controllers/        (Logic bisnis aplikasi)
│   ├── routes/            (Endpoint API)
│   ├── middleware/        (Keamanan & validasi)
│   ├── utils/            (Helper functions)
│   └── server.js         (File utama aplikasi)
├── .env                  (Konfigurasi rahasia)
└── package.json          (Daftar dependency)
```

## Cara Install dan Menjalankan

### Langkah 1: Install Dependencies
Buka terminal di folder project ini, lalu jalankan:
```bash
npm install
```

### Langkah 2: Jalankan Server
Untuk development (dengan auto-reload ketika ada perubahan code):
```bash
npm run dev
```

Untuk production:
```bash
npm start
```

Server akan berjalan di http://localhost:3000

Catatan: File .env, database, dan migrations sudah dikonfigurasi otomatis saat setup.

## Cara Menggunakan API

### 1. Registrasi Pengguna Baru
Kirim request POST ke: http://localhost:3000/api/auth/register

Data yang dikirim (JSON):
```json
{
  "email": "user@example.com",
  "password": "password123",
  "name": "John Doe"
}
```

Jika berhasil, Anda akan mendapat response berisi data user dan TOKEN. Simpan token ini untuk digunakan di request selanjutnya.

### 2. Login

Kirim request POST ke: http://localhost:3000/api/auth/login

Data yang dikirim (JSON):
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

Anda akan mendapatkan TOKEN yang harus disimpan untuk mengakses fitur yang memerlukan autentikasi.

### 3. Lihat Profil Saya (Perlu Token)

Kirim request GET ke: http://localhost:3000/api/auth/profile

Tambahkan di Header:
```
Authorization: Bearer TOKEN_ANDA_DISINI
```

### 4. Lihat Semua Produk (Tidak Perlu Token)

Kirim request GET ke: http://localhost:3000/api/products

Semua orang bisa melihat daftar produk tanpa login.

### 5. Lihat Detail 1 Produk (Tidak Perlu Token)

Kirim request GET ke: http://localhost:3000/api/products/1

Ganti angka 1 dengan ID produk yang ingin dilihat.

### 6. Tambah Produk Baru (Perlu Token)

Kirim request POST ke: http://localhost:3000/api/products

Header:
```
Authorization: Bearer TOKEN_ANDA_DISINI
Content-Type: application/json
```

Data yang dikirim (JSON):
```json
{
  "name": "Laptop Gaming",
  "description": "ROG Strix G15",
  "price": 15000000,
  "stock": 5
}
```

### 7. Edit Produk (Perlu Token, Hanya Pemilik Produk)

Kirim request PUT ke: http://localhost:3000/api/products/1

Header:
```
Authorization: Bearer TOKEN_ANDA_DISINI
Content-Type: application/json
```

Data yang dikirim (JSON):
```json
{
  "name": "Laptop Gaming Update",
  "price": 14000000,
  "stock": 3
}
```

### 8. Hapus Produk (Perlu Token, Hanya Pemilik Produk)

Kirim request DELETE ke: http://localhost:3000/api/products/1

Header:
```
Authorization: Bearer TOKEN_ANDA_DISINI
```

### 9. Lihat Produk Saya (Perlu Token)

Kirim request GET ke: http://localhost:3000/api/products/user/my-products

Header:
```
Authorization: Bearer TOKEN_ANDA_DISINI
```

## Tool untuk Test API

Anda bisa menggunakan salah satu tool berikut untuk test API:

1. Postman (Download di https://www.postman.com)
2. Thunder Client (Extension VS Code, gratis)
3. Insomnia (Download di https://insomnia.rest)
4. Browser Extension seperti RESTClient

Cara pakai (contoh di Postman):
- Buat request baru
- Pilih method (GET, POST, PUT, DELETE)
- Masukkan URL
- Untuk yang perlu token: klik tab "Headers", tambah key "Authorization" dengan value "Bearer TOKEN_ANDA"
- Untuk kirim data: klik tab "Body", pilih "raw", pilih "JSON", lalu masukkan data JSON

## Struktur Database

Tabel User:
- id (angka, auto increment)
- email (text, harus unik)
- password (text, terenkripsi)
- name (text)
- createdAt (tanggal)
- updatedAt (tanggal)

Tabel Product:
- id (angka, auto increment)
- name (text)
- description (text, opsional)
- price (angka desimal)
- stock (angka, default 0)
- userId (angka, referensi ke User)
- createdAt (tanggal)
- updatedAt (tanggal)

Setiap produk terhubung ke 1 user (pembuat produk).

## Fitur Keamanan

- Password otomatis dienkripsi sebelum disimpan
- Token JWT untuk autentikasi (expired dalam 24 jam)
- Route yang sensitif dilindungi dengan middleware
- CORS enabled untuk akses dari frontend
- Helmet untuk security headers
- Validasi input untuk mencegah error

## Tools Tambahan

Lihat isi database secara visual:
```bash
npx prisma studio
```
Akan membuka browser di http://localhost:5555

Reset database (hati-hati, semua data hilang):
```bash
npx prisma migrate reset
```

## Konfigurasi File .env

File ini berisi konfigurasi rahasia:

```
DATABASE_URL="file:./dev.db"
JWT_SECRET="your-secret-key-change-this-in-production"
PORT=3000
```

## Troubleshooting

Masalah: Server tidak bisa jalan
Solusi: Pastikan port 3000 tidak dipakai aplikasi lain

Masalah: Token invalid/expired
Solusi: Login ulang untuk mendapat token baru

Masalah: Tidak bisa create/edit/delete produk
Solusi: Pastikan sudah login dan memasukkan token di header Authorization

## Dibuat Dengan

Node.js, Express.js, Prisma ORM, SQLite
