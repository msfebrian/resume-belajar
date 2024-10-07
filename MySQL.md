## Cheat Sheet DDL MySQL

**DDL (Data Definition Language)** digunakan untuk mendefinisikan struktur database, seperti membuat database, tabel, indeks, dan mengubah struktur yang sudah ada.

### Perintah Utama DDL

* **CREATE**
  * **Membuat database:**
    ```sql
    CREATE DATABASE nama_database;
    ```
  * **Membuat tabel:**
    ```sql
    CREATE TABLE nama_tabel (
        kolom1 tipe_data,
        kolom2 tipe_data,
        ...
        PRIMARY KEY (kolom1),
        FOREIGN KEY (kolom2) REFERENCES tabel_lain(kolom_lain)
    );
    ```
  * **Membuat indeks:**
    ```sql
    CREATE INDEX nama_indeks ON nama_tabel (kolom1, kolom2);
    ```
  * **Membuat view:**
    ```sql
    CREATE VIEW nama_view AS
    SELECT kolom1, kolom2 FROM nama_tabel;
    ```

* **ALTER**
  * **Mengubah struktur tabel:**
    * **Menambahkan kolom:**
      ```sql
      ALTER TABLE nama_tabel ADD kolom_baru tipe_data;
      ```
    * **Menghapus kolom:**
      ```sql
      ALTER TABLE nama_tabel DROP COLUMN kolom_yang_dihapus;
      ```
    * **Mengubah tipe data kolom:**
      ```sql
      ALTER TABLE nama_tabel MODIFY kolom tipe_data_baru;
      ```
    * **Mengubah nama kolom:**
      ```sql
      ALTER TABLE nama_tabel CHANGE kolom nama_baru tipe_data;
      ```
  * **Mengubah nama tabel:**
    ```sql
    ALTER TABLE nama_tabel RENAME TO nama_baru;
    ```

* **DROP**
  * **Menghapus database:**
    ```sql
    DROP DATABASE nama_database;
    ```
  * **Menghapus tabel:**
    ```sql
    DROP TABLE nama_tabel;
    ```
  * **Menghapus indeks:**
    ```sql
    DROP INDEX nama_indeks ON nama_tabel;
    ```
  * **Menghapus view:**
    ```sql
    DROP VIEW nama_view;
    ```

### Contoh Penggunaan
```sql
-- Membuat database baru bernama mydatabase
CREATE DATABASE mydatabase;

-- Menggunakan database
USE mydatabase;

-- Membuat tabel pelanggan
CREATE TABLE pelanggan (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nama VARCHAR(100),
    alamat TEXT
);

-- Menambahkan kolom telepon
ALTER TABLE pelanggan ADD COLUMN telepon VARCHAR(20);

-- Menghapus tabel pelanggan
DROP TABLE pelanggan;
```
