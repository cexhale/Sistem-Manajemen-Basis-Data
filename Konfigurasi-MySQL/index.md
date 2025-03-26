# KONFIGURASI MYSQL

## Latar Belakang
MySQL adalah sistem manajemen basis data relasional (RDBMS) yang banyak digunakan di berbagai sistem informasi. Sebagai mahasiswa yang belajar tentang database, memahami cara menginstal dan mengonfigurasi MySQL adalah keterampilan penting untuk memastikan sistem bekerja optimal dan aman.

## Problem yang Diangkat
1. Bagaimana cara menginstal MySQL dengan benar?
2. Bagaimana cara mengelola user dan hak akses dalam MySQL?
3. Bagaimana cara membuat dan mengelola database?
4. Bagaimana cara melakukan konfigurasi dasar MySQL untuk meningkatkan performa?

## Solusi/Skenario Aktivitas (Soal yang Dikerjakan)

### 1.	Jelaskan tentang database relational, database unrelational dan berikan contoh produknya masing-masing 3!

   - Database Relational

      - Database relational adalah jenis database yang menyimpan data dalam bentuk tabel dengan baris dan kolom, serta memiliki hubungan antar tabel menggunakan kunci primer dan kunci asing.
   
      - Contoh produk database relational:
         - MySQL
         - PostgreSQL
         - Microsoft SQL Server

   - Database Unrelational (NoSQL)

      - Database unrelational adalah database yang tidak menggunakan model tabel, tetapi menggunakan format seperti dokumen, key-value, grafik, atau column-family. Database ini lebih fleksibel dalam menangani data yang tidak terstruktur.
      
      - Contoh produk database unrelational:
         - MongoDB (document-based)
         - Redis (key-value store)
         - Cassandra (column-family store)

### 2. Jelaskan kapan harus menggunakan database relational dan kapan harus menggunakan  database unrelational 

   - Gunakan database relational jika:

     - Data memiliki struktur yang tetap dan hubungan yang kuat antar tabel.
     - Dibutuhkan transaksi yang kuat dengan dukungan ACID (Atomicity, Consistency, Isolation, Durability).
     - Data harus di-query dengan bahasa SQL secara kompleks.

   - Gunakan database unrelational jika:
      - Data bersifat tidak terstruktur atau semi-terstruktur (misalnya JSON atau XML).
      - Dibutuhkan performa tinggi untuk skala besar dengan banyak data yang berubah secara cepat.
      - Tidak diperlukan transaksi ACID yang ketat dan lebih mengutamakan skalabilitas horizontal.

### 3.	Lakukan instalasi database mysql dari awal sampai akhir. 
4.	Pastikan dalam setiap tahapan instalasi, didokumentasikan dalam bentuk screenshot untuk  bahan Menyusun laporan 
a.	Buka website MySQL lalu pilih menu downloads
