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

> ⚠️ **Catatan penting:** Perintah `breeze:install` otomatis menjalankan `npm install` di baliknya. Jika Node dependency-nya bentrok (lihat bagian **Troubleshooting** di bawah), instalasi npm akan **gagal** meski Artisan tetap menampilkan pesan `Breeze scaffolding installed successfully`. Pesan itu hanya berarti file-file Breeze (PHP, blade/react, route) berhasil ditulis — bukan berarti `node_modules` sudah terpasang. Selalu lanjutkan ke langkah **Install Dependency** di bawah dan pastikan `npm install` benar-benar sukses sebelum lanjut.

---

# 📦 Install Dependency

```powershell
npm install
```

## 🛠️ Troubleshooting: `npm error ERESOLVE unable to resolve dependency tree`

Jika muncul error seperti ini:

```
npm error ERESOLVE unable to resolve dependency tree
npm error Found: vite@8.1.5
npm error Could not resolve dependency:
npm error peer vite@"^4.2.0 || ^5.0.0 || ^6.0.0 || ^7.0.0" from @vitejs/plugin-react@4.7.0
```

**Penyebab:** `composer create-project` / Breeze menghasilkan `package.json` yang mengizinkan `vite@^8.0.0`, padahal `@vitejs/plugin-react@4.7.0` (yang dipakai Breeze untuk React) baru mendukung `vite` sampai versi 7. Vite 8 dirilis lebih baru daripada dependency Breeze yang ada, sehingga terjadi bentrok versi (peer dependency conflict).

Ada dua cara mengatasinya — pilih salah satu:

### Opsi A (disarankan): Turunkan versi Vite ke yang didukung

Buka `package.json`, cari baris `"vite"`, lalu ubah menjadi versi yang kompatibel, misalnya:

```json
"vite": "^6.0.0"
```

Kemudian hapus lock file lama dan install ulang:

```powershell
Remove-Item package-lock.json -ErrorAction SilentlyContinue
Remove-Item node_modules -Recurse -Force -ErrorAction SilentlyContinue
npm install
```

### Opsi B (cepat, tapi kurang aman): Paksa install dengan `--legacy-peer-deps`

```powershell
npm install --legacy-peer-deps
```

Cara ini melewati pengecekan peer dependency, jadi instalasi tetap jalan meski versinya secara teknis belum "resmi" kompatibel. Cukup aman untuk kebutuhan development, tapi Opsi A lebih direkomendasikan untuk stabilitas jangka panjang.

> 💡 Setelah salah satu opsi di atas berhasil, jalankan `npm run dev` untuk memastikan Vite benar-benar bisa start tanpa error.

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

> ⚠️ Setelah `npm update`, jalankan kembali `npm run dev` untuk memastikan tidak ada konflik versi baru (seperti kasus Vite di atas) yang muncul akibat update.

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
- Jika `npm install` gagal dengan error `ERESOLVE`, lihat bagian **Troubleshooting** di langkah **Install Dependency**.

---

# 👨‍💻 Kontributor

Developed by **Teds**
Feel free to fork, improve, dan berkontribusi.

---

# 📄 Lisensi

Proyek ini menggunakan lisensi MIT.
