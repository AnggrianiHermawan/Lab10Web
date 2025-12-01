# Praktikum 10 – PHP OOP

**Nama:** Anggriani Hermawan
**NIM:** 312410175  
**Mata Kuliah:** Pemrograman Web  
**Dosen:** Agung Nugroho, S.Kom., M.Kom

---
## Tugas
Implementasikan konsep modularisasi pada kode program pada praktukum sebelumnya
dengan menggunakan class library untuk form dan database connection.

## Tujuan Praktikum
1. Menerapkan konsep modularisasi pada aplikasi PHP.
2. Membuat Class Library untuk koneksi database.
3. Membuat Class Library untuk elemen form.
4. Mengubah kode praktikum sebelumnya menjadi versi OOP yang lebih terstruktur.

   
# 1. Struktur Folder
lab10_php_oop/
│── index.php
│── simpan.php
│── database.php
│── form.php

# 2. Implementasi Program
## Class databases (database.php)
```
<?php
class Database {
    private $host = "localhost";
    private $user = "root";
    private $pass = "";
    private $dbname = "latihan1";
    public $conn;

    public function __construct() {
        $this->conn = new mysqli($this->host, $this->user, $this->pass, $this->dbname);

        if ($this->conn->connect_error) {
            die("Koneksi gagal: " . $this->conn->connect_error);
        }
    }

    public function query($sql) {
        return $this->conn->query($sql);
    }
}
```
Penjelasan:
Kelas Database ini berfungsi sebagai wrapper (pembungkus) yang menggunakan MySQLi untuk membuat koneksi ke database secara otomatis saat diinisiasi dan menyediakan metode untuk mengeksekusi kueri SQL.

## Class Form (form.php)
```
<?php
class Form {
    public function input($label, $name) {
        echo "<label>$label</label><br>";
        echo "<input type='text' name='$name'><br><br>";
    }

    public function submit($text) {
        echo "<button type='submit'>$text</button>";
    }
}
```
Penjelasan:
Class Form ini digunakan untuk membuat elemen form secara otomatis.
- input($label, $name) → menampilkan label dan input text.
- submit($text) → menampilkan tombol submit.
Tujuannya supaya pembuatan form lebih cepat dan tidak menulis HTML berulang-ulang.

## Halaman input (index.php)
```
<?php
require_once "form.php";

$form = new Form();
echo "<h2>Input Data Barang</h2>";

echo "<form method='post' enctype='multipart/form-data' action='simpan.php'>";
$form->input("Kategori", "kategori");
$form->input("Nama Barang", "nama");
$form->input("Harga Beli", "harga_beli");
$form->input("Harga Jual", "harga_jual");
$form->input("Stok", "stok");
echo "<label>Gambar</label><br><input type='file' name='gambar'><br><br>";
$form->submit("Simpan");
echo "</form>";
```
Penjelasan:
Kode ini menampilkan form input data barang dengan bantuan class Form.
- require_once "form.php" → memanggil class Form.
- $form = new Form() → membuat objek form.
- $form->input(...) → membuat input kategori, nama, harga beli, harga jual, dan stok.
- <input type='file'> → untuk upload gambar.
- $form->submit("Simpan") → membuat tombol submit.
- action='simpan.php' → data dikirim ke file simpan.php untuk diproses.

## Halaman proses simpan (simpan.php)
```<?php
require_once "database.php";

$db = new Database();

$kategori   = $_POST['kategori'];
$nama       = $_POST['nama'];
$harga_beli = $_POST['harga_beli'];
$harga_jual = $_POST['harga_jual'];
$stok       = $_POST['stok'];

$gambar = "";
if (!empty($_FILES['gambar']['name'])) {
    $file_name = time() . "_" . basename($_FILES['gambar']['name']);
    $path = "assets/uploads/" . $file_name;

    if (move_uploaded_file($_FILES['gambar']['tmp_name'], $path)) {
        $gambar = $path;
    }
}

$sql = "INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
        VALUES ('$kategori', '$nama', '$gambar', '$harga_beli', '$harga_jual', '$stok')";

$db->query($sql);

echo "Data berhasil disimpan!";
```
Penjelasan:
Kode ini mengambil data dari form, mengupload gambar jika ada, lalu menyimpan semua data (kategori, nama, harga, stok, dan gambar) ke tabel data_barang menggunakan class Database. Setelah berhasil, tampil pesan “Data berhasil disimpan!”.

## Menggunakan Class Library untuk Database Connection
```
class Database {
    public function __construct() { ... }
    public function query($sql) { ... }
}
```
Ini adalah class library OOP

## Menggunakan Class Library untuk Form
```
class Form {
    public function input() { ... }
    public function submit() { ... }
}
```
Ini juga class library

# 3. Hasil Output
<img width="1920" height="1080" alt="Cuplikan layar 2025-12-01 104000" src="https://github.com/user-attachments/assets/0b439faa-bdb4-4871-ae59-7b6d231caca3" />

<img width="1920" height="1080" alt="Cuplikan layar 2025-12-01 104014" src="https://github.com/user-attachments/assets/ff56b082-aeaa-49d6-86fd-5ee06484f54e" />

