# Lab8web

# 1. Membuat Databases

Database MySQL berperan sebagai tempat penyimpanan data yang terstruktur. Tiga fungsi utama yang dijalankan adalah CREATE, READ, UPDATE, dan DELETE (CRUD):

<img width="1214" height="536" alt="image" src="https://github.com/user-attachments/assets/cf7f5db7-d167-4bf4-85db-8e43f0470401" />


# 2. Koneksi php


Fungsi: Bertanggung jawab untuk membuat sambungan atau koneksi antara skrip PHP dengan database server MySQL.


Kegunaan: Memastikan program dapat berkomunikasi dengan database latihan1 menggunakan detail koneksi yang ditentukan (localhost, root, ``, latihan1). File ini menjadi dasar yang harus di-include pada file lain yang memerlukan akses database.

<img width="1920" height="1080" alt="Screenshot (130)" src="https://github.com/user-attachments/assets/c4e72ad5-e242-41ed-a676-ddcea7337ae6" />



# 3. Membuat file `index.php`


Fungsi: Digunakan untuk menampilkan semua data dari tabel data_barang (operasi Read dalam CRUD).

Kegunaan: Menjadi halaman utama aplikasi. Halaman ini menjalankan query SELECT * FROM data_barang dan menampilkan hasilnya dalam bentuk tabel HTML. Di halaman ini juga terdapat tautan untuk Tambah Barang (Create) serta tombol Ubah dan Hapus untuk setiap baris data.

<img width="1920" height="1080" alt="Screenshot (132)" src="https://github.com/user-attachments/assets/e30929ac-45e7-4fa1-9d14-1264581eaa01" />

```
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
<div class="container">
<h1>Data Barang</h1>
<div class="main">
<table>
<tr>
<th>Gambar</th>
<th>Nama Barang</th>
<th>Katagori</th>
<th>Harga Jual</th>
<th>Harga Beli</th>
<th>Stok</th>
<th>Aksi</th>
</tr>
<?php if($result): ?>
<?php while($row = mysqli_fetch_array($result)): ?>
<tr>
<td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
<td><?= $row['nama'];?></td>
<td><?= $row['kategori'];?></td>
<td><?= $row['harga_beli'];?></td>
<td><?= $row['harga_jual'];?></td>
<td><?= $row['stok'];?></td>
<td><?= $row['id_barang'];?></td>
</tr>
<?php endwhile; else: ?>
<tr>
<td colspan="7">Belum ada data</td>
</tr>
<?php endif; ?>
</table>
</div>
</div>
</body>
</html>
```


# 4. Menambah Barang

Fungsi: Menangani proses penambahan data baru ke dalam tabel data_barang (operasi Create dalam CRUD).



Kegunaan: Menyediakan form HTML bagi pengguna untuk menginput detail barang baru (Nama, Kategori, Harga Jual/Beli, Stok, dan File Gambar). Skrip PHP di dalamnya memproses data yang dikirim ($_POST dan $_FILES), menyimpan gambar, dan menjalankan query INSERT INTO database.

<img width="1920" height="1080" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/37107b8d-b455-492f-9f6b-0473a1193303" />



```
<?php
include_once 'koneksi.php';

if (isset($_POST['submit'])) {

    $nama        = $_POST['nama'];
    $kategori    = $_POST['kategori'];
    $harga_jual  = $_POST['harga_jual'];
    $harga_beli  = $_POST['harga_beli'];
    $stok        = $_POST['stok'];

    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    // Upload file gambar
    if ($file_gambar['error'] == 0) {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__).'/gambar/'.$filename;

        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = 'gambar/'.$filename;
        }
    }

    // Query simpan
    $sql = "INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) 
            VALUES ('$nama', '$kategori', '$harga_jual', '$harga_beli', '$stok', '$gambar')";

    $result = mysqli_query($conn, $sql);

    header('Location: index.php');
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Tambah Barang</title>
<link rel="stylesheet" href="style.css">
<style>
/* Jika tidak punya style.css, gunakan CSS ini */
.container {
    width: 600px;
    margin: 20px auto;
    font-family: Arial;
}

h1 {
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
}

.input {
    margin-bottom: 15px;
}

label {
    display: inline-block;
    width: 120px;
}

input[type="text"], select {
    width: 250px;
    padding: 5px;
}

.submit input {
    background: blue;
    color: white;
    padding: 8px 15px;
    border: none;
    cursor: pointer;
}
</style>
</head>

<body>
<div class="container">
    <h1>Tambah Barang</h1>

    <form action="tambah.php" method="post" enctype="multipart/form-data">
        
        <div class="input">
            <label>Nama Barang</label>
            <input type="text" name="nama">
        </div>

        <div class="input">
            <label>Kategori</label>
            <select name="kategori">
                <option value="Elektronik">Elektronik</option>
                <option value="Komputer">Komputer</option>
                <option value="Hand Phone">Hand Phone</option>
            </select>
        </div>

        <div class="input">
            <label>Harga Jual</label>
            <input type="text" name="harga_jual">
        </div>

        <div class="input">
            <label>Harga Beli</label>
            <input type="text" name="harga_beli">
        </div>

        <div class="input">
            <label>Stok</label>
            <input type="text" name="stok">
        </div>

        <div class="input">
            <label>File Gambar</label>
            <input type="file" name="file_gambar">
        </div>

        <div class="submit">
            <input type="submit" name="submit" value="Simpan">
        </div>

    </form>
</div>
</body>
</html>
```

# 5. Mengubah Data ubah.php

Fungsi: Menangani proses perubahan atau pengeditan data barang yang sudah ada (operasi Update dalam CRUD).

Kegunaan:


Retrieval: Mengambil data barang spesifik berdasarkan id_barang dari URL ($_GET['id']) dan menampilkan data tersebut ke dalam form.


Updating: Memproses data dari form yang di-submit, termasuk upload gambar baru (jika ada), dan menjalankan query UPDATE data_barang SET ... WHERE id_barang = '{$id}' untuk memperbarui catatan di database.

<img width="1920" height="1080" alt="Screenshot (136)" src="https://github.com/user-attachments/assets/e86a78e7-15cd-4402-993c-7eca848cd39c" />


```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php'; // Pastikan file koneksi.php sudah benar

// Fungsi untuk selected option
function is_select($value, $selected) {
    return ($value == $selected) ? 'selected="selected"' : '';
}

if (isset($_POST['submit'])) {
    // 1. Ambil dan sanitasi input
    $id         = filter_input(INPUT_POST, 'id', FILTER_SANITIZE_NUMBER_INT);
    $nama       = filter_input(INPUT_POST, 'nama', FILTER_SANITIZE_SPECIAL_CHARS);
    $kategori   = filter_input(INPUT_POST, 'kategori', FILTER_SANITIZE_SPECIAL_CHARS);
    $harga_jual = filter_input(INPUT_POST, 'harga_jual', FILTER_SANITIZE_NUMBER_FLOAT, FILTER_FLAG_ALLOW_FRACTION);
    $harga_beli = filter_input(INPUT_POST, 'harga_beli', FILTER_SANITIZE_NUMBER_FLOAT, FILTER_FLAG_ALLOW_FRACTION);
    $stok       = filter_input(INPUT_POST, 'stok', FILTER_SANITIZE_NUMBER_INT);
    $gambar_lama = filter_input(INPUT_POST, 'gambar_lama', FILTER_SANITIZE_SPECIAL_CHARS); // Ambil nama gambar lama

    $file_gambar = $_FILES['file_gambar'];
    $gambar_baru = $gambar_lama; // Default: gunakan gambar lama

    // 2. Upload file gambar jika ada
    if ($file_gambar['error'] === UPLOAD_ERR_OK) {
        $allowed_types = ['image/jpeg', 'image/png', 'image/gif'];
        if (in_array($file_gambar['type'], $allowed_types)) {
            
            $filename = uniqid() . '_' . str_replace(' ', '_', $file_gambar['name']); // Nama unik
            $destination = __DIR__ . '/gambar/' . $filename;
            $upload_dir = __DIR__ . '/gambar/';
            
            // Buat direktori jika belum ada
            if (!is_dir($upload_dir)) {
                mkdir($upload_dir, 0777, true);
            }

            if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
                $gambar_baru = 'gambar/' . $filename;

                // Hapus gambar lama jika berhasil upload gambar baru dan gambar lama ada
                if (!empty($gambar_lama) && file_exists($gambar_lama)) {
                    unlink($gambar_lama);
                }
            } else {
                die('Error: Gagal mengupload file gambar.');
            }
        } else {
            die('Error: Tipe file gambar tidak diizinkan. Hanya JPEG, PNG, dan GIF.');
        }
    }

    // 3. Query update menggunakan prepared statement
    $sql = "UPDATE data_barang SET 
                nama = ?,
                kategori = ?,
                harga_jual = ?,
                harga_beli = ?,
                stok = ?,
                gambar = ?
            WHERE id_barang = ?";
    
    // Siapkan statement
    $stmt = mysqli_prepare($conn, $sql);

    if ($stmt) {
        // Bind parameter. Gunakan tipe data yang sesuai (s=string, i=integer, d=double)
        mysqli_stmt_bind_param($stmt, 'ssddisi', $nama, $kategori, $harga_jual, $harga_beli, $stok, $gambar_baru, $id);

        // Eksekusi statement
        if (mysqli_stmt_execute($stmt)) {
            // Berhasil
            mysqli_stmt_close($stmt);
            header('Location: index.php?status=sukses_ubah');
            exit;
        } else {
            // Gagal eksekusi
            die('Error saat update data: ' . mysqli_stmt_error($stmt));
        }
    } else {
        // Gagal menyiapkan statement
        die('Error menyiapkan statement: ' . mysqli_error($conn));
    }
}

// 4. Ambil data barang berdasarkan id (menggunakan prepared statement)
if (!isset($_GET['id'])) {
    die('Error: ID barang tidak ditemukan');
}

$id = filter_input(INPUT_GET, 'id', FILTER_SANITIZE_NUMBER_INT);

$sql = "SELECT * FROM data_barang WHERE id_barang = ?";
$stmt = mysqli_prepare($conn, $sql);

if ($stmt) {
    mysqli_stmt_bind_param($stmt, 'i', $id);
    mysqli_stmt_execute($stmt);
    $result = mysqli_stmt_get_result($stmt);
    
    if (!$result || mysqli_num_rows($result) === 0) {
        die('Error: Data barang tidak ditemukan.');
    }

    $data = mysqli_fetch_array($result, MYSQLI_ASSOC);
    mysqli_stmt_close($stmt);

} else {
    die('Error menyiapkan statement: ' . mysqli_error($conn));
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Ubah Barang</title>
<link rel="stylesheet" href="style.css">
<style>
/* ... (CSS tetap sama) ... */
.container {
    width: 600px;
    margin: 20px auto;
    font-family: Arial;
}

h1 {
    font-size: 28px;
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
}

.input {
    margin-bottom: 15px;
}

label {
    display: inline-block;
    width: 120px;
}

input[type="text"], select {
    width: 250px;
    padding: 5px;
}

.submit input {
    background: blue;
    color: white;
    padding: 8px 15px;
    border: none;
    cursor: pointer;
    margin-left: 120px;
}
</style>
</head>
<body>

<div class="container">

    <h1>Ubah Barang</h1>

    <form method="post" action="ubah.php" enctype="multipart/form-data">

        <div class="input">
            <label>Nama Barang</label>
            <input type="text" name="nama" value="<?php echo htmlspecialchars($data['nama']); ?>" required>
        </div>

        <div class="input">
            <label>Kategori</label>
            <select name="kategori">
                <option value="Komputer"   <?php echo is_select('Komputer', $data['kategori']); ?>>Komputer</option>
                <option value="Elektronik" <?php echo is_select('Elektronik', $data['kategori']); ?>>Elektronik</option>
                <option value="Hand Phone" <?php echo is_select('Hand Phone', $data['kategori']); ?>>Hand Phone</option>
            </select>
        </div>

        <div class="input">
            <label>Harga Jual</label>
            <input type="text" name="harga_jual" value="<?php echo htmlspecialchars($data['harga_jual']); ?>" required>
        </div>

        <div class="input">
            <label>Harga Beli</label>
            <input type="text" name="harga_beli" value="<?php echo htmlspecialchars($data['harga_beli']); ?>" required>
        </div>

        <div class="input">
            <label>Stok</label>
            <input type="text" name="stok" value="<?php echo htmlspecialchars($data['stok']); ?>" required>
        </div>

        <div class="input">
            <label>Gambar Saat Ini</label>
            <?php if (!empty($data['gambar'])): ?>
                <img src="<?php echo htmlspecialchars($data['gambar']); ?>" width="100">
            <?php else: ?>
                <span>(Tidak ada gambar)</span>
            <?php endif; ?>
        </div>

        <div class="input">
            <label>File Gambar Baru</label>
            <input type="file" name="file_gambar">
        </div>

        <div class="submit">
            <input type="hidden" name="id" value="<?php echo htmlspecialchars($data['id_barang']); ?>">
            <input type="hidden" name="gambar_lama" value="<?php echo htmlspecialchars($data['gambar']); ?>">
            <input type="submit" name="submit" value="Simpan">
        </div>

    </form>

</div>

</body>
</html>
```

# 6. Menghapus Data

Fungsi: Menangani proses penghapusan data barang tertentu (operasi Delete dalam CRUD).


Kegunaan: Menerima id_barang melalui URL ($_GET['id']) dan menjalankan query DELETE FROM data_barang WHERE id_barang = '{$id}' untuk menghapus baris data yang sesuai dari database. Setelah selesai, skrip akan mengarahkan pengguna kembali ke halaman index.php.

```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
