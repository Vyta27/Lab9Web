Nama  : Navyta Budi Yulia

NIM   : 312410184

kelas : TI.24.A2

# Lab9Web

## Langkah 1
- buat folder `lab9_php_modular` di `htdocs`
- buat struktur folder seperti ini :
```
lab9_php_modular/
├── index.php
├── congif/
│     └── database.php
├── views/
│    ├── header.php
│    ├── footer.php
│    └── dashboard.php
├── modules/
│     ├── user/
│     │   ├── add.php
│     │   ├── edit.php
│     │   ├── list.php
│     │   └── delete.php
│     └── auth/
│         ├── login.php
│         └── logout.php
└──  assets/
      └── css
          └── style.css
```
## Langkah 2 : Buat kobeksi database 
- Tujuan : Menghubungkan seluruh modul ke database tanpa file terpisah.
- file : `database.php`

```
<?php
// Menampilkan error untuk debugging
error_reporting(E_ALL);
ini_set('display_errors', 1);

// Konfigurasi koneksi
$host = "127.0.0.1"; // atau localhost
$user = "root";
$pass = "";
$port = "3307"; // sesuaikan dengan MySQL kamu
$db   = "latihan1"; // database praktikum 8

// Membuat koneksi
$conn = mysqli_connect($host, $user, $pass, $db, $port);

// Check koneksi
if (!$conn) {
    die("Koneksi database gagal: " . mysqli_connect_error());
}
?>
```

## Langkah 3 : Buat template Header & Footer
- Tujuan : Membuat tampilan semua halaman konsisten.
- File: `header.php` & `footer.php`
  
Header :

```
<?php if(session_status() === PHP_SESSION_NONE) session_start(); ?>
<!DOCTYPE html>
<html>
<head>
    <title>Modular PHP</title>
    <link rel="stylesheet" href="assets/style.css">
</head>
<body>
<div class="container">

<header>
    <h1>Aplikasi Data Barang</h1>

    <?php if(isset($_SESSION['logged_in'])): ?>
        <a href="index.php?page=auth/logout">Logout</a>
    <?php endif; ?>
</header>

<nav>
    <a href="index.php?page=dashboard">Dashboard</a>
    <a href="index.php?page=user/list">Data Barang</a>
    <a href="index.php?page=auth/login">Login</a>
</nav>

<main>
```
Footer :

```
</main>

<footer>
    <p>&copy; 2025 - Pemrograman Web</p>
</footer>

</div>
</body>
</html>
```

## Langkah 4 : Implementasi Routing
- Tujuan : Membuat halaman modular, dipanggil via `index.php?page=...`
- File : `index.php`

```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

session_start();

// Ambil parameter page dulu!
$page = isset($_GET['page']) ? $_GET['page'] : 'auth/login';

// Cek login kecuali halaman login
if (!isset($_SESSION['logged_in']) && $page !== "auth/login") {
    header("Location: index.php?page=auth/login");
    exit;
}

// Koneksi database
include "config/database.php";

// Header
include "views/header.php";

// Sanitasi
$page = trim($page);
$page = str_replace('../', '', $page);
$page = str_replace('./', '', $page);

// DASHBOARD ADA DI FOLDER VIEWS
if ($page == "dashboard") {
    include "views/dashboard.php";
} 
else {
    // Routing modul
    $path = "modules/" . str_replace(".", "/", $page) . ".php";

    if (file_exists($path)) {
        include $path;
    } else {
        echo "<h2>404 - Halaman tidak ditemukan</h2>";
        echo "<p>File tidak ditemukan: <b>$path</b></p>";
    }
}

// Footer
include "views/footer.php";
?>
```
## Langkah 5 : Buat halaman login
- file : `login.php`
- Fitur :
      - Input username & password.
      - Tombol login & reset.
      - Menampilkan error jika salah.
  
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/37aed073-923c-481e-b9ab-1b7727a901f6" />

  ## Langkah 6 : Buat halaman dashboard
  - File : `dashboard.php`
  - Fitur :
        - tombol ke list & add barang.
        - tombol logout konsisten.
    
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/c98d57d3-b588-424d-ae0d-af697805809f" />

  ## Langkah 7 : Halaman List Barang
  - File : `list.php`
  - Fitur :
        - tabel data barang.
        - Tombol edit/hapus
        - Live search

    ss

    ## Langkah 8 : Halaman add barang
    - file : `add.php`
    - Fitur :
          - Form rapi, Flexbox, placeholder, tooltip.
          - Upload gambar.
          - Tombol Simpan & Reset pink.

    ss

    ## Langkah 9 : Edit barang
    - File : `edit.php`
    - Fitur :
          - Form pre-filled data lama.
          - Upload gambar opsional.
          - Tombol Update & Reset.

      ss

    ## Langlah 10 : Halaman Logout
    - File : `logout.php`
    - Fitur :
          - Menghapus session & redirect ke login.

      ss
      
    
