## OPTIMASI DATABASE DENGAN PARTISI TABEL

### Latar Belakang Pembahasan
Dalam manajemen basis data, performa kueri menjadi faktor penting terutama ketika menangani volume data yang besar. Salah satu teknik optimasi yang umum digunakan adalah partisi tabel, di mana data dibagi ke dalam beberapa segmen yang lebih kecil berdasarkan kriteria tertentu. Teknik ini bertujuan untuk meningkatkan efisiensi pencarian, pemrosesan data, dan penggunaan sumber daya.

Dalam konteks studi kasus database minimarket, teknik partisi tabel diterapkan pada tabel tr_penjualan untuk mengoptimalkan pengelolaan transaksi penjualan. Dengan membagi data berdasarkan periode waktu atau kriteria lainnya, sistem dapat mempercepat akses terhadap informasi yang relevan dan mengurangi beban kerja pada server database. Selain itu, implementasi partisi tabel juga membantu dalam strategi pemeliharaan data, seperti proses backup dan archiving yang lebih efisien.

Melalui pembahasan ini, diharapkan dapat dipahami bagaimana partisi tabel dapat meningkatkan kinerja sistem database, serta bagaimana penerapannya dapat disesuaikan dengan kebutuhan operasional minimarket yang terus berkembang.

### Problem yang Diangkat
Permasalahan utama dalam dokumen ini adalah bagaimana mengoptimalkan performa database dengan teknik partisi tabel. Basis data yang besar sering mengalami kinerja yang menurun saat melakukan pencarian atau manipulasi data. Oleh karena itu, diperlukan strategi yang tepat untuk meningkatkan efisiensi akses data tanpa mengorbankan integritas dan keakuratan informasi.

### Solusi/Skenario Aktivitas (Soal yang Dikerjakan)

Berikut adalah file dalam format Markdown (`.md`) berdasarkan teks yang kamu berikan. Kamu bisa menyalinnya ke file `README.md` atau file lain di repositori GitHub.  

---

### **Optimasi Query dengan Partisi Tabel di MySQL**

## **1. Skenario Sebelum Partisi**
Misalkan kita memiliki tabel transaksi (`transactions`) dengan 10 juta baris. Saat melakukan query seperti:

### **Query:**
```sql
SELECT * FROM transactions 
WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
```

### **Penjelasan:**  
Query ini mengambil semua data dari tabel `transactions` dengan transaksi yang terjadi antara **1 Januari 2023** sampai **31 Desember 2023**.

### **Hasil:**  
![image](https://github.com/user-attachments/assets/55703353-d1de-493b-8a61-4f89297bbc3d)

Karena tidak ada partisi, MySQL harus memeriksa seluruh tabel, yang bisa sangat lambat.

---

## **2. Skenario Setelah Partisi**
Sekarang kita membagi tabel `transactions` berdasarkan tahun menggunakan **RANGE PARTITION**:

### **Query:**
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

### **Penjelasan:**  
Struktur tabel ini mempercepat pencarian data berdasarkan **tahun transaksi** dengan menggunakan partisi.
![image](https://github.com/user-attachments/assets/39a0d209-db1a-44fa-8a48-f138e7f0b8cc)

---

## **3. Memasukkan Data ke Tabel yang Dipartisi**
### **Query:**
```sql
INSERT INTO transactions (transaction_date, amount, customer_id) VALUES
('2023-03-01', 500000, 1),
('2023-05-15', 750000, 2),
('2022-07-10', 200000, 3);
```

### **Query Mengecek Jumlah Data**
```sql
SELECT COUNT(*) FROM transactions;
```

Jika tabel sebelumnya kosong, hasilnya adalah `3`. Jika sudah ada data sebelumnya, hasilnya adalah jumlah total transaksi yang ada.
![image](https://github.com/user-attachments/assets/1018b4d7-07d9-43a2-9d56-9b8e96cd3f60)

---

## **4. Mengecek Partisi yang Digunakan**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'transactions';
```

Jika `transactions` menggunakan partisi, query ini akan menampilkan daftar partisi beserta jumlah datanya. Jika tabel tidak dipartisi, hasilnya kosong.
![image](https://github.com/user-attachments/assets/6d627e9b-1a22-4353-88cb-6fe8cd6da17f)

---

## **5. Optimasi Query dengan Partisi**
```sql
SELECT * FROM transactions 
WHERE transaction_date BETWEEN '2023-01-01' AND '2023-12-31';
```

**Keuntungan**: MySQL hanya akan mencari di **partisi p3** (data tahun 2023), bukan seluruh tabel, sehingga query menjadi jauh lebih cepat.
![image](https://github.com/user-attachments/assets/03792672-25c5-4bcd-8d52-8229edfaf38f)

---

## **6. Manfaat Partisi Tabel**
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

## **7. List Partition dalam MySQL**
List Partition digunakan untuk membagi tabel berdasarkan **nilai tertentu** pada suatu kolom, seperti kode wilayah.

### **Studi Kasus: Partisi Data Transaksi Berdasarkan Wilayah**
Misalkan kita memiliki tabel `transactions1` yang menyimpan transaksi berdasarkan kode wilayah (`region_id`).

### **Membuat Tabel dengan List Partition**
```sql
CREATE TABLE transactions1 (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL, 
    amount DECIMAL(10,2) NOT NULL,
    region_id INT NOT NULL,
    PRIMARY KEY (id, region_id)
)
PARTITION BY LIST (region_id) (
    PARTITION p_jakarta VALUES IN (1),
    PARTITION p_surabaya VALUES IN (2),
    PARTITION p_bandung VALUES IN (3),
    PARTITION p_other VALUES IN (4,5,6,7)
);
```
![image](https://github.com/user-attachments/assets/fd198256-0f3f-4dd5-a924-92bd4b502c6a)

---
Berikut adalah file dalam format Markdown (`.md`) yang bisa langsung kamu unggah ke GitHub:  

---
```
# **LIST PARTITION PADA PARTISI TABEL DALAM MYSQL**

## **1. Pengertian List Partition**
List Partition adalah metode partisi di MySQL yang digunakan untuk membagi tabel berdasarkan **nilai tertentu pada suatu kolom**. Biasanya digunakan ketika data memiliki **kategori diskrit**, seperti:
- **Kode wilayah**
- **Tipe produk**
- **Cabang perusahaan**

---

## **2. Studi Kasus: Partisi Data Transaksi Berdasarkan Kode Wilayah**
Misalkan kita memiliki database yang menyimpan transaksi keuangan, dan kita ingin membagi data transaksi berdasarkan **wilayah (`region_id`)**.

### **Struktur Tabel**
Tabel `transactions1` memiliki kolom:
- `id` → **ID transaksi** (Primary Key)
- `transaction_date` → **Tanggal transaksi**
- `amount` → **Total transaksi**
- `region_id` → **Kode wilayah**  
  _(Misalnya: 1 = Jakarta, 2 = Surabaya, 3 = Bandung, dst.)_

Kita akan membuat **partisi berdasarkan wilayah** menggunakan **List Partition**.

---

## **3. Membuat Tabel dengan List Partition**
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

### **Hasil:**
Tabel `transactions1` berhasil dibuat dengan partisi berdasarkan **kode wilayah**.
![image](https://github.com/user-attachments/assets/f648c707-b1cd-412b-b92d-35b4cabca6a3)

---

## **4. Menambahkan Data ke Tabel**
```sql
INSERT INTO transactions1 (transaction_date, amount, region_id) VALUES 
('2024-03-01', 500000, 1),  -- Jakarta (p_jakarta)
('2024-03-02', 750000, 2),  -- Surabaya (p_surabaya)
('2024-03-03', 200000, 3),  -- Bandung (p_bandung)
('2024-03-04', 300000, 5),  -- Wilayah Lainnya (p_other)
('2024-03-05', 450000, 6);  -- Wilayah Lainnya (p_other)
```

### **Hasil:**
Data transaksi berhasil dimasukkan ke dalam **partisi yang sesuai berdasarkan region_id**.
![image](https://github.com/user-attachments/assets/2478a219-41eb-42f5-8c70-4e1db554c694)

---

## **5. Mengecek Partisi yang Digunakan**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS 
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'transactions1';
```

### **Hasil:**
Query ini akan menampilkan daftar **partisi beserta jumlah baris data dalam masing-masing partisi**.
![image](https://github.com/user-attachments/assets/974e66c7-a7e3-48e1-9175-2e31a6859618)

---

## **6. Menjalankan Query dengan Optimasi Partisi**
```sql
SELECT * FROM transactions1 
WHERE region_id = 2;
```

### **Hasil:**
Query ini hanya mencari di **partisi `p_surabaya`**, bukan di seluruh tabel.
![image](https://github.com/user-attachments/assets/e3149ca6-9fe3-4414-94c4-79f058244fd0)

### **Keuntungan:**
✅ **MySQL hanya mencari di partisi `p_surabaya` (lebih cepat).**  
✅ **Tidak perlu melakukan full table scan pada semua data.**

---

## **7. Menambahkan Partisi Baru**
Jika kita ingin menambahkan wilayah baru, misalnya `region_id = 8` untuk **Semarang**, kita bisa menambahkan partisi baru:

```sql
ALTER TABLE transactions1
ADD PARTITION (PARTITION p_semarang VALUES IN (8));
```

### **Hasil:**
Partisi baru `p_semarang` berhasil ditambahkan.
![image](https://github.com/user-attachments/assets/037aa2e4-4222-4a22-ad1b-08bb2360dc00)

---

## **8. Menghapus Partisi Lama**
Jika kita ingin **menghapus partisi untuk wilayah yang sudah tidak digunakan**, misalnya **p_bandung**, kita bisa menggunakan perintah berikut:

```sql
ALTER TABLE transactions1
DROP PARTITION p_bandung;
```

### **Hasil:**
- **Semua data dalam partisi `p_bandung` akan dihapus!**
- **Pastikan tidak ada data penting sebelum menjalankan perintah ini.**
![image](https://github.com/user-attachments/assets/6014f8e5-cf6a-4304-a48c-0b617a54c360)

---

## **9. Kesimpulan**
✅ **List Partition sangat efektif untuk data dengan kategori tetap** seperti **wilayah, jenis produk, atau status pelanggan**.  
✅ **Mempercepat pencarian data**, karena MySQL hanya membaca **partisi yang relevan**.  
✅ **Mempermudah manajemen data**, karena kita bisa **menambahkan atau menghapus partisi tanpa mempengaruhi data lain**.  
✅ **Cocok untuk database dengan skala besar**, karena meminimalkan penggunaan indeks global dan mempercepat proses query.  

Berikut adalah file dalam format Markdown (`.md`) yang bisa Anda gunakan untuk dokumentasi di GitHub.  

---

## **LATIHAN STUDI KASUS - PARTISI TABEL PADA DATABASE MINIMARKET**

### **1. Konsep dan Pemahaman**
- Menjalankan contoh script untuk memahami konsep partisi, cara kerja partisi, dan manfaatnya.
- Studi kasus mengacu pada database minimarket.

---

### **2. Redesain Tabel `tr_penjualan` dengan Partisi**
#### **Membuat Tabel `tr_penjualan_partisi`**
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
**Penjelasan:**  
Meningkatkan efisiensi pencarian data transaksi berdasarkan tahun dengan sistem partisi.
![image](https://github.com/user-attachments/assets/1ef4f2d2-6c07-4948-9cac-fbde07eae6ee)

#### **Menambahkan Kolom pada `tr_penjualan`**
```sql
ALTER TABLE tr_penjualan ADD COLUMN nama_kasir VARCHAR(255);
ALTER TABLE tr_penjualan ADD COLUMN harga INT(6);
```
**Penjelasan:**  
•	Query ini memindahkan data dari tabel tr_penjualan ke tabel tr_penjualan_partisi.
•	Query ini digunakan untuk memperbarui struktur tabel agar bisa menyimpan nama kasir dan harga barang dalam transaksi penjualan.
![Uploading image.png…]()

---

### **3. Memasukkan Data ke `tr_penjualan_partisi`**
#### **Script Insert Data**
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga) 
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```
**Penjelasan:**  
•	Query ini memindahkan data dari tr_penjualan ke tr_penjualan_partisi, tetapi dengan menambah 1 tahun pada kolom tgl_transaksi.
•	Query ini digunakan untuk memigrasikan data dari tabel lama (tanpa partisi) ke tabel baru (dengan partisi berdasarkan tahun tgl_transaksi).
![image](https://github.com/user-attachments/assets/7f700373-ad98-4f3a-a358-684f66cd38e6)

#### **Menambah Tahun ke Data Transaksi**
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 1 YEAR), kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, nama_kasir, harga
FROM tr_penjualan;
```
**Penjelasan:**  
Menambahkan 1 tahun pada setiap transaksi sebelum dimasukkan ke tabel `tr_penjualan_partisi`.

#### **Menambahkan Partisi Baru untuk Tahun 2016**
```sql
ALTER TABLE tr_penjualan_partisi 
ADD PARTITION (PARTITION p8 VALUES LESS THAN (2016));
```
**Penjelasan:**  
Menambahkan partisi baru untuk menyimpan data transaksi sebelum tahun 2016.

---

### **4. Mengecek Data dalam Partisi**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS 
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'tr_penjualan_partisi';
```
**Penjelasan:**  
Mengecek distribusi data di dalam partisi.

---

### **5. Membuat Tabel `tr_penjualan_raw` (Tanpa Partisi)**
```sql
CREATE TABLE tr_penjualan_raw AS
SELECT * FROM tr_penjualan_partisi;
```
**Penjelasan:**  
Membuat tabel tanpa partisi (`tr_penjualan_raw`) yang isinya sama dengan `tr_penjualan_partisi`.

---

### **6. Pengujian Performa Query**
#### **Query dengan `tgl_transaksi`**
```sql
SELECT * FROM tr_penjualan_raw
WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31');
```
```sql
SELECT * FROM tr_penjualan_partisi
WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31');
```
**Hasil Waktu Eksekusi (Rata-rata dalam detik):**
| No | tr_penjualan_raw | tr_penjualan_partisi |
|----|------------------|----------------------|
| 1  | 0.097 sec       | 0.347 sec            |
| 2  | 0.093 sec       | 0.646 sec            |
| 3  | 0.094 sec       | 0.690 sec            |
| 4  | 0.091 sec       | 0.656 sec            |
| 5  | 0.086 sec       | 0.714 sec            |
| 6  | 0.094 sec       | 0.626 sec            |
| 7  | 0.090 sec       | 0.622 sec            |
| 8  | 0.085 sec       | 0.727 sec            |
| 9  | 0.106 sec       | 0.722 sec            |
| 10 | 0.088 sec       | 0.705 sec            |
| **Rata-rata** | **0.0924 sec** | **0.6455 sec** |

---

#### **Query dengan `kode_cabang`**
```sql
SELECT * FROM tr_penjualan_raw WHERE kode_cabang = 'CAB001';
```
```sql
SELECT * FROM tr_penjualan_partisi WHERE kode_cabang = 'CAB001';
```
**Hasil Waktu Eksekusi (Rata-rata dalam detik):**
| No | tr_penjualan_raw | tr_penjualan_partisi |
|----|------------------|----------------------|
| 1  | 2.673 sec       | 2.591 sec            |
| 2  | 2.669 sec       | 2.545 sec            |
| 3  | 2.685 sec       | 2.538 sec            |
| 4  | 2.571 sec       | 2.550 sec            |
| 5  | 2.588 sec       | 2.591 sec            |
| 6  | 2.629 sec       | 2.540 sec            |
| 7  | 2.694 sec       | 2.456 sec            |
| 8  | 2.658 sec       | 2.029 sec            |
| 9  | 2.601 sec       | 2.561 sec            |
| 10 | 2.634 sec       | 2.559 sec            |
| **Rata-rata** | **2.6402 sec** | **2.436 sec** |

---

### **7. Kesimpulan**
1. **Optimasi Query:**
   - Partisi tabel mempercepat pencarian data berdasarkan **tgl_transaksi**.
   - Namun, untuk query yang tidak menggunakan kolom partisi (misalnya `kode_cabang`), perbedaannya tidak signifikan.

2. **Manfaat Partisi:**
   - **Peningkatan Performa Query** → Query lebih cepat jika menggunakan filter berdasarkan kolom partisi.
   - **Pengelolaan Data yang Lebih Efisien** → Data dapat diatur dengan lebih baik.
   - **Penghapusan Data Lebih Mudah** → Cukup dengan `DROP PARTITION`, tanpa harus menghapus satu per satu.
   - **Pengurangan Beban Indeks** → Setiap partisi memiliki indeks sendiri.
   - **Distribusi Beban Kerja** → Meningkatkan efisiensi storage dan RAM.

**Kesimpulan Akhir:**  
Partisi tabel sangat berguna untuk database besar yang sering melakukan query berdasarkan **kolom yang digunakan dalam partisi**. Namun, untuk query yang tidak menggunakan kolom partisi, peningkatan performanya tidak terlalu signifikan.

---

