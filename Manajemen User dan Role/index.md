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

- Proses pembuatan username ke dalam MySQL sebanyak 3 kali dengan menggunakan script sebagai berikut:

  -	Membuat tiga user baru dalam database MySQL dengan nama chalimatus, safira, dan sherli, masing-masing hanya dapat mengakses dari localhost.
  -	Code :
    ```sql
    CREATE USER 'chalimatus'@'localhost' IDENTIFIED BY 'chalimatus';
  -	Output run yaitu 1 querie, 1 succes tanpa adanya error.
![Gambar 1](Gambar1.jpg)
  - Code :
   	```sql
    CREATE USER 'safira'@'localhost' IDENTIFIED BY 'safira';
  - Hasil run yaitu 1 querie, 1 succes tanpa adanya error.
![Gambar 2](Gambar2.jpg)
