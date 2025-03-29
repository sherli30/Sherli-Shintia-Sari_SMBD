# BOTTLENECK PADA MYSQL

# LATAR BELAKANG PEMBAHASAN
Dalam pengelolaan database MySQL, performa sistem menjadi faktor kunci untuk menjaga kecepatan, efisiensi, dan kestabilan dalam memproses data. Namun, seringkali ditemui permasalahan bottleneck yang menyebabkan kinerja database menurun, terutama ketika jumlah data yang dikelola semakin besar dan kompleks. Bottleneck dapat disebabkan oleh berbagai faktor, seperti penggunaan query yang tidak optimal, kurangnya indeks pada kolom yang sering digunakan, ukuran buffer pool yang tidak memadai, hingga konfigurasi server yang belum disesuaikan dengan kebutuhan.

Salah satu contoh bottleneck yang sering terjadi adalah full table scan, yaitu ketika MySQL harus membaca seluruh tabel akibat tidak adanya indeks yang sesuai, sehingga query menjadi lambat. Selain itu, tingginya jumlah koneksi yang tidak dikelola dengan baik juga dapat menyebabkan server kelebihan beban dan memunculkan error seperti Too Many Connections. Masalah lain yang umum adalah deadlock pada transaksi, penggunaan SELECT * tanpa spesifikasi kolom, serta ukuran InnoDB Buffer Pool yang terlalu kecil sehingga mengharuskan MySQL sering membaca dari disk.

Oleh karena itu, diperlukan teknik optimasi yang tepat, seperti menambahkan indeks, memanfaatkan query cache, meningkatkan ukuran buffer pool, dan mengoptimalkan penulisan query. Optimasi ini bertujuan untuk meningkatkan performa database agar lebih responsif, mengurangi waktu eksekusi query, serta meminimalisir penggunaan sumber daya yang berlebihan. Dengan demikian, sistem database dapat bekerja secara lebih efisien dalam menangani berbagai kebutuhan aplikasi maupun layanan yang berjalan di atasnya.

---- 

# PROBLEM YANG DIANGKAT
1. Bagaimana cara mengoptimalkan eksekusi query agar lebih cepat dan efisien?
2. Bagaimana cara menambahkan dan memanfaatkan indeks untuk mempercepat pencarian data?
3. Bagaimana teknik optimasi yang bisa diterapkan pada database transaksi seperti minimarket?
4. Bagaimana cara menangani pencarian data teks (string search) yang lebih efisien?

----

# SOLUSI/SKENARIO AKTIVITAS

**> SOAL LATIHAN**

1.	Jalankan setiap langkah dari materi diatas, tuliskan pemahaman anda.

**> Query :**
```
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATETIME NOT NULL,
    total DECIMAL(10,2) NOT NULL
);
```

**> Penjelasan :**

Tabel orders berfungsi untuk mencatat pesanan pelanggan, termasuk siapa yang memesan, kapan dipesan, dan berapa total harganya.

----

**> Query :**
```
DELIMITER $$

CREATE PROCEDURE InsertOrders()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 1000000 DO
        INSERT INTO orders (customer_id, order_date, total)
        VALUES (
            FLOOR(1 + (RAND() * 50000)), -- Random customer_id antara 1-50000
            NOW() - INTERVAL FLOOR(RAND() * 365) DAY, -- Random tanggal dalam 1 tahun terakhir
            ROUND(RAND() * 1000, 2) -- Random total transaksi
        );
        SET i = i + 1;
    END WHILE;
END$$

DELIMITER ;
````

**> Hasil :**
![image](https://github.com/user-attachments/assets/0f046523-a057-4524-94ed-19f12be10121)

**> Penjelasan :**
1.	DECLARE i INT DEFAULT 1; → Membuat variabel i yang dimulai dari 1 untuk menghitung jumlah data yang dimasukkan. 
2.	WHILE i <= 1000000 DO → Perulangan (WHILE) akan berjalan sebanyak 1.000.000 kali untuk memasukkan data ke tabel orders. 
3.	INSERT INTO orders (customer_id, order_date, total) → Memasukkan data ke dalam tabel orders dengan nilai acak: 
  •	customer_id: Dipilih secara acak dari 1 hingga 50.000 menggunakan FLOOR(1 + (RAND() * 50000)).
  •	rder_date: Dipilih secara acak dari 365 hari terakhir menggunakan NOW()-INTERVAL FLOOR(RAND() * 365) DAY.
  •	total: Jumlah transaksi dipilih secara acak antara 0 hingga 1000, dengan 2 angka di belakang koma (ROUND(RAND() * 1000, 2)).
4.	SET i = i + 1; → Menambah nilai i setiap kali data baru dimasukkan, hingga mencapai 1.000.000. 
5.	DELIMITER $$ dan DELIMITER ; → Digunakan untuk menandai awal dan akhir dari stored procedure agar MySQL bisa menjalankannya dengan benar.

----

**> Query :**
```
CALL InsertOrders();
```

**> Penjelasan :**

Perintah untuk menjalankan prosedur InsertOrders di database, yang berfungsi untuk memasukkan data pesanan baru.


**> Query :**
```
EXPLAIN SELECT * FROM orders WHERE customer_id = 1001;
```

**> Hasil :**
![image](https://github.com/user-attachments/assets/efb0f149-ff43-4a2e-a74f-2fcc555f1866)

**> Penjelasan :**
  •	Digunakan untuk melihat bagaimana database mengeksekusi query pencarian data dari tabel orders berdasarkan customer_id = 1001.
  •	Jika database menggunakan full table scan (artinya membaca seluruh tabel), mungkin perlu menambahkan index pada customer_id agar query lebih cepat.

----

**> Query :**
```
CREATE INDEX idx_customer_id ON orders(customer_id);
```

**> Hasil :**
![image](https://github.com/user-attachments/assets/b5363e8f-2f41-4618-a02a-9d0ee2e74303)

**> Penjelasan :**

Membuat indeks bernama idx_customer_id pada kolom customer_id di tabel orders.

----

**> Query :**
```
SELECT * FROM orders WHERE customer_id = 1001;
```

**> Penjelasan :**

Perintah ini menampilkan semua informasi pesanan dari tabel "orders" khusus untuk pelanggan dengan ID 1001.

**> Hasil :**
![image](https://github.com/user-attachments/assets/616ec96b-5c91-4a32-a509-6dd30234a133)

----

**> Query :**
```
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

**> Penjelasan :**

Perintah ini memindahkan 100 unit uang dari akun dengan ID 1 ke akun dengan ID 2, dan memastikan bahwa kedua operasi tersebut terjadi sebagai satu transaksi atomik.

**> Hasil :**
![image](https://github.com/user-attachments/assets/fdef658b-09a9-4b79-8a14-60785a8f5b9a)

----

**> Query :**
```
CREATE INDEX idx_customer_id ON orders(customer_id);
```

**> Penjelasan :**

Perintah ini membuat indeks pada kolom customer_id di tabel orders untuk mempercepat pencarian data berdasarkan customer_id.

**> Hasil :**
![image](https://github.com/user-attachments/assets/59e5f5d7-74b5-4aa0-96d1-db05e324a0fe)

----

2.	Pada database minimartket, pada table tr_penjualan_raw, lakukan optimasi dengan menggunakan tehnik diatas. Gunakan index dan query chace.

**> Query 1:**
```
CREATE INDEX idx_tgl_transaksi ON tr_penjualan_raw(tgl_transaksi);
```

**> Hasil 1:**
![image](https://github.com/user-attachments/assets/bc197dac-1f02-4c24-8f7f-b62e9cf05824)

**> Penjelasan :**

Digunakan untuk membuat indeks bernama idx_tgl_transaksi pada kolom tgl_transaksi di tabel tr_penjualan_raw dan membantu mempercepat akses data berdasarkan tanggal transaksi.

----

**> Query 2:**
```
CREATE INDEX idx_kode_item ON tr_penjualan_raw(kode_item);
```

**> Hasil 2:**
![image](https://github.com/user-attachments/assets/40dd9b0a-fb84-4224-8dc6-d27f6a499b06)

**> Penjelasan :**
  •	Digunakan untuk membuat indeks pada kolom kode_item dalam tabel tr_penjualan_raw.

----

**> Query 3:**
```
CREATE INDEX idx_nama_kasir ON tr_penjualan_raw(nama_kasir);
```

**> Hasil 3:**
![image](https://github.com/user-attachments/assets/fcecf1c9-6ba2-474f-969c-95e644a67e36)

**> Penjelasan :**

Indeks ini sangat membantu jika sering ada pencarian berdasarkan nama_kasir. Tapi jika tabel sering diperbarui dan nama_kasir tidak sering digunakan dalam query, indeks ini bisa jadi tidak terlalu diperlukan.

-----

**> Query :**
```
CREATE INDEX idx_kode_cabang ON tr_penjualan_raw(kode_cabang);
```

**> Hasil :**
![image](https://github.com/user-attachments/assets/adc399ea-e76f-440e-94c6-da0890fa949c)

**> Penjelasan :**

CREATE INDEX idx_kode_cabang → Membuat indeks dengan nama idx_kode_cabang.
ON tr_penjualan_raw(kode_cabang) → Indeks diterapkan pada kolom kode_cabang.

------

3.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi. SELECT * FROM tr_penjualan_raw WHERE YEAR(tgl_transaksi) = 2024;

Dikarenakan pada database 2008 bukan 2024

**> Query :**
```
SELECT * FROM tr_penjualan_raw WHERE YEAR(tgl_transaksi) = 2015;
&
SELECT * FROM tr_penjualan_raw
WHERE tgl_transaksi BETWEEN '2015-01-01' AND '2015-12-31';
```

**> Hasil-nya sama :**
![image](https://github.com/user-attachments/assets/6a9cedbb-c51b-4501-9a95-63c108d6a163)

**> Penjelasan :**
  •	YEAR(tgl_transaksi) = 2015
    - Menggunakan fungsi YEAR(), yang dapat menghambat penggunaan indeks karena harus menghitung tahun untuk setiap baris sebelum memfilter data.
    - Bisa lebih lambat jika tabel besar.
  •	BETWEEN '2015-01-01' AND '2015-12-31'
    - Lebih optimal karena langsung membandingkan nilai tanggal tanpa fungsi tambahan.
    - Indeks pada tgl_transaksi dapat digunakan, sehingga eksekusi lebih cepat.

------

4.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi
SELECT * FROM tr_penjualan_raw WHERE kode_item IN ('ITEM1', 'ITEM2', 'ITEM3', ..., 'ITEM500');

**> Query :**
```
SELECT * FROM tr_penjualan_raw 
WHERE kode_item IN (
    'ITM-038', 'ITM-020', 'ITM-017', 'ITM-007', 'ITM-015', 
    'ITM-006', 'ITM-035', 'ITM-034', 'ITM-022', 'ITM-021', 
    'ITM-005', 'ITM-012', 'ITM-009', 'ITM-031'
);
```

**> Hasil :**
![image](https://github.com/user-attachments/assets/d9478e12-7e30-4f0f-883c-e09be5ea6634)

**> Penjelasan :**

Gunakan indeks pada kode_item (CREATE INDEX idx_kode_item ON tr_penjualan_raw(kode_item);) untuk mempercepat eksekusi query ini.

-----

5.	Query berikut apakah sudah optimal?? Jika belum, lakukan optimasi SELECT * FROM tr_penjualan_raw WHERE nama_kasir LIKE '%John%';

**> Query :**
SELECT * FROM tr_penjualan_raw WHERE nama_kasir LIKE '%Jono%';

**> Hasil :**
![image](https://github.com/user-attachments/assets/b8beecba-2c21-4b03-81a7-925924a52cab)

**> Penjelasan :**

Query ini digunakan untuk mencari semua data dalam tabel tr_penjualan_raw di mana nama_kasir mengandung kata "Jono" di mana saja dalam teks.

-----

Karena nama_kasir masih NULL maka saya UPDATE 
**> Query :**
```
UPDATE tr_penjualan_raw SET nama_kasir = 'Joko' WHERE kode_kasir = '039-053';
UPDATE tr_penjualan_raw SET nama_kasir = 'Jono' WHERE kode_kasir = '039-127';
UPDATE tr_penjualan_raw SET nama_kasir = 'Sam' WHERE kode_kasir = '039-156';
UPDATE tr_penjualan_raw SET nama_kasir = 'Sari' WHERE kode_kasir = '039-212';
UPDATE tr_penjualan_raw SET nama_kasir = 'Ciko' WHERE kode_kasir = '039-203';
UPDATE tr_penjualan_raw SET nama_kasir = 'Chika' WHERE kode_kasir = '039-031';
UPDATE tr_penjualan_raw SET nama_kasir = 'Dito' WHERE kode_kasir = '039-147';
UPDATE tr_penjualan_raw SET nama_kasir = 'Abel' WHERE kode_kasir = '039-044';
UPDATE tr_penjualan_raw SET nama_kasir = 'Nana' WHERE kode_kasir = '039-023';
```

**> Penjelasan :**
Perintah-perintah tersebut mengubah nama-nama kasir di tabel tr_penjualan_raw berdasarkan kode kasir mereka.

**> Hasil :**
![image](https://github.com/user-attachments/assets/2770192a-1708-4d5c-ac91-1e2207c3755e)

------

6.	Diberikan query berikut. Pada kolom harga belum ada index. Apakah query berikut sudah optimal?? Jelaskan langkah2 optimasinya. SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CB001';

**> Query :**
```
SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CABANG-039';
````

**> Hasil :**
![image](https://github.com/user-attachments/assets/e04d90b4-89b9-4ac6-8bf3-713b710d7dfa)

**> Penjelasan :**

Query tersebut mencari harga tertinggi (MAX(harga)) dalam tabel tr_penjualan_raw dengan filter hanya untuk data yang memiliki kode_cabang = 'CABANG-039'.

-------

**> Query :**
```
CREATE INDEX idx_harga_cabang ON tr_penjualan_raw(kode_cabang, harga);
```

**> Hasil :**
 ![image](https://github.com/user-attachments/assets/9367bad7-5dca-4e6b-9320-178897644c4a)
 
 ![image](https://github.com/user-attachments/assets/434911fc-96c2-4b09-b0d8-917e78a28958)

**> Penjelasan :**

Query berikut membuat indeks bernama idx_harga_cabang pada tabel tr_penjualan_raw, dengan kolom kode_cabang sebagai kolom utama dan harga sebagai kolom tambahan dalam indeks.

-----

Karena harga masih NULL maka saya UPDATE 

**> Query :**
```
UPDATE tr_penjualan_raw SET harga = 15000 WHERE kode_item = 'ITM-038' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 12000 WHERE kode_item = 'ITM-020' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 10000 WHERE kode_item = 'ITM-017' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 8000  WHERE kode_item = 'ITM-002' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 18000 WHERE kode_item = 'ITM-034' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 22000 WHERE kode_item = 'ITM-023' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 14000 WHERE kode_item = 'ITM-015' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 9000  WHERE kode_item = 'ITM-006' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 25000 WHERE kode_item = 'ITM-035' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 11000 WHERE kode_item = 'ITM-022' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 13000 WHERE kode_item = 'ITM-007' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 17000 WHERE kode_item = 'ITM-009' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 16000 WHERE kode_item = 'ITM-012' AND harga IS NULL;
UPDATE tr_penjualan_raw SET harga = 14000 WHERE kode_item = 'ITM-019' AND harga IS NULL;
```

**> Hasil :**
![image](https://github.com/user-attachments/assets/66645417-2566-4768-8b82-68df62145b30)

![image](https://github.com/user-attachments/assets/49870978-d433-4072-a268-48673696a75b)

**> Penjelasan :**

Perintah-perintah SQL tersebut digunakan untuk memperbarui harga item di tabel tr_penjualan_raw jika harga item tersebut sebelumnya belum diisi (NULL). Berikut ringkasannya:

- Setiap perintah UPDATE menetapkan harga spesifik untuk item dengan kode_item tertentu.
- Perubahan hanya dilakukan pada baris di mana kolom harga bernilai NULL.
- Tujuannya adalah untuk mengisi nilai harga yang hilang atau kosong dalam tabel.
  
Dengan kata lain, skrip ini mengisi harga-harga yang kosong pada tabel penjualan raw, berdasarkan kode item yang terkait.

----

# KESIMPULAN

Optimasi bottleneck dalam MySQL sangat penting untuk meningkatkan performa database. Dokumen ini menjelaskan bahwa bottleneck adalah kondisi di mana kinerja database menurun karena keterbatasan sumber daya atau masalah dalam eksekusi query. Beberapa teknik optimasi yang dibahas meliputi pembuatan indeks pada kolom yang sering digunakan, penggunaan query cache, meningkatkan ukuran buffer pool untuk InnoDB, dan menghindari penggunaan SELECT * dengan hanya mengambil kolom yang diperlukan.

Melalui latihan praktik, dokumen ini menunjukkan bagaimana mengimplementasikan teknik-teknik tersebut pada database minimarket dengan tabel tr_penjualan_raw. Hasil latihan menunjukkan bahwa penggunaan indeks dan query yang dioptimalkan dapat meningkatkan kecepatan akses data secara signifikan. Selain itu, optimasi query dengan menghindari fungsi pada kolom terindeks (seperti YEAR()) dan menggunakan perbandingan langsung juga terbukti lebih efisien.

----

# PEMBAHASAN

**1. Penggunaan Indeks dalam MySQL**

Indeks adalah salah satu komponen paling penting dalam optimasi database. Dokumen ini menjelaskan bahwa indeks membantu mempercepat pencarian data dengan mengurangi jumlah baris yang harus dipindai. Beberapa jenis indeks yang dibahas meliputi:
- Indeks pada kolom WHERE
- Indeks pada kolom JOIN
- Indeks multi-kolom

Praktik pembuatan indeks pada tabel tr_penjualan_raw (idx_tgl_transaksi, idx_kode_item, idx_nama_kasir, idx_kode_cabang) menunjukkan implementasi teknik ini. Dokumen juga menunjukkan cara mengecek keberadaan indeks dengan `SHOW INDEX FROM nama_tabel` dan menggunakan EXPLAIN untuk mengevaluasi efektivitas indeks.

**2. Query Cache dan Buffer Pool**

Dokumen membahas pentingnya query cache untuk menyimpan hasil query yang sering digunakan. Meskipun tidak diimplementasikan dalam latihan, dokumen menjelaskan cara mengaktifkan dan mengecek status query cache. Selain itu, dokumen juga membahas InnoDB Buffer Pool sebagai tempat penyimpanan utama untuk data dan indeks yang sering digunakan, serta cara untuk meningkatkan ukurannya untuk performa yang lebih baik.

**3. Optimasi Query**

Beberapa teknik optimasi query yang dibahas dalam dokumen ini meliputi:
- Menghindari penggunaan SELECT * dan hanya mengambil kolom yang diperlukan
- Menghindari penggunaan fungsi pada kolom terindeks (seperti pada latihan nomor 3)
- Menggunakan perbandingan langsung daripada fungsi (BETWEEN vs YEAR())
- Membuat indeks yang tepat untuk query dengan klausa IN atau LIKE

Contoh optimasi query pada latihan nomor 3 menunjukkan perbedaan antara penggunaan `YEAR(tgl_transaksi) = 2015` dan `tgl_transaksi BETWEEN '2015-01-01' AND '2015-12-31'`. Metode kedua lebih optimal karena dapat memanfaatkan indeks pada kolom tgl_transaksi.

**4. Identifikasi Bottleneck**

Dokumen ini mengidentifikasi beberapa penyebab utama bottleneck dalam MySQL:
- Full table scan karena tidak ada indeks
- Koneksi database yang tidak dikelola dengan baik
- Deadlock karena transaksi yang berjalan bersamaan
- Ukuran InnoDB Buffer Pool yang kecil
- Penggunaan SELECT * yang tidak efisien

Contoh-contoh implementasi pada tabel orders dan tr_penjualan_raw menunjukkan bagaimana bottleneck dapat diidentifikasi dan diatasi dengan teknik optimasi yang tepat.

# REFERENSI

1. Dokumentasi resmi MySQL tentang optimasi: https://dev.mysql.com/doc/refman/8.0/en/optimization.html
2. Buku "High Performance MySQL" oleh Baron Schwartz, Peter Zaitsev, dan Vadim Tkachenko
3. Dokumentasi InnoDB Buffer Pool: https://dev.mysql.com/doc/refman/8.0/en/innodb-buffer-pool.html
4. Panduan optimasi query dari MySQL: https://dev.mysql.com/doc/refman/8.0/en/select-optimization.html
5. Dokumentasi tentang indeks dalam MySQL: https://dev.mysql.com/doc/refman/8.0/en/optimization-indexes.html

