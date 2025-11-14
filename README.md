# 🚀 Setup Laravel 12 + Inertia React.js

Panduan langkah demi langkah untuk menginstal dan menjalankan proyek **Laravel 12** dengan **Inertia.js + React.js**.

---

## ⚙️ Instalasi Awal

```bash
# 1️⃣ Buat project Laravel baru
composer create-project laravel/laravel file-project

# 2️⃣ Masuk ke direktori project
cd file-project

# 3️⃣ Install Laravel Breeze
composer require laravel/breeze --dev

# 4️⃣ Install Breeze dengan React (Inertia.js)
php artisan breeze:install react

# 5️⃣ Install dependensi frontend
npm install

# 6️⃣ Jalankan Vite untuk build frontend
npm run dev

# 7️⃣ Jalankan migrasi database
php artisan migrate
```

---

## 📧 Konfigurasi Mailtrap (untuk pengujian email)

Tambahkan konfigurasi berikut di file `.env`:

```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=xxxx
MAIL_PASSWORD=xxxx
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="noreply@dss-net.com"
MAIL_FROM_NAME="${APP_NAME}"
```

> Gantilah `MAIL_USERNAME` dan `MAIL_PASSWORD` dengan kredensial Mailtrap Anda.

---

## 🚀 Jalankan Server

```bash
php artisan serve
```

Akses aplikasi di: 👉 **http://127.0.0.1:8000**

---

## 📂 Struktur Dasar Folder

```
file-project/
├── app/
├── bootstrap/
├── config/
├── database/
├── public/
├── resources/
│   ├── js/
│   │   ├── Pages/
│   │   ├── Components/
│   │   └── app.jsx
│   └── css/
├── routes/
│   ├── web.php
│   └── api.php
└── vite.config.js
```

## Bersih-bersih
- php artisan optimize:clear
- php artisan config:clear
- php artisan route:clear
- php artisan view:clear
- php artisan cache:clear
- php artisan clear-compiled
- composer dump-autoload

---

## 🧩 Catatan Tambahan

- Pastikan Node.js versi **>=18** dan PHP versi **>=8.2**
- Gunakan perintah `npm run build` untuk produksi
- Anda dapat menambahkan autentikasi, dashboard, atau API tambahan setelah setup ini

---

## 👨‍💻 Kontributor

Developed by **[Teds]**  
💡 Feel free to fork and contribute!

---

### 🛠 Lisensi
Proyek ini menggunakan lisensi [MIT](https://opensource.org/licenses/MIT).
