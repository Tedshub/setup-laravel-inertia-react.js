# 🚀 Setup Laravel 12 + Inertia.js + React + TypeScript (PowerShell)

Panduan instalasi dan menjalankan proyek **Laravel 12** menggunakan **Inertia.js + React + TypeScript** di **Windows PowerShell**.

---

# 📋 Persyaratan

Pastikan telah menginstal:

- PHP >= 8.2
- Composer
- Node.js >= 20 (disarankan LTS)
- Git
- Database (MySQL/PostgreSQL/SQLite)

Cek versi:

```powershell
php -v
composer -V
node -v
npm -v
git --version
```

---

# ⚙️ Membuat Project Baru

```powershell
# Membuat project Laravel
composer create-project laravel/laravel file-project

# Masuk ke folder project
cd file-project
```

---

# ⚙️ Install Breeze + React

Install Laravel Breeze:

```powershell
composer require laravel/breeze --dev
```

Install Breeze menggunakan React + TypeScript:

```powershell
php artisan breeze:install react
```

> Secara default Breeze terbaru menggunakan **React + TypeScript**.

---

# 📦 Install Dependency

```powershell
npm install
```

---

# ⚙️ Konfigurasi Environment

Salin file environment:

```powershell
Copy-Item .env.example .env
```

Generate application key:

```powershell
php artisan key:generate
```

Edit file `.env` dan sesuaikan konfigurasi database:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nama_database
DB_USERNAME=root
DB_PASSWORD=
```

---

# 🗄️ Migrasi Database

```powershell
php artisan migrate
```

Jika ingin sekaligus membuat data dummy:

```powershell
php artisan migrate --seed
```

---

# 📧 Konfigurasi Mailtrap (Testing Email)

Isi file `.env`:

```env
MAIL_MAILER=smtp
MAIL_HOST=sandbox.smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=YOUR_USERNAME
MAIL_PASSWORD=YOUR_PASSWORD
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@example.com
MAIL_FROM_NAME="${APP_NAME}"
```

---

# ▶️ Menjalankan Project

Buka **2 terminal PowerShell**.

Terminal pertama:

```powershell
php artisan serve
```

Terminal kedua:

```powershell
npm run dev
```

Aplikasi dapat diakses di:

```
http://127.0.0.1:8000
```

---

# 📦 Build Production

```powershell
npm run build
```

---

# 🧹 Bersih-Bersih Cache Laravel

```powershell
php artisan optimize:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
php artisan cache:clear
composer dump-autoload
```

> Pada Laravel 11 dan 12, `php artisan clear-compiled` sudah tidak diperlukan.

---

# 📁 Struktur Folder

```
file-project/
├── app/
├── bootstrap/
├── config/
├── database/
├── public/
├── resources/
│   ├── js/
│   │   ├── Components/
│   │   ├── Layouts/
│   │   ├── Pages/
│   │   ├── Types/
│   │   ├── app.tsx
│   │   └── ssr.tsx
│   └── css/
├── routes/
│   ├── web.php
│   └── console.php
├── storage/
├── tests/
├── vite.config.ts
└── package.json
```

---

# 📦 Package yang Sering Digunakan

Install semua sekaligus:

```powershell
npm install axios react-icons clsx
```

---

# 🔄 Update Dependency

Laravel:

```powershell
composer update
```

Frontend:

```powershell
npm update
```

---

# 🔍 Mengecek Route

```powershell
php artisan route:list
```

---

# 🔨 Membuat Resource

Controller:

```powershell
php artisan make:controller UserController
```

Model + Migration + Factory + Seeder:

```powershell
php artisan make:model User -mfs
```

Request Validation:

```powershell
php artisan make:request UserRequest
```

Middleware:

```powershell
php artisan make:middleware CheckRole
```

---

# 📌 Catatan

- Gunakan **Node.js LTS** (20 atau lebih baru).
- Jalankan `npm run dev` selama proses development.
- Jalankan `npm run build` sebelum deploy ke production.
- Jangan menghapus file `manifest.json` hasil build production.
- Simpan file `.env` dan jangan pernah mengunggahnya ke Git.

---

# 👨‍💻 Kontributor

Developed by **Teds**

Feel free to fork, improve, dan berkontribusi.

---

# 📄 Lisensi

Proyek ini menggunakan lisensi MIT.
