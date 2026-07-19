# Instalasi Project Laravel Breeze (Inertia React)

Panduan ini berisi langkah-langkah untuk membuat dan menginstal project Laravel menggunakan Breeze dengan *stack* Inertia + React dari awal, beserta solusi untuk beberapa *error* yang mungkin terjadi.

## 1. Membuat Project Laravel Baru
Jalankan perintah berikut di terminal untuk membuat project Laravel baru:
```bash
composer create-project laravel/laravel nama-project
cd nama-project
```

## 2. Konfigurasi Database
1. Buka file `.env`.
2. Atur koneksi database yang ingin digunakan. Jika ingin menggunakan **SQLite** (default di Laravel 11+), konfigurasinya cukup seperti ini:
```env
DB_CONNECTION=sqlite
```
*(Catatan: Laravel 11 akan otomatis membuat file `database.sqlite` di folder `database/` saat pertama kali migrate).*

## 3. Instalasi Laravel Breeze
Instal paket pendukung Laravel Breeze menggunakan Composer:
```bash
composer require laravel/breeze --dev
```

Setelah paket terinstal, jalankan proses *scaffolding* Breeze dan pilih **React with Inertia**:
```bash
php artisan breeze:install react
```
*(Perintah ini akan menyusun struktur dasar Inertia, React, dan Tailwind CSS).*

---

## 4. Mengatasi Error Umum (Troubleshooting)
Berdasarkan pengalaman saat melakukan *setup* sebelumnya, ada dua kendala umum yang sering terjadi. Berikut adalah cara menyelesaikannya:

### Error 1: Dependency Conflict pada NPM (Vite)
Saat menjalankan `npm install`, terkadang muncul *error* `ERESOLVE unable to resolve dependency tree` karena ketidakcocokan versi *peer dependency* (misalnya versi Vite terbaru bentrok dengan plugin React bawaan).
**Solusi:** Lewati pengecekan *peer dependency* yang ketat dengan menambahkan *flag* `--legacy-peer-deps`:
```bash
npm install --legacy-peer-deps
npm install axios --legacy-peer-deps
```

### Error 2: Error Build "Could not resolve './bootstrap'"
Saat menjalankan *build* Vite, muncul pesan peringatan bahwa file `./bootstrap` tidak ditemukan di `resources/js/app.jsx`. File ini berisi konfigurasi *header HTTP default* dari Axios yang terkadang gagal di-*generate*.
**Solusi:** Buat file baru di `resources/js/bootstrap.js` secara manual dan isikan dengan kode berikut:
```javascript
import axios from 'axios';
window.axios = axios;

window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
```

---

## 5. Menjalankan Migrasi Database
Jalankan migrasi agar tabel bawaan otentikasi (users, password_reset, dll.) terbuat di database Anda:
```bash
php artisan migrate
```

## 6. Build Asset & Jalankan Server
Lakukan kompilasi untuk aset React dan Tailwind Anda:
```bash
npm run build
```
*(Selama masa development, sangat disarankan untuk menggunakan `npm run dev` agar setiap perubahan langsung ter-update).*

Terakhir, jalankan server lokal Laravel:
```bash
php artisan serve
```

Selesai! Aplikasi Anda kini dapat diakses melalui `http://localhost:8000`.
