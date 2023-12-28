# Cara Run Mariadb Docker
```
docker run \
--name=mariadb-lts-jammy \
-e MARIADB_ROOT_PASSWORD=password_yg_diinginkan \
-e LOWER_CASE_TABLE_NAMES=1 \
-p 3306:3306 \
-v mariadb-data:/var/lib/mysql \
-d mariadb:lts-jammy
```

# Cara mematikan casesensitive table name
## Install nano ke container utk edit conf
```
docker exec -it [container name or ID] bash -c 'apt-get -y update && apt -y install nano'
```
jika stuck tengah jalan coba lagi atau hapus perintah update jika tidak bisa

## masuk ke terminal dalam docker container mariadb
```
docker exec -it mariadb-lts-jammy bash
```

## rubah config mariadb untuk lowercase
agar tabel dapat diakses tanpa casesensitive
```
nano /etc/mysql/mariadb.cnf
```
tambah line konfigurasi mariadb.cnf
```
[mariadb]
lower_case_table_names=1
```
matikan dan jalankan ulang container

