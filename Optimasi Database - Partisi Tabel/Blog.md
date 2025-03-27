### 1.	Jalankan semua contoh script diatas sampai anda benar-benar memahami konsep partisi, cara kerja partisi, manfaat partisi tabel.

  - Query:
    ```sql
    SELECT * FROM transactions WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
    ```

    ```sql
    CREATE TABLE transactions (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL, amount DECIMAL(10,2) NOT NULL,
    customer_id INT NOT NULL, PRIMARY KEY (id, transaction_date)
    )
    PARTITION BY RANGE (YEAR(transaction_date)) (
    PARTITION p0 VALUES LESS THAN (2020),
    PARTITION p1 VALUES LESS THAN (2021),
    PARTITION p2 VALUES LESS THAN (2022),
    PARTITION p3 VALUES LESS THAN (2023),
    PARTITION p4 VALUES LESS THAN (2024)
    );
    ```
    ![Gambar 1](Gambar1.png)

    ```sql
    SELECT * FROM transactions WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
    ```

    ```sql
    CREATE TABLE transactions (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    region_id INT NOT NULL,
    PRIMARY KEY (id, region_id)
    )
    PARTITION BY LIST (region_id) (
    PARTITION p_jakarta VALUES IN (1), -- Wilayah Jakarta
    PARTITION p_surabaya VALUES IN (2), -- Wilayah Surabaya
    PARTITION p_bandung VALUES IN (3), -- Wilayah Bandung
    PARTITION p_other VALUES IN (4,5,6,7) -- Wilayah lainnya
    );
    ```
    ![Gambar 2](Gambar2.png)
    
- Menambahkan Data ke Tabel
  - Query:
    ```sql
    INSERT INTO transactions (transaction_date, amount, region_id) VALUES
    ('2024-03-01', 500000, 1), -- Jakarta (p_jakarta)
    ('2024-03-02', 750000, 2), -- Surabaya (p_surabaya)
    ('2024-03-03', 200000, 3), -- Bandung (p_bandung)
    ('2024-03-04', 300000, 5), -- Wilayah Lainnya (p_other)
    ('2024-03-05', 450000, 6); -- Wilayah Lainnya (p_other)
    ```
    ![Gambar 3](Gambar3.png)
    
- Mengecek Partisi yang Digunakan
  - Query:
    ```sql
    SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS FROM
    INFORMATION_SCHEMA.PARTITIONS
    WHERE TABLE_NAME = 'transactions';
    ```
    ![Gambar 4](Gambar4.png)
    
- Menjalankan Query dengan Optimasi Partisi
  - Query:
    ```sql
    SELECT * FROM transactions WHERE region_id = 2;
    ```
    ![Gambar 5](Gambar5.png)

- Menambahkan Partisi Baru
  - Jika kita ingin menambahkan wilayah baru, misalnya region_id = 8 untuk Semarang, kita bisa menambahkan partisi baru:

    - Query:
      ```sql
      ALTER TABLE transactions
      ADD PARTITION (PARTITION p_semarang VALUES IN (8));
      ```
      ![Gambar 6](Gambar6.png)
      
- Menghapus Partisi Lama
  - Jika kita ingin menghapus partisi untuk wilayah yang sudah tidak digunakan, misalnya p_bandung:

    - Query:
      ```sql
      ALTER TABLE transactions DROP PARTITION p_bandung;
      ```
      ![Gambar 7](Gambar7.png)

### 2.	Studi kasus mengacu pada database minimarket.

### 3.	Pada tabel tr_penjualan, lakukan scenario sebagai berikut:
- Redesaian tabel tr_penjualan, tambahkan partisi pada tabel tersebut. Sehingga ada tabel baru tr_penjualan_partisi

  - Query:
    ```sql
    CREATE TABLE tr_penjualan_partisi (
    tgl_transaksi DATETIME DEFAULT NULL,
    kode_cabang VARCHAR(10) DEFAULT NULL,
    kode_kasir VARCHAR(10) DEFAULT NULL,
    kode_item VARCHAR(7) DEFAULT NULL,
    kode_produk VARCHAR(12) DEFAULT NULL,
    jumlah_pembelian INT(11) DEFAULT NULL,
    nama_kasir VARCHAR(40) DEFAULT NULL,
    harga INT(6) DEFAULT NULL
    )
    PARTITION BY RANGE (YEAR(tgl_transaksi)) (
    PARTITION p0 VALUES LESS THAN (2008),
    PARTITION p1 VALUES LESS THAN (2009),
    PARTITION p2 VALUES LESS THAN (2010),
    PARTITION p3 VALUES LESS THAN (2011),
    PARTITION p4 VALUES LESS THAN (2012),
    PARTITION p5 VALUES LESS THAN (2013),
    PARTITION p6 VALUES LESS THAN (2014),
    PARTITION p7 VALUES LESS THAN (2015)
    );
    ```
    ![Gambar 8](Gambar8.png)

- Isikan tabel tr_penjualan_partisi.

  - Script insert utk tahun 2008:
 
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga) (
    SELECT	tgl_transaksi,	kode_cabang,	kode_kasir,	kode_item,	kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan )
    ```
    ![Gambar 9](Gambar9.png)

  - Script insert utk tahun 2009

    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga) (
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	1	YEAR),	kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    )
    ```
    ![Gambar 10](Gambar10.png)

  - Insert untuk tahun 2010
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga)
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	2	YEAR),	kode_cabang, kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    WHERE YEAR(tgl_transaksi) = 2008;
    ```
    ![Gambar 11](Gambar11.png)

  - Insert untuk tahun 2011
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga)
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	3	YEAR),	kode_cabang, kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    WHERE YEAR(tgl_transaksi) = 2008;
    ```
    ![Gambar 12](Gambar12.png)
 
  - Insert untuk tahun 2012
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga)
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	4	YEAR),	kode_cabang, kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    WHERE YEAR(tgl_transaksi) = 2008;
    ```
    ![Gambar 13](Gambar13.png)

  - Insert untuk tahun 2013
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga)
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	5	YEAR),	kode_cabang, kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    WHERE YEAR(tgl_transaksi) = 2008;
    ```
    ![Gambar 14](Gambar14.png)

  - Insert untuk tahun 2014
    ```sql
    INSERT	INTO	tr_penjualan_partisi	(tgl_transaksi,	kode_cabang,	kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga)
    SELECT	DATE_ADD(tgl_transaksi,	INTERVAL	6	YEAR),	kode_cabang, kode_kasir, kode_item,
    kode_produk, jumlah_pembelian, nama_kasir, harga
    FROM tr_penjualan
    WHERE YEAR(tgl_transaksi) = 2008;
    ```
    ![Gambar 15](Gambar15.png)

- Lakukan penambahan data dummy di tr_penjualan_partisi dengan susunan
  - Tahun 2008 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir,
    kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2008-01-15', 'CB001', 'KSR01', 'IT001', 'PRD001', 2, 'Kasir A', 10000),
    ('2008-03-22', 'CB002', 'KSR02', 'IT002', 'PRD002', 3, 'Kasir B', 15000);
    ```
    ![Gambar 16](Gambar16.png)

  - Tahun 2010 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir,
    kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2010-05-10', 'CB003', 'KSR03', 'IT003', 'PRD003', 5, 'Kasir C', 20000),
    ('2010-07-18', 'CB004', 'KSR04', 'IT004', 'PRD004', 1, 'Kasir D', 25000);
    ```
    ![Gambar 17](Gambar17.png)

  - Tahun 2011 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir,
    kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2011-02-14', 'CB005', 'KSR05', 'IT005', 'PRD005', 4, 'Kasir E', 30000),
    ('2011-08-30', 'CB006', 'KSR06', 'IT006', 'PRD006', 6, 'Kasir F', 35000);
    ```
    ![Gambar 18](Gambar18.png)

  - Tahun 2012 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2012-03-21', 'CB007', 'KSR07', 'IT007', 'PRD007', 3, 'Kasir G', 12000),
    ('2012-11-05', 'CB008', 'KSR08', 'IT008', 'PRD008', 7, 'Kasir H', 22000);
    ```
    ![Gambar 19](Gambar19.png)

  - Tahun 2013 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir,
    kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2013-06-14', 'CB009', 'KSR09', 'IT009', 'PRD009', 2, 'Kasir I', 18000),
    ('2013-09-28', 'CB010', 'KSR10', 'IT010', 'PRD010', 5, 'Kasir J', 28000);
    ```
    ![Gambar 20](Gambar20.png)

  - Tahun 2014 =
    ```sql
    INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir,
    kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
    VALUES
    ('2014-04-12', 'CB011', 'KSR11', 'IT011', 'PRD011', 4, 'Kasir K', 32000),
    ('2014-12-19', 'CB012', 'KSR12', 'IT012', 'PRD012', 6, 'Kasir L', 42000);
    ```
    ![Gambar 21](Gambar21.png)

- Pengisian tabel tr_penjualan_partisi disesuai dengan kapasitas LAPTOP masingmasing. Makin banyak data, makin terlihat efek dari partisi table.

- Setelah diisikan, maka susunan record sesuai partisi akan ditampilkan seperti berikut:
  ```sql
  SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS
  FROM INFORMATION_SCHEMA.PARTITIONS
  WHERE TABLE_NAME = 'tr_penjualan_partisi';
  ```
  ![Gambar 22](Gambar22.png)
 
### 4.	Buat tabel tr_penjualan_raw yang isinya sama persis dengan tabel tr_penjualan_partisi. Yang membedakan hanya struktur tabel nya saja.
- Tr_penjualan_raw = struktur biasa
- Tr_penjualan_partisi = struktur tabel ter partisi
  ```sql
  CREATE TABLE tr_penjualan_raw LIKE tr_penjualan_partisi;
  ```
  ![Gambar 23](Gambar23.png)
 
### 5.	Pengujian table
- Jalankan query berikut dengan perulangan 10x. lakukan pencatatan waktu running setiap query. Dan ambil rata-ratanya.

  ```sql
  SELECT * FROM tr_penjualan_raw
  WHERE tgl_transaksi > DATE('2010-08-01')
  AND tgl_transaksi < DATE('2011-07-31')
  ```
  | No  | Waktu (sec) |
  |---- |------------|
  | 1   | 0.650      |
  | 2   | 0.254      |
  | 3   | 0.260      |
  | 4   | 0.249      |
  | 5   | 0.253      |
  | 6   | 0.223      |
  | 7   | 0.240      |
  | 8   | 0.225      |
  | 9   | 0.242      |
  | 10  | 0.225      |
  | **Rata-rata** | **0.2821** |

  ```sql
  SELECT * FROM tr_penjualan_partisi
  WHERE tgl_transaksi > DATE('2010-08-01')
  AND tgl_transaksi < DATE('2011-07-31')
  ```
  | No  | Waktu (sec) |
  |---- |------------|
  | 1   | 0.409      |
  | 2   | 0.307      |
  | 3   | 0.244      |
  | 4   | 0.265      |
  | 5   | 0.260      |
  | 6   | 0.252      |
  | 7   | 0.276      |
  | 8   | 0.292      |
  | 9   | 0.929      |
  | 10  | 0.253      |
  | **Rata-rata** | **0.3487** |

### 6.	Lakukan pengujian juga utk kolom lain. Dengan cara menjalankan query yang where clause nya bukan tgl_transaksi. Catat waktunya. Jelaskan Hasil yang anda peroleh??

```sql
SELECT * FROM tr_penjualan_raw WHERE kode_cabang = 'CB003';
```
| No  | Waktu (sec) |
|---- |------------|
| 1   | 4.374      |
| 2   | 4.552      |
| 3   | 3.875      |
| 4   | 3.994      |
| 5   | 4.046      |
| 6   | 3.917      |
| 7   | 3.982      |
| 8   | 4.014      |
| 9   | 3.944      |
| 10  | 4.011      |
| **Rata-rata** | **4.0709** |

```sql
SELECT * FROM tr_penjualan_partisi WHERE kode_cabang = 'CB003';
```
| No  | Waktu (sec) |
|---- |------------|
| 1   | 3.838      |
| 2   | 4.170      |
| 3   | 3.883      |
| 4   | 4.022      |
| 5   | 3.955      |
| 6   | 3.819      |
| 7   | 4.506      |
| 8   | 4.224      |
| 9   | 3.930      |
| 10  | 5.302      |
| **Rata-rata** | **4.1649** |

### 7.	Berikan Kesimpulan dari percobaan yang anda lakukan.
- Efektivitas Partisi Tabel
  - Partisi tabel hanya efektif jika query menggunakan kolom yang menjadi kriteria partisi, yaitu tgl_transaksi.
  - Pada pengujian ini, waktu eksekusi query pada tr_penjualan_partisi tidak lebih cepat dibandingkan tr_penjualan_raw, karena dataset masih kecil dan optimasi indeks belum diterapkan sepenuhnya.
- Pengaruh Partisi pada Kolom Selain tgl_transaksi
  - Query yang menggunakan kode_cabang sebagai filter tidak mengalami peningkatan performa pada tabel dengan partisi.
  - Hal ini terjadi karena partisi didasarkan pada tgl_transaksi, bukan kode_cabang, sehingga MySQL tetap harus melakukan pencarian di seluruh partisi.
- Indeks Lebih Efektif untuk Query Kolom Non-Partisi
  - Untuk meningkatkan performa query pada kode_cabang atau kode_kasir, diperlukan indeks tambahan.
  - Contoh optimasi dengan indeks:
    ```sql
    ALTER TABLE tr_penjualan_partisi ADD INDEX idx_kode_cabang (kode_cabang);
    ```
  - Dengan indeks, MySQL dapat mencari data lebih cepat tanpa perlu membaca seluruh tabel.
- Partisi Tabel Lebih Berguna untuk Dataset Besar
  - Keuntungan partisi tabel lebih terasa jika tabel memiliki jutaan hingga miliaran baris.
  - Pada dataset kecil, MySQL masih dapat menangani query dengan baik tanpa partisi, sehingga peningkatan performa belum signifikan.

