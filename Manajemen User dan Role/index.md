# MANAJEMEN USER DAN ROLE

## Latar Belakang
Dalam sistem basis data, manajemen pengguna (user), peran (role), dan hak akses (privilege) sangat penting untuk menjaga keamanan dan keteraturan sistem. Dengan adanya pengelolaan user dan role, administrator basis data dapat mengatur siapa yang dapat mengakses, mengubah, atau menghapus data. Praktikum ini bertujuan untuk memahami bagaimana membuat, mengelola, serta menguji pengguna dan hak akses dalam MysQL.

## Problem yang Diangkat
1. Bagaimana cara membuat, menghapus, dan melihat daftar user dalam MySQL?
2. Bagaimana cara memberikan hak akses tertentu kepada user melalui role?
3. Bagaimana cara menguji apakah role yang diberikan telah berfungsi dengan benar?
4. Bagaimana cara mencabut role dari user agar mereka tidak memiliki hak akses tertentu?
5. Bagaimana cara memonitor aktivitas pengguna dalam MySQL melalui log?

## Solusi/Skenario Aktivitas (Soal yang Dikerjakan)

### 1. Lakukan proses pembuatan username sebanyak jumlah kelompok Anda! Tuliskan script dan tampilkan hasilnya!

-	Membuat tiga user baru dalam database MySQL dengan nama chalimatus, safira, dan sherli, masing-masing hanya dapat mengakses dari localhost.
-	Code :
  ```sql
  CREATE USER 'chalimatus'@'localhost' IDENTIFIED BY 'chalimatus';
  ```
  ![Gambar 1](Gambar1.jpg)
- Code :
  ```sql
  CREATE USER 'safira'@'localhost' IDENTIFIED BY 'safira';
  ```
  ![Gambar 2](Gambar2.jpg)
- Code :
  ```sql
  CREATE USER 'sherli'@'localhost' IDENTIFIED BY 'sherli';
  ```
  ![Gambar 3](Gambar3.jpg)
-	Menampilkan daftar user beserta host yang diizinkan untuk mengakses database.
- Code :
  ```sql
  SELECT USER, HOST FROM mysql.user WHERE USER IN ('chalimatus', 'safira', 'sherli');
  ```
  ![Gambar 4](Gambar4.jpg)

### 2. Lakukan penghapusan username terhadap user yang sudah dibuat. Tuliskan script dan tampilkan hasilnya

- Menghapus user sherli dari MySQL
- Code :
  ```sql
  DROP USER 'sherli'@'localhost';
  ```
  ![Gambar 5](Gambar5.jpg)
- Mengecek kembali daftar user yang masih ada setelah penghapusan user sherli.
- Code :
  ```sql
  SELECT USER, HOST FROM mysql.user WHERE USER IN ('chalimatus', 'safira', 'sherli');
  ```
  ![Gambar 6](Gambar6.jpg)
