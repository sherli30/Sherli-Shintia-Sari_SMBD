# OPTIMASI DATABASE DENGAN PARTISI TABEL

# LATAR BELAKANG PEMBAHASAN

Dalam manajemen basis data, performa kueri menjadi faktor penting terutama ketika menangani volume data yang besar. Salah satu teknik optimasi yang umum digunakan adalah partisi tabel, di mana data dibagi ke dalam beberapa segmen yang lebih kecil berdasarkan kriteria tertentu. Teknik ini bertujuan untuk meningkatkan efisiensi pencarian, pemrosesan data, dan penggunaan sumber daya.

Dalam konteks studi kasus database minimarket, teknik partisi tabel diterapkan pada tabel tr_penjualan untuk mengoptimalkan pengelolaan transaksi penjualan. Dengan membagi data berdasarkan periode waktu atau kriteria lainnya, sistem dapat mempercepat akses terhadap informasi yang relevan dan mengurangi beban kerja pada server database. Selain itu, implementasi partisi tabel juga membantu dalam strategi pemeliharaan data, seperti proses backup dan archiving yang lebih efisien.

Melalui pembahasan ini, diharapkan dapat dipahami bagaimana partisi tabel dapat meningkatkan kinerja sistem database, serta bagaimana penerapannya dapat disesuaikan dengan kebutuhan operasional minimarket yang terus berkembang.

---

# PROBLEM YANG DIANGKAT

Permasalahan utama dalam dokumen ini adalah bagaimana mengoptimalkan performa database dengan teknik partisi tabel. Basis data yang besar sering mengalami kinerja yang menurun saat melakukan pencarian atau manipulasi data. Oleh karena itu, diperlukan strategi yang tepat untuk meningkatkan efisiensi akses data tanpa mengorbankan integritas dan keakuratan informasi.

---

# SOLUSI/SKENARIO AKTIVITAS

# Optimasi Query dengan Partisi Tabel di MySQL

**1. Skenario Sebelum Partisi**

Misalkan kita memiliki tabel transaksi (`transactions`) dengan 10 juta baris. Saat melakukan query seperti:

**> Query :**
```sql
SELECT * FROM transactions 
WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
```

**> Penjelasan :**  

Query ini mengambil semua data dari tabel transactions dengan transaksi yang terjadi antara 1 Januari 2023 sampai 31 Desember 2023. Jadi, hasilnya adalah daftar transaksi selama tahun 2023.

**> Hasil :**  

![image](https://github.com/user-attachments/assets/55703353-d1de-493b-8a61-4f89297bbc3d)

Karena tidak ada partisi, MySQL harus memeriksa seluruh tabel, yang bisa sangat lambat.

---

**2. Skenario Setelah Partisi**

Sekarang kita membagi tabel `transactions` berdasarkan tahun menggunakan **RANGE PARTITION**:

**> Query :**
```sql
CREATE TABLE transactions (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL, 
    amount DECIMAL(10,2) NOT NULL,
    customer_id INT NOT NULL,
    PRIMARY KEY (id, transaction_date)
)
PARTITION BY RANGE (YEAR(transaction_date)) ( 
    PARTITION p0 VALUES LESS THAN (2020), 
    PARTITION p1 VALUES LESS THAN (2021), 
    PARTITION p2 VALUES LESS THAN (2022), 
    PARTITION p3 VALUES LESS THAN (2023), 
    PARTITION p4 VALUES LESS THAN (2024)
);
```

**> Penjelasan :**  

Query ini membuat tabel `transactions` dengan partisi berdasarkan tahun `transaction_date`. Tabel memiliki kolom `id` (auto-increment), `transaction_date`, `amount`, dan `customer_id`, dengan kunci utama (`id`, `transaction_date`). Data dikelompokkan ke dalam lima partisi berdasarkan tahun, dari sebelum 2020 hingga sebelum 2024, untuk optimasi pencarian dan pengelolaan data.

**> Hasil :**

![image](https://github.com/user-attachments/assets/39a0d209-db1a-44fa-8a48-f138e7f0b8cc)

---

**3. Memasukkan Data ke Tabel yang Dipartisi**
**> Query :**
```sql
INSERT INTO transactions (transaction_date, amount, customer_id) VALUES
('2023-03-01', 500000, 1),
('2023-05-15', 750000, 2),
('2022-07-10', 200000, 3);
```

**> Penjelasan :**

Query ini menambahkan tiga transaksi ke dalam tabel `transactions`, masing-masing berisi tanggal transaksi, jumlah, dan ID pelanggan. Data akan otomatis masuk ke partisi yang sesuai berdasarkan tahun `transaction_date`.

**> Query Mengecek Jumlah Data :**
```sql
SELECT COUNT(*) FROM transactions;
```

**> Penjelasan :**

Query ini menghitung dan menampilkan total jumlah transaksi yang tersimpan dalam tabel `transactions`, termasuk semua partisi jika tabel dipartisi.

**> Kesimpulan :**

Query pertama menambahkan tiga transaksi ke dalam tabel `transactions`, lalu query kedua menghitung total transaksi dalam tabel tersebut. Jika tabel sebelumnya kosong, hasilnya adalah `3`. Jika sudah ada data sebelumnya, hasilnya adalah jumlah total transaksi yang ada.

**> Hasil :**

![image](https://github.com/user-attachments/assets/1018b4d7-07d9-43a2-9d56-9b8e96cd3f60)

---

**4. Mengecek Partisi yang Digunakan**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'transactions';
```

**> Penjelasan :**

Query ini menampilkan informasi partisi pada tabel `transactions`, termasuk nama tabel, nama partisi, dan perkiraan jumlah baris dalam setiap partisi. Jika tabel tidak dipartisi, hasilnya bisa kosong atau hanya satu entri.

**> Hasil :**

![image](https://github.com/user-attachments/assets/6d627e9b-1a22-4353-88cb-6fe8cd6da17f)

---

**5. Optimasi Query dengan Partisi**
```sql
SELECT * FROM transactions 
WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
```

**> Penjelasan :**

Query ini mengambil semua transaksi dari tabel `transactions` yang terjadi dalam rentang waktu **1 Januari 2023 hingga 31 Desember 2023**. Jika tabel dipartisi berdasarkan tahun, pencarian akan lebih efisien karena hanya memproses partisi yang relevan.

**> Hasil :**

MySQL hanya akan mencari di partisi p3 (data tahun 2023), bukan seluruh tabel, sehingga query menjadi jauh lebih cepat.

![image](https://github.com/user-attachments/assets/03792672-25c5-4bcd-8d52-8229edfaf38f)

---

**6. Manfaat Partisi Tabel**
1. **Meningkatkan Performa Query**  
   → Dengan hanya mencari di partisi yang relevan, waktu eksekusi query berkurang signifikan.
2. **Mempermudah Manajemen Data**  
   → Data lama bisa dihapus dengan `ALTER TABLE ... DROP PARTITION`, tanpa perlu menghapus satu per satu.
3. **Meminimalkan Beban Indeks**  
   → Karena setiap partisi memiliki indeksnya sendiri, MySQL tidak harus mencari di seluruh indeks utama.
4. **Mempercepat Proses Backup dan Restore**  
   → Backup dan restore dapat dilakukan hanya pada partisi tertentu.
5. **Mendukung Performa Tinggi untuk Big Data**  
   → Untuk tabel dengan jutaan hingga miliaran baris, partisi membuat penyimpanan dan akses data lebih efisien.

---

# LIST PARTITION PADA PARTISI TABEL DALAM MYSQL

List Partition adalah metode partisi di MySQL yang digunakan untuk membagi tabel berdasarkan nilai tertentu pada suatu kolom. Biasanya digunakan ketika data memiliki kategori diskrit, seperti kode wilayah, tipe produk, atau cabang perusahaan.

**> Studi Kasus: Partisi Data Transaksi Berdasarkan Wilayah**

Misalkan kita memiliki tabel `transactions1` yang menyimpan transaksi berdasarkan kode wilayah (`region_id`).

**> Tabel transactions memiliki kolom:**
- id → ID transaksi (Primary Key)
- transaction_date → Tanggal transaksi
- amount → Total transaksi
- region_id → Kode wilayah (Misalnya: 1 = Jakarta, 2 = Surabaya, 3 = Bandung, dst.) Kita akan membuat partisi berdasarkan wilayah menggunakan List Partition.

---

# Membuat Tabel dengan List Partition

**1. Pengertian List Partition**

List Partition adalah metode partisi di MySQL yang digunakan untuk membagi tabel berdasarkan **nilai tertentu pada suatu kolom**. Biasanya digunakan ketika data memiliki **kategori diskrit**, seperti:
- **Kode wilayah**
- **Tipe produk**
- **Cabang perusahaan**

---

**2. Studi Kasus: Partisi Data Transaksi Berdasarkan Kode Wilayah**

Misalkan kita memiliki database yang menyimpan transaksi keuangan, dan kita ingin membagi data transaksi berdasarkan **wilayah (`region_id`)**.

**> Struktur Tabel**
Tabel `transactions1` memiliki kolom:
- `id` → **ID transaksi** (Primary Key)
- `transaction_date` → **Tanggal transaksi**
- `amount` → **Total transaksi**
- `region_id` → **Kode wilayah**  
  _(Misalnya: 1 = Jakarta, 2 = Surabaya, 3 = Bandung, dst.)_

Kita akan membuat **partisi berdasarkan wilayah** menggunakan **List Partition**.

---

**3. Membuat Tabel dengan List Partition**
```sql
CREATE TABLE transactions1 (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL, 
    amount DECIMAL(10,2) NOT NULL,
    region_id INT NOT NULL,
    PRIMARY KEY (id, region_id)
)
PARTITION BY LIST (region_id) (
    PARTITION p_jakarta VALUES IN (1),      -- Wilayah Jakarta
    PARTITION p_surabaya VALUES IN (2),     -- Wilayah Surabaya
    PARTITION p_bandung VALUES IN (3),      -- Wilayah Bandung
    PARTITION p_other VALUES IN (4,5,6,7)   -- Wilayah lainnya
);
```

**> Penjelasan :**

Query ini membuat tabel `transactions1` dengan partisi berdasarkan `region_id` menggunakan **LIST partitioning**. Data dibagi ke dalam partisi sesuai wilayah: Jakarta (1), Surabaya (2), Bandung (3), dan lainnya (4,5,6,7), untuk optimasi pencarian dan pengelolaan data.

**> Hasil :**

![image](https://github.com/user-attachments/assets/f648c707-b1cd-412b-b92d-35b4cabca6a3)

---

**4. Menambahkan Data ke Tabel**
```sql
INSERT INTO transactions1 (transaction_date, amount, region_id) VALUES 
('2024-03-01', 500000, 1),  -- Jakarta (p_jakarta)
('2024-03-02', 750000, 2),  -- Surabaya (p_surabaya)
('2024-03-03', 200000, 3),  -- Bandung (p_bandung)
('2024-03-04', 300000, 5),  -- Wilayah Lainnya (p_other)
('2024-03-05', 450000, 6);  -- Wilayah Lainnya (p_other)
```

**> Penjelasan :**

Query ini menambahkan lima transaksi ke dalam tabel `transactions1`, dengan `transaction_date`, `amount`, dan `region_id`. Data otomatis masuk ke partisi sesuai `region_id`: Jakarta (1), Surabaya (2), Bandung (3), dan wilayah lainnya (4,5,6,7) untuk optimasi pencarian dan pengelolaan.

**> Hasil :**

![image](https://github.com/user-attachments/assets/2478a219-41eb-42f5-8c70-4e1db554c694)

---

**5. Mengecek Partisi yang Digunakan**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS 
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'transactions1';
```

**> Penjelasan :**

Query ini menampilkan informasi partisi dalam tabel `transactions1`, termasuk nama tabel, nama partisi, dan perkiraan jumlah baris di setiap partisi. Jika tabel dipartisi, hasilnya akan menunjukkan data per partisi; jika tidak, hasilnya mungkin kosong atau hanya satu entri.

**> Hasil :**

![image](https://github.com/user-attachments/assets/974e66c7-a7e3-48e1-9175-2e31a6859618)

---

**6. Menjalankan Query dengan Optimasi Partisi**
```sql
SELECT * FROM transactions1 
WHERE region_id = 2;
```

**> Penjelasan :**

Query ini mengambil semua transaksi dari tabel `transactions1` yang memiliki `region_id = 2`, yaitu transaksi yang masuk dalam partisi **p_surabaya**. Jika tabel dipartisi, pencarian akan lebih efisien karena hanya memproses partisi yang sesuai.

**> Hasil :**

![image](https://github.com/user-attachments/assets/e3149ca6-9fe3-4414-94c4-79f058244fd0)

**> Keuntungan :**

✅ MySQL hanya mencari di partisi `p_surabaya` (lebih cepat). 

✅ Tidak perlu melakukan full table scan pada semua data.

---

**7. Menambahkan Partisi Baru**

Jika kita ingin menambahkan wilayah baru, misalnya `region_id = 8` untuk **Semarang**, kita bisa menambahkan partisi baru:

```sql
ALTER TABLE transactions1
ADD PARTITION (PARTITION p_semarang VALUES IN (8));
```

**> Penjelasan :**

Query ini mencoba menambahkan partisi `p_semarang` untuk `region_id = 8`, tetapi MySQL tidak mendukung ALTER TABLE untuk menambah partisi LIST. Solusinya, buat ulang tabel dengan semua partisi yang diinginkan, lalu pindahkan datanya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/037aa2e4-4222-4a22-ad1b-08bb2360dc00)

---

**8. Menghapus Partisi Lama**

Jika kita ingin **menghapus partisi untuk wilayah yang sudah tidak digunakan**, misalnya **p_bandung**, kita bisa menggunakan perintah berikut:

```sql
ALTER TABLE transactions1
DROP PARTITION p_bandung;
```

**> Penjelasan :**

Query ini mencoba menghapus partisi `p_bandung` dari tabel `transactions1`, tetapi **menghapus partisi akan menghapus semua data di dalamnya**. Pastikan data sudah dicadangkan sebelum menjalankan perintah ini.

**> Hasil :**

![image](https://github.com/user-attachments/assets/6014f8e5-cf6a-4304-a48c-0b617a54c360)

---
# Catatan :
- Akan menghapus semua data di dalam partisi tersebut!
- List Partition sangat efektif untuk data yang memiliki kategori tetap seperti wilayah, jenis produk, atau status pelanggan.
- Mempercepat pencarian data karena MySQL hanya membaca partisi yang relevan.
- Mempermudah manajemen data dengan memungkinkan penghapusan data dalam partisi tertentu tanpa mempengaruhi data lain.
- Dengan metode ini, database lebih efisien dalam menangani data dalam skala besar!

# Kesimpulan : List Partition di MySQL

List Partition di MySQL digunakan untuk membagi tabel berdasarkan nilai tertentu, seperti `region_id`, sehingga pencarian data lebih cepat dan efisien.

- **1. Pembuatan Partisi**
Data dibagi ke dalam partisi berdasarkan wilayah.
- **2. Penyisipan Data**
Data otomatis masuk ke partisi sesuai `region_id`.
- **3. Pengecekan Partisi**
Bisa melihat jumlah data di setiap partisi.
- **4. Optimasi Query**
Pencarian hanya dilakukan di partisi yang sesuai, bukan seluruh tabel.
- **5. Penambahan Partisi**
Tidak bisa langsung, harus membuat ulang tabel.
- **6. Penghapusan Partisi**
Data di partisi yang dihapus akan hilang.

**> Keuntungan :**

✅ Query lebih cepat  

✅ Manajemen data lebih mudah  

✅ Mengurangi beban sistem saat mengambil data dalam jumlah besar

---

# LATIHAN STUDI KASUS - PARTISI TABEL PADA DATABASE MINIMARKET

**> 1. Konsep dan Pemahaman**

1.	Jalankan semua contoh script diatas sampai anda benar-benar memahami konsep partisi, cara kerja partisi, manfaat partisi tabel.
2.	Studi kasus mengacu pada database minimarket.

---

**> 2. Redesain Tabel `tr_penjualan` dengan Partisi**

3.	Pada tabel tr_penjualan, lakukan scenario sebagai berikut:
- a. Redesaian tabel tr_penjualan, tambahkan partisi pada tabel tersebut. Sehingga ada tabel baru tr_penjualan_partisi

**> Membuat Tabel `tr_penjualan_partisi`**
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

**> Penjelasan :**  

Query ini membuat tabel `tr_penjualan_partisi` dengan partisi berdasarkan tahun `tgl_transaksi`, membagi data dari sebelum 2008 hingga sebelum 2015 untuk optimasi pencarian dan pengelolaan transaksi.

**> Hasil :**

![image](https://github.com/user-attachments/assets/1ef4f2d2-6c07-4948-9cac-fbde07eae6ee)

---

**> Menambahkan Kolom pada `tr_penjualan`**
```sql
ALTER TABLE tr_penjualan ADD COLUMN nama_kasir VARCHAR(255);
ALTER TABLE tr_penjualan ADD COLUMN harga INT(6);
```

**> Penjelasan :**  

Query ini menambahkan kolom `nama_kasir` (VARCHAR 255) dan `harga` (INT 6) ke tabel `tr_penjualan` untuk menyimpan informasi kasir dan harga dalam setiap transaksi.

**> Hasil :**

![image](https://github.com/user-attachments/assets/8f1bb733-ccd4-4c7f-928e-ae74e798eb02)

---

**3. Memasukkan Data ke `tr_penjualan_partisi`**

**> Script insert utk tahun 2008**
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga) 
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**  

Query ini memasukkan data dari `tr_penjualan` ke `tr_penjualan_partisi`, tetapi harus ditambahkan **WHERE YEAR(tgl_transaksi) = 2008** agar hanya data tahun 2008 yang dimasukkan.

**> Hasil :**

![image](https://github.com/user-attachments/assets/7f700373-ad98-4f3a-a358-684f66cd38e6)

---

**> Menambah Tahun ke Data Transaksi**

# Script insert utk tahun 2009
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 1 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**  

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi`, tetapi dengan **menambahkan 1 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 1 YEAR)`.  

**> Hasil :**

![image](https://github.com/user-attachments/assets/07501d98-cc2e-4667-9e24-0b0497d0e36a)

---

# Script insert utk tahun 2010
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 2 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi`, tetapi **menambahkan 2 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 2 YEAR)`, sehingga semua transaksi yang dimasukkan akan memiliki tahun yang lebih baru dari data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/5dd19a8f-862f-4712-8ef6-d92d5e175bd3)

---

# Script insert utk tahun 2011
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 3 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi`, tetapi **menambahkan 2 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 2 YEAR)`, sehingga semua transaksi yang dimasukkan akan memiliki tahun yang lebih baru dari data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/aead747c-383c-4df4-bfe3-4e026e7f587e)

---

# Script insert utk tahun 2012
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 4 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi` dengan **menambahkan 4 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 4 YEAR)`, sehingga transaksi yang dimasukkan akan memiliki tahun lebih baru dibandingkan data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/67140da3-a6c6-4791-8ffb-193dee1311c5)

---

# Script insert utk tahun 2013
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 5 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi` dengan **menambahkan 5 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 5 YEAR)`, sehingga transaksi yang dimasukkan akan memiliki tahun lebih baru dibandingkan data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/996daed9-925f-4eb4-a11f-9f946ff6331d)

---

# Script insert utk tahun 2014
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 6 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi` dengan **menambahkan 6 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 6 YEAR)`, sehingga transaksi yang dimasukkan akan memiliki tahun yang lebih baru dibandingkan data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/a29e5f9a-6a78-490f-97af-cf3238c6bc9e)

---

# **Menambahkan Partisi Baru untuk Tahun 2016**
```sql
ALTER TABLE tr_penjualan_partisi 
ADD PARTITION (PARTITION p8 VALUES LESS THAN (2016));
```

**> Penjelasan :**  
- Query ini menambahkan partisi baru pada tabel tr_penjualan_partisi untuk menyimpan data transaksi sebelum tahun 2016.
- ALTER TABLE tr_penjualan_partisi → Mengubah struktur tabel.
- ADD PARTITION (PARTITION p8 VALUES LESS THAN (2016)) → Menambahkan partisi baru (p8) untuk transaksi sebelum tahun 2016.

**> Hasil :**

![image](https://github.com/user-attachments/assets/352f8e0c-8026-46b7-bb27-99b712c656e2)

---

# Script insert utk tahun 2015
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 7 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```

**> Penjelasan :**

Query ini menyalin data dari `tr_penjualan` ke `tr_penjualan_partisi` dengan **menambahkan 7 tahun ke `tgl_transaksi`** menggunakan `DATE_ADD(tgl_transaksi, INTERVAL 7 YEAR)`, sehingga transaksi yang dimasukkan akan memiliki tahun lebih baru dibandingkan data aslinya.

**> Hasil :**

![image](https://github.com/user-attachments/assets/a34af869-bd5f-4725-ba57-5e9cf22a793e)

---

c.	Pengisian tabel tr_penjualan_partisi disesuai dengan kapasitas LAPTOP masing- masing. Makin banyak data, makin terlihat efek dari partisi table.
d.	Setelah diisikan, maka susunan record sesuai partisi akan ditampilkan seperti berikut:

**> Query :**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'tr_penjualan_partisi';
```

**> Penjelasan :**
- Query ini berguna untuk mengecek apakah data sudah terdistribusi ke partisi yang benar serta mengetahui jumlah data dalam masing-masing partisi.
- Mengambil data dari INFORMATION_SCHEMA.PARTITIONS, yaitu metadata partisi dalam database.
- Menyaring hanya tabel tr_penjualan_partisi dengan klausa WHERE TABLE_NAME = 'tr_penjualan_partisi'.
- Menampilkan kolom-kolom berikut:
1.	TABLE_NAME → Nama tabel.
2.	PARTITION_NAME → Nama partisi.
3.	TABLE_ROWS → Perkiraan jumlah baris dalam setiap partisi.

**> Hasil :**

![image](https://github.com/user-attachments/assets/1516df36-3fb1-47b3-8b70-320bfb0c3f3e)

---

**4. Buat tabel tr_penjualan_raw yang isinya sama persis dengan tabel tr_penjualan_partisi. Yang membedakan hanya struktur tabel nya saja.**

- Tr_penjualan_raw = struktur biasa 
- Tr_penjualan_partisi = struktur tabel ter partisi

**> Query :**
```sql
CREATE TABLE tr_penjualan_raw AS
SELECT * FROM tr_penjualan_partisi;
```

**> Penjelasan :**
- Membuat tabel baru tr_penjualan_raw berdasarkan struktur dan data dari tr_penjualan_partisi.
- Semua data dari tr_penjualan_partisi akan disalin ke dalam tr_penjualan_raw.
- Tidak termasuk partisi (tabel baru ini tidak memiliki partisi, meskipun tabel asalnya memiliki partisi).

**> Hasil :**

![image](https://github.com/user-attachments/assets/15fdcea6-afb2-4013-aa3b-04b9eb53398b)

---

**5. Pengujian tabel**

Jalankan query berikut dengan perulangan 10x. lakukan pencatatan waktu running setiap query. Dan ambil rata-ratanya.

**> Query :**
```
SELECT * FROM tr_penjualan_raw
WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31')
```

**> Penjelasan :**
- Memilih semua kolom dari tabel tr_penjualan_raw.
- Menyaring data berdasarkan tanggal transaksi (tgl_transaksi):
    1.	Harus lebih besar dari 1 Agustus 2010.
    2.	Harus lebih kecil dari 31 Juli 2011.
- Query ini digunakan untuk menampilkan transaksi dalam periode 1 tahun, yaitu dari Agustus 2010 hingga Juli 2011.

# Tabel Pengujian RAW dan PARTISI dengan Perbandingan Waktu Eksekusi
| No | tr_penjualan_raw | tr_penjualan_partisi |
|----|------------------|----------------------|
| 1  | 0.097 sec        | 0.347 sec            |
| 2  | 0.093 sec        | 0.646 sec            |
| 3  | 0.094 sec        | 0.690 sec            |
| 4  | 0.091 sec        | 0.656 sec            |
| 5  | 0.086 sec        | 0.714 sec            |
| 6  | 0.094 sec        | 0.626 sec            |
| 7  | 0.090 sec        | 0.622 sec            |
| 8  | 0.085 sec        | 0.727 sec            |
| 9  | 0.106 sec        | 0.722 sec            |
| 10 | 0.088 sec        | 0.705 sec            |
| **Rata-rata** | **0.0924 sec** | **0.6455 sec** |

---

**> Query dengan `tgl_transaksi`**
```sql
SELECT * FROM tr_penjualan_partisi
WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31');
```

**> Penjelasan :**
- Memilih semua kolom (*) dari tabel tr_penjualan_partisi.
- Menyaring data berdasarkan tgl_transaksi:
1.	Harus lebih besar dari 1 Agustus 2010.
2.	Harus lebih kecil dari 31 Juli 2011.
- Query ini menampilkan transaksi dalam periode satu tahun, dari Agustus 2010 hingga Juli 2011, pada tabel berpartisi (tr_penjualan_partisi).

---

**6. Lakukan pengujian juga utk kolom lain. Dengan cara menjalankan query yang where clause nya bukan tgl_transaksi. Catat waktunya. Jelaskan Hasil yang anda peroleh??**
   
**> Tanpa Partisi (tr_penjualan_raw)**
```sql
SELECT * FROM tr_penjualan_raw WHERE kode_cabang = 'CAB001';
```

**> Penjelasan :**

- Query ini mencari semua data dengan kode_cabang = 'CAB001' di seluruh tabel tr_penjualan_raw tanpa partisi.
- Kelemahan: Proses pencarian bisa lebih lambat karena harus memindai seluruh tabel (full table scan).

**> Dengan Partisi (tr_penjualan_partisi)**
```sql
SELECT * FROM tr_penjualan_partisi WHERE kode_cabang = 'CAB001';
```

**> Penjelasan :**

- Query ini mencari data di tabel berpartisi (tr_penjualan_partisi).
- Keuntungan: Jika partisi digunakan dengan baik, pencarian bisa lebih cepat karena hanya membaca bagian tabel tertentu (partition pruning).

**> Hasil Waktu Eksekusi (Rata-rata dalam detik):**
| No | tr_penjualan_raw | tr_penjualan_partisi |
|----|------------------|----------------------|
| 1  | 2.673 sec        | 2.591 sec            |
| 2  | 2.669 sec        | 2.545 sec            |
| 3  | 2.685 sec        | 2.538 sec            |
| 4  | 2.571 sec        | 2.550 sec            |
| 5  | 2.588 sec        | 2.591 sec            |
| 6  | 2.629 sec        | 2.540 sec            |
| 7  | 2.694 sec        | 2.456 sec            |
| 8  | 2.658 sec        | 2.029 sec            |
| 9  | 2.601 sec        | 2.561 sec            |
| 10 | 2.634 sec        | 2.559 sec            |
| **Rata-rata** | **2.6402 sec** | **2.436 sec** |

---

**7. Berikan Kesimpulan dari percobaan yang anda lakukan.**

**> Kesimpulan Pengujian :**
- Untuk query dengan filter tgl_transaksi, performa tabel tanpa partisi (tr_penjualan_raw) lebih baik dibandingkan tabel berpartisi (tr_penjualan_partisi). Hal ini disebabkan oleh overhead dalam manajemen partisi, terutama jika jumlah partisi besar dan index tidak optimal.
- Untuk query yang menggunakan filter selain tgl_transaksi, seperti kode_cabang, perbedaan performa antara tabel partisi dan non-partisi tidak signifikan. Bahkan dalam beberapa kasus, tabel partisi lebih lambat karena partition pruning tidak berlaku untuk kolom yang tidak termasuk dalam skema partisi.

----

**> Analisis Penyebab :**
- Overhead Manajemen Partisi: Saat melakukan query pada tabel berpartisi, sistem harus menentukan partisi mana yang akan digunakan, yang bisa menambah waktu eksekusi.
- Partition Pruning Tidak Berlaku untuk Semua Kolom: Partisi berbasis tgl_transaksi tidak membantu jika query menggunakan filter pada kolom lain (kode_cabang), sehingga performanya tetap mirip dengan full table scan.
- Indeks di Tabel Non-Partisi: Jika tabel non-partisi memiliki indeks yang baik pada kolom yang sering dicari (kode_cabang), maka query bisa berjalan lebih cepat dibanding tabel partisi tanpa indeks yang sesuai.

----
  
**> Kesimpulan Akhir Pengujian :**  
- Gunakan partisi jika query sering menyaring data berdasarkan kolom yang digunakan dalam skema partisi (misalnya, tgl_transaksi pada tabel tr_penjualan_partisi).
- Jika query sering menggunakan filter di luar kolom partisi, pertimbangkan untuk menambahkan indeks tambahan atau menghindari partisi yang tidak diperlukan.

---
# PEMBAHASAN

**> Tujuan Utama :**
- Meningkatkan kecepatan query dengan hanya memindai partisi yang relevan.
- Mempermudah manajemen data, seperti penghapusan atau arsip data lama.
- Mempercepat proses backup dan restore.

**> Jenis Partisi :**
- RANGE: Cocok untuk data dengan rentang nilai, seperti tanggal.
- LIST: Cocok untuk data dengan kategori diskrit, seperti kode wilayah.

**> Pertimbangan Penting :**
- Pemilihan kunci partisi yang tepat sesuai pola akses data.
- Keseimbangan antara jumlah partisi dan overhead manajemen.
- Penggunaan indeks yang tepat pada setiap partisi.
- Pengujian menyeluruh sebelum implementasi.

---

# KESIMPULAN SECARA KESELURUHAN
Partisi tabel merupakan teknik optimasi database yang membagi tabel besar menjadi segmen-segmen lebih kecil berdasarkan kriteria tertentu. Dari hasil studi kasus pada database minimarket, diperoleh kesimpulan sebagai berikut:

1. **Efektivitas Partisi Tabel**:
   - Partisi tabel efektif untuk mengoptimalkan performa query yang melibatkan klausa WHERE pada kolom yang digunakan sebagai kunci partisi.
   - Partisi RANGE cocok untuk data yang memiliki rentang nilai seperti tanggal, sedangkan partisi LIST cocok untuk data dengan kategori diskrit seperti kode wilayah.

2. **Kinerja Query**:
   - Berdasarkan pengujian, untuk kueri dengan filter pada kolom kunci partisi (tgl_transaksi), tabel non-partisi justru menunjukkan performa lebih baik.
   - Untuk kueri dengan filter pada kolom non-partisi (kode_cabang), tabel partisi menunjukkan sedikit peningkatan performa.

3. **Faktor yang Mempengaruhi**:
   - Overhead manajemen partisi dapat mempengaruhi kinerja, terutama jika jumlah partisi terlalu banyak.
   - Partition pruning hanya efektif untuk kueri yang melibatkan kolom partisi.
   - Pengindeksan yang tepat tetap diperlukan pada tabel berpartisi.

4. **Manfaat Praktis**:
   - Memudahkan manajemen data, terutama untuk operasi archiving dan penghapusan data lama.
   - Mempercepat proses backup dan restore karena dapat dilakukan per partisi.
   - Meningkatkan performa pada database skala besar dengan pola akses data yang sesuai dengan skema partisi.

5. **Rekomendasi Implementasi**:
   - Gunakan partisi tabel untuk database dengan volume data besar yang terus bertambah.
   - Pilih kolom partisi berdasarkan pola akses data yang paling sering digunakan.
   - Lakukan pengujian menyeluruh sebelum implementasi di lingkungan produksi.
   - Kombinasikan partisi tabel dengan strategi pengindeksan yang tepat untuk hasil optimal.
     
---

# REFERENSI

1. MySQL Documentation. "Partitioning in MySQL." [Online]. Tersedia: https://dev.mysql.com/doc/refman/8.0/en/partitioning.html

2. Baron Schwartz, Peter Zaitsev, & Vadim Tkachenko. (2012). "High Performance MySQL: Optimization, Backups, and Replication." O'Reilly Media.

3. Stephane Faroult & Peter Robson. (2006). "The Art of SQL." O'Reilly Media.

4. Jay Pipes & Michael Kruckenberg. (2005). "Pro MySQL." Apress.

5. MySQL Enterprise White Papers. "MySQL Partitioning: Improving Performance, Availability, and Manageability." [Online]. Tersedia: https://www.mysql.com/why-mysql/white-papers/

6. Andrew J. Holdsworth, Richard A. Loft, & David J. Smith. (2012). "Oracle Database Performance and Scalability: A Quantitative Approach." Wiley Publishing.

7. Giuseppe DeCandia et al. (2007). "Dynamo: Amazon's Highly Available Key-value Store." In Proceedings of Twenty-first ACM SIGOPS Symposium on Operating Systems Principles.

8. Sheeri K. Cabral & Keith Murphy. (2009). "MySQL Administrator's Bible." Wiley Publishing.


