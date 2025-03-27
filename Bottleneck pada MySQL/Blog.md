# BOTTLENECK PADA MYSQL

## Latar Belakang Pembahasan
Dalam pengelolaan basis data, sering terjadi bottleneck atau hambatan yang menyebabkan performa database menurun, terutama ketika menangani jumlah data yang besar. Bottleneck ini dapat muncul dalam berbagai bentuk seperti query yang tidak optimal, penggunaan indeks yang kurang efektif, atau penguncian transaksi yang menghambat proses lainnya. Oleh karena itu, optimasi sangat diperlukan agar performa database tetap efisien dan responsif.

## Problem yang Diangkat
Masalah utama yang diangkat dalam laporan ini adalah:
1. Bagaimana cara mengoptimalkan eksekusi query agar lebih cepat dan efisien?
2. Bagaimana cara menambahkan indeks untuk mempercepat pencarian data?
3. Bagaimana teknik optimasi yang bisa diterapkan pada database transaksi seperti minimarket?
4. Bagaimana cara menangani pencarian data teks yang lebih efisien?

## Solusi/Skenario Aktivitas (Soal yang Dikerjakan)
### 1.	Jalankan setiap langkah dari materi diatas, tuliskan pemahaman anda.
```sql
SELECT * FROM orders WHERE customer_id = 1001;
```

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
![Gambar 1](assets/Gambar1.png)
 
```sql
SELECT * FROM users;
```
![Gambar 2](assets/Gambar2.png)
  
```sql
SELECT id, nama, email FROM users;
```
![Gambar 3](assets/Gambar3.png)
 
- Menambahkan Indeks pada Kolom yang Sering Digunakan dalam WHERE dan JOIN Cara Menambahkan Indeks:
  - Indeks pada Kolom WHERE
    - Query:
      ```sql
      CREATE INDEX idx_nama ON users(nama);
      ```
      ![Gambar 4](assets/Gambar4.png)
 
  -	Indeks pada Kolom yang Digunakan dalam JOIN
    - Query:
      ```sql
      CREATE INDEX idx_customer_id ON orders(customer_id);
      ```
      ![Gambar 5](assets/Gambar5.png)
 
  -	Indeks Multi-Kolom untuk Query yang Melibatkan Beberapa Kolom
    ```sql
    Query: CREATE INDEX idx_multi ON orders(customer_id, status);
    ```
    ![Gambar 6](assets/Gambar6.png)
 
- Mengevaluasi efektivitas indeks
  - Query:
    ```sql
    EXPLAIN SELECT * FROM users WHERE nama = 'John';
    ```
    ![Gambar 7](assets/Gambar7.png)
 
### 2.	Pada database minimartket, pada table tr_penjualan_raw, lakukan optimasi dengan menggunakan tehnik diatas. Gunakan index dan query chace.
- Menambahkan Indeks
  - Berdasarkan tgl_transaksi
    - Query:
      ```sql
      CREATE INDEX idx_tgl_transaksi ON tr_penjualan_raw(tgl_transaksi);
      ```
      ![Gambar 8](assets/Gambar8.png)

  -	Berdasarkan kode_item
    - Query:
      ```sql
      CREATE INDEX idx_kode_item ON tr_penjualan_raw(kode_item);
      ```
      ![Gambar 9](assets/Gambar9.png)
 
### 3.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi.
```sql
SELECT * FROM tr_penjualan_raw WHERE YEAR(tgl_transaksi) = 2008;
```
![Gambar 10](assets/Gambar10.png) 

- Optimasi:
  - Gunakan range data Query:
    ```sql
    SELECT * FROM tr_penjualan_raw
    WHERE tgl_transaksi BETWEEN '2008-01-01' AND '2008-12-31';
    ```
    ![Gambar 11](assets/Gambar11.png)
    
### 4.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi
```sql
SELECT * FROM tr_penjualan_raw WHERE kode_item IN ('IT003', 'IT004', 'IT005', 'IT006', 'IT007', 'IT008', 'IT009', 'IT010', 'IT011', 'IT012');
```
![Gambar 12](assets/Gambar12.png)
 
### 5.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi
```sql
SELECT * FROM tr_penjualan_raw WHERE nama_kasir LIKE '%John%';
```
![Gambar 13](assets/Gambar13.png)

- Optimasi
  - Query:
    ```sql
    ALTER TABLE tr_penjualan_raw ADD FULLTEXT(nama_kasir);
    ```
    ![Gambar 14](assets/Gambar14.png)
    
- Ubah query:
  ```sql
  SELECT * FROM tr_penjualan_raw WHERE MATCH(nama_kasir) AGAINST ('John' IN NATURAL LANGUAGE MODE);
  ```
  ![Gambar 15](assets/Gambar15.png)
  
### 6.	Diberikan query berikut. Pada kolom harga belum ada index. Apakah query berikut sudah optimal?? Jelaskan langkah2 optimasinya.
```sql
SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CB001';
```
![Gambar 16](assets/Gambar16.png)

## Pembahasan
Setelah menjalankan berbagai teknik optimasi di atas, diperoleh beberapa temuan:
1. Menambahkan indeks pada kolom yang sering digunakan dalam WHERE dan JOIN dapat mempercepat proses pencarian data secara signifikan.
2. Penggunaan BETWEEN lebih efisien dibandingkan dengan fungsi YEAR(), karena fungsi agregasi pada WHERE memperlambat kinerja database.
3. FULLTEXT Index sangat berguna untuk pencarian teks panjang, dibandingkan dengan LIKE '%keyword%', yang memerlukan pemindaian seluruh tabel.
4. Indeks pada kolom harga meningkatkan efisiensi dalam pencarian nilai maksimum dengan MAX().

## Kesimpulan
1. Teknik optimasi database, terutama dengan penggunaan indeks, dapat secara signifikan meningkatkan performa query.
2. Penggunaan indeks harus disesuaikan dengan pola akses data agar tidak membebani penyimpanan dan performa database.
3. Query yang memanfaatkan indeks dengan baik dapat mengurangi waktu eksekusi secara drastis, terutama pada dataset yang besar.
4. Pencarian teks lebih optimal menggunakan FULLTEXT dibandingkan dengan LIKE '%keyword%'.
5. Pemilihan metode optimasi yang tepat bergantung pada karakteristik tabel dan pola query yang sering digunakan.
