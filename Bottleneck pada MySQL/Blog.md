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
