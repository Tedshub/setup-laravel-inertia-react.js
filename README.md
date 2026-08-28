# Instalasi Project Laravel Breeze (Inertia React)

Panduan ini berisi langkah-langkah untuk membuat dan menginstal project Laravel menggunakan Breeze dengan *stack* Inertia + React dari awal, beserta solusi untuk beberapa *error* yang mungkin terjadi.

> **Catatan versi (per Agustus 2026):** Panduan ini mengacu pada Laravel 13.x, yang merupakan rilis stabil saat ini. Laravel Breeze sudah kompatibel dengan Laravel 13.x.

## 0. Prasyarat

Sebelum memulai, pastikan environment Anda memenuhi syarat berikut:

- **PHP 8.3 atau lebih tinggi** — ini adalah syarat minimum untuk Laravel 13.x (naik dari PHP 8.2 di versi sebelumnya).
- **Composer** versi terbaru.
- **Node.js & NPM** versi LTS terbaru (disarankan untuk menghindari masalah kompatibilitas dengan Vite).

## 1. Membuat Project Laravel Baru

Ada dua cara umum untuk membuat project baru:

**Opsi A — Menggunakan Laravel Installer (direkomendasikan resmi):**
```bash
laravel new nama-project
cd nama-project
```
*(Saat menjalankan `laravel new`, Anda bisa langsung memilih untuk menginstal starter kit Breeze pada proses instalasi jika tersedia sebagai opsi.)*

**Opsi B — Menggunakan Composer (masih valid dan umum dipakai):**
```bash
composer create-project laravel/laravel nama-project
cd nama-project
```

Panduan ini akan melanjutkan proses instalasi Breeze secara manual, sehingga kedua opsi di atas dapat digunakan.

## 2. Konfigurasi Database

1. Buka file `.env`.
2. Atur koneksi database yang ingin digunakan. Jika ingin menggunakan **SQLite** (default sejak Laravel 11 dan masih berlaku di Laravel 13), konfigurasinya cukup seperti ini:
```env
DB_CONNECTION=sqlite
```
*(Catatan: Laravel akan otomatis membuat file `database.sqlite` di folder `database/` saat pertama kali migrate.)*

## 3. Instalasi Laravel Breeze

Instal paket pendukung Laravel Breeze menggunakan Composer:
```bash
composer require laravel/breeze --dev
```

Setelah paket terinstal, jalankan proses *scaffolding* Breeze dan pilih **React with Inertia**:
```bash
php artisan breeze:install react
```
*(Perintah ini akan menyusun struktur dasar Inertia, React, dan Tailwind CSS.)*

---

## 4. Mengatasi Error Umum (Troubleshooting)

Berdasarkan pengalaman saat melakukan *setup*, berikut dua kendala umum yang masih sering terjadi hingga saat ini:

### Error 1: Dependency Conflict pada NPM (Vite)

Saat menjalankan `npm install`, terkadang muncul *error* `ERESOLVE unable to resolve dependency tree` karena ketidakcocokan versi *peer dependency* (misalnya versi Vite terbaru bentrok dengan plugin React bawaan).

**Solusi:** Lewati pengecekan *peer dependency* yang ketat dengan menambahkan *flag* `--legacy-peer-deps`:
```bash
npm install --legacy-peer-deps
npm install axios --legacy-peer-deps
```

### Error 2: Error Build "Could not resolve './bootstrap'"

Saat menjalankan *build* Vite, terkadang muncul pesan bahwa file `./bootstrap` tidak ditemukan saat diimpor dari `resources/js/app.jsx`. File ini seharusnya berisi konfigurasi *header HTTP default* dari Axios, namun terkadang gagal ikut ter-*generate* saat proses scaffolding.

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
*(Selama masa development, sangat disarankan untuk menggunakan `npm run dev` agar setiap perubahan langsung ter-update.)*

Terakhir, jalankan server lokal Laravel:
```bash
php artisan serve
```

Selesai! Aplikasi Anda kini dapat diakses melalui `http://localhost:8000`.
