# Sistem Distribusi Instrumen Medis (Berbasis QR CSSD)

Selamat datang di Sistem Distribusi Instrumen Medis! Aplikasi ini dirancang untuk merevolusi cara departemen CSSD (*Central Sterile Services Department*) mengelola, melacak, dan mendistribusikan instrumen medis menggunakan alur kerja berbasis kode QR yang efisien.

## 🏗️ Arsitektur Sistem

Proyek ini adalah aplikasi *full-stack* yang terdiri dari tiga komponen utama yang bekerja secara harmonis:

-   **Backend (Laravel 11)**: API inti yang menangani semua logika bisnis, manajemen basis data, autentikasi, dan operasi stok.
-   **Frontend Web (Vue 3)**: Dasbor administrasi berbasis web bagi staf CSSD untuk mengelola instrumen, unit, melihat laporan, dan memantau aktivitas sistem.
-   **Aplikasi Mobile (Ionic/Vue)**: Aplikasi seluler lintas platform bagi staf lapangan untuk melakukan transaksi secara cepat dengan memindai kode QR, bahkan dalam kondisi *offline*.

---

## 🚀 Memulai (Instalasi Lokal)

Bagian ini akan memandu Anda melalui proses penyiapan lengkap untuk setiap komponen aplikasi di lingkungan pengembangan lokal Anda.

### 1. Prasyarat

Pastikan Anda telah menginstal perangkat lunak berikut di mesin Anda:

-   **PHP v8.1+**
-   **Composer 2.x**
-   **Node.js v18+** (dengan npm v9+)
-   **Git**
-   **Database MySQL 8.0+**
-   **Ionic CLI v7+** (`npm install -g @ionic/cli`)
-   **Docker** (Opsional, tetapi direkomendasikan untuk penyiapan yang lebih mudah)

### 2. Struktur Proyek

Berikut adalah gambaran singkat tentang direktori utama dalam repositori ini:

```
/
├── backend/         # Aplikasi Backend Laravel
├── frontend-web/    # Aplikasi Frontend Web Vue
├── mobile/          # Aplikasi Mobile Ionic/Vue
└── docker-compose.yml # File konfigurasi untuk penyiapan Docker
```

### 3. Instalasi Backend (Laravel)

Backend adalah otak dari aplikasi. Ikuti langkah-langkah ini untuk menyiapkannya:

1.  **Navigasi ke Direktori Backend**
    ```bash
    cd backend
    ```

2.  **Instal Dependensi PHP**
    ```bash
    composer install
    ```

3.  **Konfigurasi Lingkungan**
    Salin *file* contoh `.env` dan konfigurasikan koneksi basis data Anda.
    ```bash
    cp .env.example .env
    ```
    Buka `.env` dan perbarui variabel `DB_*` berikut:
    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=nama_database_anda
    DB_USERNAME=root
    DB_PASSWORD=password_anda
    ```

4.  **Hasilkan Kunci Aplikasi**
    ```bash
    php artisan key:generate
    ```

5.  **Jalankan Migrasi & Seeding Database**
    Perintah ini akan membuat semua tabel basis data yang diperlukan dan mengisinya dengan data awal (termasuk pengguna *default*).
    ```bash
    php artisan migrate --seed
    ```

6.  **Jalankan Server Pengembangan**
    *Server* API sekarang akan berjalan di `http://localhost:8000`.
    ```bash
    php artisan serve
    ```

### 4. Instalasi Frontend Web (Vue)

Frontend web adalah dasbor admin Anda.

1.  **Navigasi ke Direktori Frontend**
    ```bash
    cd frontend-web
    ```

2.  **Instal Dependensi Node.js**
    ```bash
    npm install
    ```

3.  **Konfigurasi Lingkungan**
    Salin *file* `.env` dan pastikan URL API menunjuk ke *server backend* Anda.
    ```bash
    cp .env.example .env
    ```
    Isi dari `.env` harus terlihat seperti ini:
    ```
    VITE_API_URL=http://localhost:8000/api
    ```

4.  **Jalankan Server Pengembangan**
    Aplikasi web sekarang akan dapat diakses di `http://localhost:5173`.
    ```bash
    npm run dev
    ```

### 5. Instalasi Aplikasi Mobile (Ionic/Vue)

Aplikasi seluler digunakan untuk operasi lapangan.

1.  **Navigasi ke Direktori Mobile**
    ```bash
    cd mobile
    ```

2.  **Instal Dependensi Node.js**
    ```bash
    npm install
    ```

3.  **Konfigurasi URL API**
    Buka `vite.config.js` atau *file* konfigurasi lingkungan yang relevan dan pastikan *proxy* atau variabel lingkungan API menunjuk ke alamat IP lokal *backend* Anda (bukan `localhost`, agar dapat diakses dari perangkat seluler).
    Contoh: `VITE_API_URL=http://192.168.1.10:8000/api`

4.  **Jalankan di Browser (untuk Pengujian Cepat)**
    ```bash
    ionic serve
    ```

5.  **Jalankan di Emulator atau Perangkat Asli**
    Untuk menguji fungsionalitas *native* seperti pemindai kamera:
    ```bash
    # Tambahkan platform yang Anda inginkan (android atau ios)
    ionic cap add android

    # Sinkronkan kode Anda dengan proyek native
    ionic cap sync

    # Buka proyek di Android Studio untuk menjalankan di emulator/perangkat
    ionic cap open android
    ```

---

## 👥 Pengguna Default

Setelah menjalankan *database seeder*, Anda dapat masuk dengan kredensial berikut:

| Peran | Email | Kata Sandi |
| :--- | :--- | :--- |
| Admin CSSD | `admin@example.com` | `password` |
| Validator | `validator@example.com` | `password` |
| Operator | `operator@example.com` | `password` |

---

## 🐳 Instalasi dengan Docker (Direkomendasikan)

Jika Anda memiliki Docker, Anda dapat menjalankan seluruh tumpukan aplikasi dengan satu perintah:

1.  **Jalankan Docker Compose**
    ```bash
    docker-compose up -d --build
    ```

2.  **Jalankan Migrasi Database**
    ```bash
    docker-compose exec app php artisan migrate --seed
    ```

Layanan akan dapat diakses di:
-   **Backend API**: `http://localhost:8000`
-   **Frontend Web**: `http://localhost:5173`
-   **phpMyAdmin**: `http://localhost:8080`
