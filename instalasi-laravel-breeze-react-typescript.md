# Instalasi Project Laravel Breeze (Inertia React + TypeScript)

Panduan ini berisi langkah-langkah untuk membuat project Laravel dengan Breeze *stack* Inertia + React **berbasis TypeScript**, beserta solusi untuk beberapa *error* yang mungkin terjadi.

> **Catatan versi (per Oktober 2026):** Panduan ini mengacu pada Laravel 13.x. Perbedaan utama dibanding versi JavaScript ada pada flag `--typescript`, ekstensi file (`.tsx` / `.ts`), dan adanya `tsconfig.json`.

## 0. Prasyarat

- **PHP 8.3 atau lebih tinggi** (syarat minimum Laravel 13.x).
- **Composer** versi terbaru.
- **Node.js & NPM** versi LTS terbaru.

## 1. Membuat Project Laravel Baru

**Opsi A: Laravel Installer:**
```bash
laravel new nama-project
cd nama-project
```

**Opsi B: Composer:**
```bash
composer create-project laravel/laravel nama-project
cd nama-project
```

## 2. Konfigurasi Database

Buka `.env`, lalu atur koneksi database. Untuk SQLite:
```env
DB_CONNECTION=sqlite
```
*(Laravel akan otomatis membuat file `database/database.sqlite` saat pertama kali migrate.)*

## 3. Instalasi Laravel Breeze (TypeScript)

```bash
composer require laravel/breeze --dev
```

Jalankan *scaffolding* Breeze dengan stack React dan tambahkan flag `--typescript`:
```bash
php artisan breeze:install react --typescript
```

Atau, jika ingin memilih lewat prompt interaktif, jalankan `php artisan breeze:install`, pilih **React with Inertia**, lalu jawab **Yes** pada pertanyaan *"Would you like any optional features?"* → centang **TypeScript**.

Perintah ini akan menghasilkan:
- `resources/js/app.tsx` dan `resources/js/Pages/**/*.tsx` (bukan `.jsx`)
- `resources/js/types/` (berisi tipe global dan tipe Inertia)
- `tsconfig.json`
- Dependensi TypeScript (`typescript`, `@types/react`, `@types/react-dom`, `@types/node`)

Struktur ringkasnya:
```
resources/js/
├── app.tsx
├── bootstrap.ts
├── Components/
├── Layouts/
├── Pages/
└── types/
    ├── global.d.ts
    └── index.d.ts
```

---

## 4. Mengatasi Error Umum (Troubleshooting)

### Error 1: Dependency Conflict pada NPM (Vite)

Saat `npm install`, kadang muncul `ERESOLVE unable to resolve dependency tree`.

**Solusi:**
```bash
npm install --legacy-peer-deps
npm install axios --legacy-peer-deps
```

### Error 2: Error Build "Could not resolve './bootstrap'"

Terjadi jika `bootstrap.ts` gagal ter-*generate* saat scaffolding.

**Solusi:** Buat file `resources/js/bootstrap.ts`:
```typescript
import axios from 'axios';

window.axios = axios;
window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
```

Pastikan `resources/js/app.tsx` mengimpornya:
```typescript
import './bootstrap';
```

### Error 3 (khusus TypeScript): `Property 'axios' does not exist on type 'Window'`

Error ini muncul saat `tsc` tidak mengenali `window.axios`, biasanya karena file deklarasi tipe global hilang.

**Solusi:** Buat atau periksa file `resources/js/types/global.d.ts`:
```typescript
import { AxiosInstance } from 'axios';
import { route as routeFn } from 'ziggy-js';

declare global {
    interface Window {
        axios: AxiosInstance;
    }

    var route: typeof routeFn;
}
```

Pastikan `tsconfig.json` menyertakan folder tersebut:
```json
{
    "compilerOptions": {
        "allowJs": true,
        "module": "ESNext",
        "moduleResolution": "bundler",
        "jsx": "react-jsx",
        "strict": true,
        "isolatedModules": true,
        "target": "ESNext",
        "esModuleInterop": true,
        "forceConsistentCasingInFileNames": true,
        "skipLibCheck": true,
        "noEmit": true,
        "paths": {
            "@/*": ["./resources/js/*"],
            "ziggy-js": ["./vendor/tightenco/ziggy"]
        }
    },
    "include": ["resources/js/**/*.ts", "resources/js/**/*.tsx", "resources/js/**/*.d.ts"]
}
```

### Error 4 (khusus TypeScript): `npm run build` gagal karena error tipe

Pada project TypeScript, script `build` biasanya menjalankan `tsc && vite build`, sehingga error tipe akan menghentikan proses build.

**Solusi:** Jalankan pengecekan tipe terpisah untuk melihat detail error:
```bash
npx tsc --noEmit
```
Perbaiki error yang ditampilkan. Jika hanya ingin membangun aset sementara, jalankan `npx vite build` (tanpa pengecekan tipe).

---

## 5. Menjalankan Migrasi Database

```bash
php artisan migrate
```

## 6. Build Asset & Jalankan Server

```bash
npm run build
```
*(Selama development, gunakan `npm run dev` agar perubahan langsung ter-update.)*

Jalankan server lokal Laravel:
```bash
php artisan serve
```

Selesai! Aplikasi Anda dapat diakses di `http://localhost:8000`.

---

## Contoh: Halaman Inertia dengan Props Bertipe

Contoh `resources/js/Pages/Contoh.tsx`:
```tsx
import { Head } from '@inertiajs/react';
import { PageProps } from '@/types';

interface ContohProps extends PageProps {
    judul: string;
}

export default function Contoh({ auth, judul }: ContohProps) {
    return (
        <>
            <Head title={judul} />
            <h1>Halo, {auth.user.name}!</h1>
        </>
    );
}
```
