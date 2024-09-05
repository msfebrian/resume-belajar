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

## Mariadb dan Phpmyadmin
sesuaikan port, volume dll docker compose
```
version: '3'
services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb
    environment:
      - MARIADB_ALLOW_EMPTY_ROOT_PASSWORD=1
    volumes:
      - mariadb-data:/var/lib/mysql
    ports:
      - 3306:3306
    restart: always
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    ports:
      - 8081:80
    environment:
      - PMA_HOST=mariadb
      - PMA_PORT=3306

volumes:
  mariadb-data:
```

contoh jika volume assign di direktory os
```
version: '3.7'

services:
  mariadb:
    image: mariadb:lts-jammy
    container_name: mariadb-lts-jammy
    restart: always
    volumes:
      - /home/docker/mariadb-data:/var/lib/mysql
    ports:
      - 3306:3306
    environment:
      - MARIADB_ALLOW_EMPTY_ROOT_PASSWORD=1
  phpmyadmin:
    image: phpmyadmin
    container_name: phpmyadmin
    restart: always
    ports:
      - 8081:80
    environment:
      - PMA_PORT=3306
      - PMA_HOST=mariadb-lts-jammy
```

cara run dengan metode compose
```
   masuk ke di direktori yg berisi docker-compose.yml
   kemudian run di terminal
      docker compose up
   atau
      docker compose up -d
```

