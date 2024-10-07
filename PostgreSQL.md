# masuk ke client postgresql
## connect dari local dan port default
```
psql --username=nama_user --dbname=postgres --password
```

## connect ke host berbeda dan port non default
```
psql --host=namahost/iphost --port=nomor_port --dbname=postgres --username=nama_user --password 
```

# melihat schema saat ini
```
SELECT current_schema();
```
default schema adalah public

# membuat dan menghapus schema
## membuat schema
```
CREATE SCHEMA nama_schema;
```
## hapus schema
```
CREATE SCHEMA nama_schema;
```

# pindah schema
```
SET search_path TO nama_schema;
```
```
SHOW search_path;
```
lihat apakah sudah pindah schema
```
SELECT current_schema();
```

# contoh select
format perintah
```
select * nama_schema.nama_table
```
jika nama schema tidak dicantumkan maka akan merujuk ke default schema yg sedang di set

## contoh create table schema contoh table products dan id auto increment menggunakan serial
```
CREATE TABLE contoh.products
(
  id serial not null,
  name varchar(100) not null,
  primary key (id) 
);
```

# insert multi row
```
INSERT INTO contoh.products(name)
VALUES('iphone'),
      ('Play Station');
```

# membuat dan menghapus user
## membuat user
```
CREATE ROLE nama_user
```
## menghapus user
```
DROP ROLE nama_user
```


# Resume dari Gemini Ai
Tentu, dengan senang hati saya bantu buatkan resume perintah PostgreSQL yang mudah dipahami beserta contohnya.

**Resume Perintah PostgreSQL**

### Login ke PostgreSQL
* **Perintah:**
  ```bash
  psql -h <hostname> -p <port> -U <username> <database>
  ```
* **Contoh:**
  ```bash
  psql -h localhost -p 5432 -U postgres mydatabase
  ```
  * **Penjelasan:**
    * `-h`: Hostname server database (localhost jika di komputer lokal).
    * `-p`: Port yang digunakan (default 5432).
    * `-U`: Username untuk login.
    * `<database>`: Nama database yang ingin diakses.

### Membuat User
* **Perintah:**
  ```sql
  CREATE USER <username> WITH PASSWORD '<password>';
  ```
* **Contoh:**
  ```sql
  CREATE USER john WITH PASSWORD 'rahasia123';
  ```

### Memberikan Hak Akses User
* **Perintah:**
  ```sql
  GRANT <privileges> ON <object> TO <username>;
  ```
* **Contoh:**
  ```sql
  GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO john;
  ```
  * **Penjelasan:**
    * `ALL PRIVILEGES`: Memberikan semua hak akses.
    * `ON ALL TABLES IN SCHEMA public`: Pada semua tabel di schema public.
    * `TO john`: Kepada user john.

### Mengatur Akses User Berdasarkan Host
* **Perintah:**
  ```sql
  GRANT <privileges> ON <object> TO <username> WITH GRANT OPTION;
  ```
* **Contoh:**
  ```sql
  GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO john WITH GRANT OPTION;
  ```
* **Penjelasan:**
  * `WITH GRANT OPTION`: Memberikan izin kepada user untuk memberikan izin kepada user lain.

### Data Definition Language (DDL)
* **Membuat Database:**
  ```sql
  CREATE DATABASE <database_name>;
  ```
* **Menghapus Database:**
  ```sql
  DROP DATABASE <database_name>;
  ```
* **Membuat Tabel:**
  ```sql
  CREATE TABLE <table_name> (
      <column_name> <data_type>,
      <column_name> <data_type>,
      ...
  );
  ```
* **Menghapus Tabel:**
  ```sql
  DROP TABLE <table_name>;
  ```
* **Mengubah Nama Tabel:**
  ```sql
  ALTER TABLE <old_table_name> RENAME TO <new_table_name>;
  ```
* **Menambah Kolom:**
  ```sql
  ALTER TABLE <table_name> ADD COLUMN <column_name> <data_type>;
  ```
* **Mengubah Tipe Data Kolom:**
  ```sql
  ALTER TABLE <table_name> ALTER COLUMN <column_name> TYPE <new_data_type>;
  ```
* **Menghapus Kolom:**
  ```sql
  ALTER TABLE <table_name> DROP COLUMN <column_name>;
  ```

### Data Manipulation Language (DML)
* **Memasukkan Data:**
  ```sql
  INSERT INTO <table_name> (<column_list>) VALUES (<value_list>);
  ```
* **Mengubah Data:**
  ```sql
  UPDATE <table_name> SET <column1> = <value1>, <column2> = <value2> WHERE <condition>;
  ```
* **Menghapus Data:**
  ```sql
  DELETE FROM <table_name> WHERE <condition>;
  ```

### Data Control Language (DCL)
* **Memberikan Izin:** (Sudah dijelaskan di atas)
* **Mencabut Izin:**
  ```sql
  REVOKE <privileges> ON <object> FROM <username>;
  ```

### Transaction Control Language (TCL)
* **Memulai Transaksi:**
  ```sql
  BEGIN;
  ```
* **Membatalkan Transaksi:**
  ```sql
  ROLLBACK;
  ```
* **Mengkonfirmasi Transaksi:**
  ```sql
  COMMIT;
  ```

**Contoh Lengkap**
```sql
-- Membuat database baru
CREATE DATABASE my_company;

-- Masuk ke database
\c my_company

-- Membuat tabel karyawan
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    salary NUMERIC(10,2)
);

-- Memasukkan data karyawan
INSERT INTO employees (name, salary) VALUES ('John Doe', 5000.00);

-- Memberikan semua hak akses pada tabel employees kepada user john
GRANT ALL PRIVILEGES ON employees TO john;
```

**Catatan:**
* Ini adalah contoh dasar, PostgreSQL memiliki banyak fitur dan perintah lain yang lebih kompleks.
* Selalu periksa dokumentasi resmi PostgreSQL untuk informasi lebih lanjut.

**Referensi:**
* **Dokumentasi Resmi PostgreSQL:** [https://www.postgresql.org/docs/current/index.html](https://www.postgresql.org/docs/current/index.html)

**Tips:**
* **Latihan:** Praktekkan perintah-perintah di atas secara langsung pada database PostgreSQL Anda.
* **Eksperimen:** Cobalah berbagai kombinasi perintah untuk memahami cara kerjanya.
* **Gunakan Alat Bantu:** Manfaatkan alat bantu seperti pgAdmin atau DBeaver untuk mempermudah pengelolaan database.

Semoga resume ini bermanfaat! Jika ada pertanyaan lebih lanjut, jangan ragu untuk bertanya.



