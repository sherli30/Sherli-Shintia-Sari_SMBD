# MANAJEMEN USER, ROLE, DAN PRIVILLEGE

# LATAR BELAKANG PEMBAHASAN 
Dalam sistem basis data, manajemen user, role, dan privilege memiliki peran yang sangat penting dalam menjaga keamanan, integritas, dan keteraturan akses terhadap data. Setiap pengguna yang terhubung dengan basis data harus diatur hak aksesnya agar hanya bisa melakukan operasi sesuai kebutuhan dan tanggung jawabnya. Oleh karena itu, dibutuhkan mekanisme yang baik dalam membuat, mengelola, serta mengawasi pengguna, role, dan privilege.

Seiring perkembangan, MySQL kini telah mendukung konsep role yang memudahkan administrator dalam mengelola hak akses secara lebih efisien. Selain itu, monitoring aktivitas pengguna juga menjadi bagian penting dalam proses pengelolaan sistem, untuk mendeteksi potensi aktivitas mencurigakan dan memastikan bahwa seluruh operasi pada database berjalan sesuai kebijakan.

Praktikum ini bertujuan untuk memberikan pemahaman mendalam tentang cara melakukan manajemen akun pengguna, pengaturan privilege, penerapan role, serta monitoring aktivitas pada sistem basis data MySQL. Melalui kegiatan ini, diharapkan peserta mampu mengelola keamanan dan akses pengguna dengan efektif.

--- 

# PROBLEM YANG DIANGKAT
- a. Bagaimana cara membuat, menghapus, dan mengelola akun pengguna dalam MySQL, termasuk penerapan otentikasi?
- b. Bagaimana cara memberikan hak akses atau privilege kepada user secara langsung maupun melalui role?
- c. Bagaimana cara membuat dan mengelola role di MySQL untuk memudahkan manajemen privilege?
- d. Bagaimana cara melakukan pengujian untuk memastikan role dan privilege yang diberikan berfungsi sesuai kebutuhan?
- e. Bagaimana cara mencabut role atau privilege dari user yang tidak lagi memerlukan akses tertentu?
- f. Bagaimana cara melakukan monitoring aktivitas pengguna, mendeteksi query yang dijalankan, perubahan data, dan audit keamanan menggunakan general log?

---

# SOLUSI/SKENARIO AKTIVITAS

# Tugas praktikum
1.	Lakukan proses pembuatan username sebanyak jumlah kelompok anda. Tuliskan script dan tampilkan hasilnya.
2.	Lakukan penghapusan username terhadap user yang sudah dibuat. Tuliskan script dan tampilkan hasilnya
3.	 ![image](https://github.com/user-attachments/assets/40426a06-4856-4753-9b48-c126c23a9b73)

4.	Berikan privilege select, insert kedalam role diatas
5.	 ![image](https://github.com/user-attachments/assets/c96bc426-247b-4c47-b97c-7415f7a7b958)

6.	Berikan privilege create, drop kedalam role diatas
7.	Berikan 2 user kedalam masing-masing role diatas.
8.	Lakukan pengujian sebelum dan sesudah user diberikan role.
9.	Lepas role dari user diatas. Sehingga user menjadi tidak memiliki role.
10.	Lakukan konfigurasi untuk proses monitoring proses seperti contoh diatas, dan lakukan beberapa kali proses query. Kemudian lihat di log nya dan tampilkan hasilnya.
11.	Tulis kesimpulan dari kegiatan praktek kelompok anda.

---

# Jawaban
1.	Proses Pembuatan username ke dalam SQL yog sebanyak 3 kali denagn menggunakan scripst sebagai berikut:

-	Membuat tiga user baru dalam database MySQL dengan nama chalimatus, safira, dan sherli, masing-masing hanya dapat mengakses dari localhost.
**> Query :**
```
CREATE USER 'chalimatus'@'localhost' IDENTIFIED BY 'chalimatus';
```

**> Penjelasan :**
-	Output run yaitu 1 querie, 1 succes tanpa adanya error.

**> Hasil:**
![image](https://github.com/user-attachments/assets/d181fc39-d87e-452e-be60-4bc100a2f90a)

----

**> Query :**
```
CREATE USER 'safira'@'localhost' IDENTIFIED BY 'safira';
```

**> Penjelasan :**

Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/96b1547d-e1d0-4bfe-a108-f6678bcfdf43)

----

**> Query :**
```
CREATE USER 'sherli'@'localhost' IDENTIFIED BY 'sherli';
```

**> Penjelasan :**

Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/e244cf99-3b26-470d-ae28-0ec8d35ad1df)

----

#	Menampilkan daftar user beserta host yang diizinkan untuk mengakses database.

**> Query :**
```
SELECT USER, HOST FROM mysql.user WHERE USER IN ('chalimatus', 'safira', 'sherli'); 
```

**> Penjelasan :**

Output menghasilkan 3 users dengan host yaitu localhost

**> Hasil :**
![image](https://github.com/user-attachments/assets/4b8c5595-c543-40c8-9d66-610250f6f8f3)

----

2.	Proses penghapusan username terhadap user yang sudah dibuat., dengan menggunakan script sebagai berikut:
# Menghapus user sherli dari MySQL.

**> Query :**
```
DROP USER 'sherli'@'localhost'; 
```

**> Penjelasan :**

-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/53ddd1f3-0fa7-4747-81a6-c8b16eb5c22c)

----

# Mengecek kembali daftar user yang masih ada setelah penghapusan user sherli.

**> Query :**
```
SELECT USER, HOST FROM mysql.user WHERE USER IN ('chalimatus', 'safira', 'sherli');
```

**> Penjelasan :**
-	Output menghasilkan 3 users dengan host yaitu localhost

**> Hasil :**
![image](https://github.com/user-attachments/assets/16a90a05-a682-4930-b97d-efa913b7763f)

----

3.	Role dengan menggunakan “role_nama_anda_insert_se  role_andi_select_insert kemudian menghasilkan output:

# Membuat role bernama role_chalimatus_insert_select.

**> Query :**
```
CREATE ROLE 'role_chalimatus_insert_select';
```

**> Penjelasan :**
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/0a1afa24-62d5-4fdc-b8ac-327c5d116047)

----

4.	Berikan privilege select, insert kedalam role diatas

# Memberikan izin SELECT dan INSERT pada seluruh tabel di database_example untuk role role_chalimatus_insert_select.

**> Query :**
```
GRANT SELECT, INSERT ON database_example.* TO 'role_chalimatus_insert_select';
```

**> Penjelasan :**
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/e995a9e0-8c66-4160-a6bf-41a78c1fa657)

----

5. 
![image](https://github.com/user-attachments/assets/ee8b0aed-977a-4e01-83f4-1e3cb67137ef)

# Membuat role bernama role_chalimatus_create_drop.

**> Query :**
```
CREATE ROLE 'role_chalimatus_create_drop'; dengan output sebagai berikut:
```

**> Penjelasan :**
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/6fc2e121-a620-41c1-b420-9a77ab25fc1b)

----

6.	privilege create, drop kedalam role diatas
   
# Memberikan izin CREATE dan DROP pada seluruh tabel di database_example untuk role role_chalimatus_create_drop.

**> Query :**
```
GRANT CREATE, DROP ON database_example.* TO 'role_chalimatus_create_drop';
```

**> Penjelasan :**
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

**> Hasil :**
![image](https://github.com/user-attachments/assets/1687e662-02fa-4b28-bff7-5c132bc7d5a5)

----

7.	Berikan 2 user kedalam masing-masing role diatas
#	Memberikan role role_chalimatus_insert_select kepada chalimatus dan safira.
 

-	Code : GRANT 'role_chalimatus_insert_select' TO 'chalimatus'@'localhost', 'safira'@'localhost'; ; dengan output sebagai berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error

![image](https://github.com/user-attachments/assets/944e0893-3e6e-41bb-b3d0-2b68837bca1a)

----

-	Mengecek kembali daftar user yang memiliki hak akses tertentu.
-	Code : SELECT USER, HOST FROM mysql.user WHERE USER IN ('chalimatus', 'safira', 'sherli');
-	Output menghasilkan 3 users dengan host yaitu localhost
 ![image](https://github.com/user-attachments/assets/0c979500-43b3-447f-8260-9f783a129b0f)

----

8.	Proses pengujian sebelum dan sesudah user diberikan role dengan menggunakan
-	Menampilkan daftar hak akses (privilege) yang dimiliki oleh user chalimatus dan safira.
-	Code : SHOW GRANTS FOR 'chalimatus'@'localhost'; dengan outptu sebagai berikut:
-	Output dari rants for chalimatus@localhost yaitu grant yang di select ke tujuan user dari localhost
![image](https://github.com/user-attachments/assets/65fd2c34-153b-4706-bbd8-328f113cd955)

----

-	SHOW GRANTS FOR 'safira'@'localhost'; dengan output sebagai berikut:
-	Menampilkan hak akses user setellah role di terapkan, dengan code: SHOW GRANTS FOR 'safira'@'localhkst;
![image](https://github.com/user-attachments/assets/4fe11b77-6ed8-4d87-a30d-10280d98dead)

----
-	Mengaktifkan peran yang telah diberikan untuk sesi pengguna saat ini.
-	Code : SET ROLE 'role_chalimatus_insert_select'; sdengan output sebagai berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/b8fe6235-e59e-4dfd-a88d-541481ca28db)

----

-	SET ROLE 'role_chalimatus_create_drop'; dengan output sebaga berikut:
![image](https://github.com/user-attachments/assets/c86cc427-a578-449f-87f5-fff5747913ce)

----

-	Menampilkan kembali hak akses user setelah role diterapkan.
-	Code : SHOW GRANTS FOR 'chalimatus'@'localhost'; dengan output sebaga berikut:
-	Output dari rants for chalimatus@localhost yaitu grant yang di select ke tujuan user dari localhost
![image](https://github.com/user-attachments/assets/e3affb98-5413-4d15-a0fe-f7ee85e56d4f)

----
-	SHOW GRANTS FOR 'safira'@'localhost'; dengan output sebaga berikut:
-	Output dari rants for safira@localhost yaitu grant yang di select ke tujuan user dari localhost
![image](https://github.com/user-attachments/assets/483491c8-70c2-47e0-95d1-b50e74373f3d)

----

9.	Lepas role dari user diatas. Sehingga user menjadi tidak memiliki role.
 
-	Menghapus role role_chalimatus_insert_select dari chalimatus dan safira.
-	Menghapus role role_chalimatus_create_drop dari safira dan sherli.
-	Code : REVOKE 'role_chalimatus_create_drop' FROM 'safira'@'localhost', 'sherli'@'localhost';
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/e776a6e1-c19d-45f5-88a3-b7c92117274a)

----

-	SHOW GRANTS FOR 'chalimatus'@'localhost'; dengan output sebaga berikut:
-	menampilkan hak akses user setellah role di terapkan, dengan code: SHOW GRANTS FOR chalima@'localhost;
![image](https://github.com/user-attachments/assets/640351fe-3139-412d-8add-a5865aa7484e)

----

-	Mengecek kembali daftar hak akses user setelah role dicabut.
-	Code : SHOW GRANTS FOR 'safira'@'localhost'; dengan output sebaga berikut:
-	menampilkan hak akses user setellah role di terapkan, dengan code: SHOW GRANTS FOR 'safira'@'localhost
![image](https://github.com/user-attachments/assets/3627ec7e-0bc7-44b2-8527-70551bd9cc95)

----

10. konfigurasi untuk proses monitoring proses seperti contoh diatas, dan lakukan beberapa kali proses query. Kemudian lihat di log nya dan tampilkan hasilnya.
-	Mengaktifkan general log MySQL dan mengatur output log ke dalam tabel database MySQL.
-	Code : SET GLOBAL general_log = 1; dengan output sebaga berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/0aa0b6e7-d2fd-4dbb-97cd-c6e16c263a79)

----

-	SET GLOBAL log_output = 'TABLE'; dengan output sebaga berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/115415aa-94bc-40c5-b96f-0486288903c0)

----

-	Membuat tabel baru bernama TABLE_NAME jika belum ada sebelumnya.
-	Code :
CREATE TABLE IF NOT EXISTS TABLE_NAME ( id INT PRIMARY KEY AUTO_INCREMENT,
column1 VARCHAR(50), column2 VARCHAR(50)
);
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/8b0714e5-8cc4-4f61-b7a1-5e84a080d9a2)

----

-	Membuat tabel test_table dengan kolom id sebagai primary key dan NAME sebagai kolom teks.
-	Code : CREATE TABLE database_example.test_table (id INT PRIMARY KEY, NAME VARCHAR(50)); dengan output sebaga berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/991b6cdc-1cf4-4526-9a70-d8c92285c7a4)

----

-	Menghapus tabel test_table dari database.
-	DROP TABLE database_example.test_table; dengan output sebaga berikut:
-	Hasil run yaitu 1 querie, 1 succes tanpa adanya error
![image](https://github.com/user-attachments/assets/7ee47ce7-461b-4db9-ac79-f74796ba5f72)

----

-	Menampilkan 10 log aktivitas terakhir yang terekam dalam mysql.general_log.
-	Code : SELECT * FROM mysql.general_log ORDER BY event_time
DESC LIMIT 10; dengan output sebaga berikut. Output ada beberapa kolom yaitu event_time : menampilan tanggal dan jam; user_host nya yaitu root, localhost serta kode; thread_id yaitu 8; server_id yaitu 1; command_type yaitu query; dan argument yaitu menyesuaikan yang kita insertkan beserta kode.
![image](https://github.com/user-attachments/assets/fc542a2e-83d4-4ba3-8ba2-0ed45a53dca5)

----

Tulis kesimpulan dari kegiatan praktek kelompok anda. KESIMPULAN
1.	Manajemen Akun Pengguna
-	Berhasil membuat, melihat, dan menghapus user dalam MySQL.
2.	Manajemen Hak Akses dan Role
-	Berhasil membuat role dan memberikan hak akses (privilege) seperti SELECT, INSERT, CREATE, dan DROP.
-	Role berhasil diberikan ke user dan diuji sebelum serta sesudah pemberian role.
 
-	Role juga berhasil dicabut dari user untuk memastikan mereka kembali tanpa hak akses yang diberikan.

3.	Monitoring Akses dan Aktivitas Pengguna
-	Berhasil mengaktifkan general log untuk mencatat aktivitas database.
-	Dapat melihat log dari query yang dijalankan seperti SELECT, INSERT, CREATE, dan DROP.

Kesimpulan Keseluruhan:
Menunjukkan bagaimana mengelola pengguna, role, dan hak akses dalam MySQL secara efektif, serta bagaimana melakukan monitoring aktivitas database untuk keamanan dan audit.

----
# PEMBAHASAN

Praktikum manajemen user, role, dan privilege di MySQL yang telah dilakukan menunjukkan beberapa aspek penting dalam pengelolaan keamanan database:

## 1. Manajemen User

Pembuatan dan pengelolaan user merupakan langkah dasar dalam keamanan database. Dalam praktikum, tiga user (`chalimatus`, `safira`, dan `sherli`) berhasil dibuat dengan authentication yang sederhana menggunakan password yang sama dengan username. Dari segi keamanan produksi, praktik ini tidak dianjurkan karena rentan terhadap upaya brute force, namun cukup untuk tujuan pembelajaran.

Sintaks yang digunakan seperti `CREATE USER 'username'@'host' IDENTIFIED BY 'password'` menunjukkan bahwa MySQL membatasi akses tidak hanya berdasarkan kredensial, tetapi juga berdasarkan host yang diperbolehkan untuk mengakses. Dalam praktikum ini, seluruh user dibatasi hanya untuk akses dari `localhost`, yang merupakan praktik keamanan yang baik untuk membatasi eksposur database.

Penghapusan user dengan sintaks `DROP USER 'username'@'host'` juga berhasil dilakukan sebagai bagian dari siklus hidup manajemen user. Ini merupakan langkah penting ketika seorang user tidak lagi memerlukan akses ke database.

## 2. Implementasi Role-Based Access Control (RBAC)

Praktikum mendemonstrasikan konsep RBAC (Role-Based Access Control) yang merupakan pendekatan modern dalam manajemen hak akses. Alih-alih memberikan privilege secara langsung ke masing-masing user, privilege diberikan ke role yang kemudian diberikan ke user. Pendekatan ini memiliki beberapa keuntungan:

1. **Efisiensi Administrasi**: Perubahan pada satu role akan berlaku untuk semua user yang memiliki role tersebut.
2. **Keamanan Terstruktur**: Memudahkan implementasi principle of least privilege.
3. **Skalabilitas**: Memudahkan manajemen hak akses saat jumlah user bertambah.

Dalam praktikum, dua role berhasil dibuat:
- `role_chalimatus_insert_select`: Diberikan privilege SELECT dan INSERT
- `role_chalimatus_create_drop`: Diberikan privilege CREATE dan DROP

Role-role ini kemudian diberikan ke user yang berbeda untuk menunjukkan fleksibilitas RBAC. Satu user (`safira`) bahkan diberikan dua role berbeda, menunjukkan bahwa user dapat memiliki kombinasi privilege dari beberapa role.

## 3. Granularity Privilege

MySQL menyediakan granularity (tingkat kedetailan) yang tinggi dalam pemberian privilege. Dalam praktikum, privilege diberikan pada level database dengan sintaks `ON database_example.*`, yang berarti privilege berlaku untuk semua tabel dalam database tersebut. 

Dalam implementasi yang lebih ketat, privilege dapat diberikan dengan tingkat granularity yang lebih spesifik:
- Level server (global privileges)
- Level database
- Level tabel
- Level kolom
- Level routine (stored procedure/function)

Praktikum ini fokus pada level database, namun pemahaman tentang berbagai level granularity privilege sangat penting untuk implementasi keamanan database yang komprehensif.

## 4. Aktivasi dan Pencabutan Role

Praktikum juga menunjukkan bahwa pemberian role tidak secara otomatis mengaktifkan privilege tersebut. User perlu mengaktifkan role dengan perintah `SET ROLE 'role_name'`. Ini merupakan fitur keamanan tambahan yang memungkinkan user untuk bekerja dengan privilege minimal kecuali secara eksplisit mengaktifkan role yang memberikan privilege lebih tinggi.

Pencabutan role dengan perintah `REVOKE 'role_name' FROM 'username'@'host'` juga berhasil dilakukan, menunjukkan fleksibilitas dalam manajemen privilege yang dinamis.

## 5. Monitoring dan Auditing

Praktikum terakhir menunjukkan pentingnya monitoring aktivitas database melalui general log. Dengan mengaktifkan general log dan mengarahkan output ke table (`log_output = 'TABLE'`), semua aktivitas database termasuk query yang dijalankan dapat dipantau.

Hal ini sangat berguna untuk:
1. **Audit Keamanan**: Mendeteksi aktivitas mencurigakan.
2. **Troubleshooting**: Mengidentifikasi masalah performa atau error.
3. **Compliance**: Memenuhi persyaratan regulasi yang mengharuskan audit trail.

Query seperti `SELECT * FROM mysql.general_log ORDER BY event_time DESC LIMIT 10` memungkinkan administrator untuk melihat aktivitas terbaru yang dilakukan pada database, termasuk informasi tentang user yang melakukan operasi, waktu, jenis perintah, dan query yang dijalankan.

## 6. Best Practices dan Rekomendasi

Berdasarkan praktikum yang dilakukan, beberapa best practices dalam manajemen user, role, dan privilege yang dapat diimplementasikan adalah:

1. **Prinsip Privilege Minimal (Least Privilege)**: Berikan user hanya privilege yang benar-benar dibutuhkan untuk melakukan tugasnya.
2. **Gunakan Role untuk Pengelompokkan Privilege**: Memanfaatkan role untuk meminimalkan kompleksitas manajemen privilege.
3. **Batasi Akses Berdasarkan Host**: Tentukan dengan tepat dari mana user diizinkan mengakses database.
4. **Password Policy yang Kuat**: Implementasikan kebijakan password yang kompleks (tidak diimplementasikan dalam praktikum ini).
5. **Rotasi Kredensial Secara Berkala**: Ganti password dan audit role secara berkala.
6. **Logging dan Monitoring**: Aktifkan logging dan pantau aktivitas database secara teratur.
7. **Segmentasi Akses**: Pisahkan akses antara development, testing, dan production environment.

## 7. Keterbatasan dan Tantangan

Meskipun praktikum ini berhasil menunjukkan konsep dasar manajemen user, role, dan privilege, terdapat beberapa keterbatasan:

1. **Keamanan Password**: Praktikum menggunakan password sederhana yang sama dengan username.
2. **Granularity Terbatas**: Praktikum hanya menunjukkan privilege pada level database, bukan pada level yang lebih spesifik.
3. **Tidak Ada Implementasi SSL/TLS**: Koneksi database tidak dienkripsi.
4. **Tidak Ada Plugin Autentikasi Lanjutan**: Tidak menggunakan metode autentikasi yang lebih aman seperti autentikasi dua faktor.

----

# KESIMPULAN SECARA KESELURUHAN

Praktikum manajemen user, role, dan privilege di MySQL telah berhasil mendemonstrasikan aspek-aspek penting dalam keamanan dan pengelolaan akses database:

1. **Efektivitas RBAC**: Role-Based Access Control terbukti efektif dalam menyederhanakan manajemen privilege untuk multiple user dengan kebutuhan akses yang sama.

2. **Fleksibilitas Privilege**: MySQL menyediakan fleksibilitas dalam pemberian dan pencabutan privilege, baik secara langsung maupun melalui role.

3. **Pentingnya Monitoring**: Aktivasi general log memungkinkan pemantauan aktivitas database yang penting untuk keamanan dan troubleshooting.

4. **Skalabilitas Manajemen Akses**: Penggunaan role memudahkan pengelolaan akses yang dapat diskalakan seiring pertumbuhan jumlah user.

5. **Implementasi Principle of Least Privilege**: Praktikum menunjukkan bagaimana memberikan user hanya privilege yang benar-benar dibutuhkan untuk tugasnya.

6. **Dinamika Privilege**: Privilege dapat diubah secara dinamis sesuai kebutuhan melalui GRANT dan REVOKE.

7. **Transparansi Aktivitas**: Penggunaan log memungkinkan transparansi aktivitas database yang penting untuk audit dan kepatuhan regulasi.

Dalam implementasi nyata, manajemen user, role, dan privilege merupakan komponen kritis dari strategi keamanan database yang komprehensif. Pemahaman mendalam tentang konsep-konsep ini memungkinkan administrator database untuk menyeimbangkan kebutuhan akses dengan keamanan data.

----

# REFERENSI

1. MySQL 8.0 Reference Manual. (2023). "Access Control and Account Management." [https://dev.mysql.com/doc/refman/8.0/en/access-control.html](https://dev.mysql.com/doc/refman/8.0/en/access-control.html)

2. MySQL 8.0 Reference Manual. (2023). "The MySQL Access Privilege System." [https://dev.mysql.com/doc/refman/8.0/en/privilege-system.html](https://dev.mysql.com/doc/refman/8.0/en/privilege-system.html)

3. MySQL 8.0 Reference Manual. (2023). "Role-Based Privilege Management." [https://dev.mysql.com/doc/refman/8.0/en/roles.html](https://dev.mysql.com/doc/refman/8.0/en/roles.html)

4. MySQL 8.0 Reference Manual. (2023). "The General Query Log." [https://dev.mysql.com/doc/refman/8.0/en/query-log.html](https://dev.mysql.com/doc/refman/8.0/en/query-log.html)

5. Oracle. (2022). "MySQL Security Best Practices." [https://www.oracle.com/mysql/technologies/security/](https://www.oracle.com/mysql/technologies/security/)

6. OWASP. (2023). "Database Security Cheat Sheet." [https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html)

7. Schwartz, B., Zaitsev, P., & Tkachenko, V. (2012). "High Performance MySQL: Optimization, Backups, and Replication." O'Reilly Media.

8. Kuhn, D. R., Coyne, E. J., & Weil, T. R. (2010). "Adding Attributes to Role-Based Access Control." IEEE Computer, 43(6), 79-81.

9. Casbin. (2023). "Role-Based Access Control With Pattern Matching." [https://casbin.org/docs/rbac-with-pattern](https://casbin.org/docs/rbac-with-pattern)

10. National Institute of Standards and Technology. (2020). "Guide to Enterprise Password Management." NIST Special Publication 800-118.
