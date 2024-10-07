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




