# Cara Install Nextcloud HTTPS Ready Versi Docker by LinuxServer
## Link
```
https://hub.docker.com/r/linuxserver/nextcloud
https://hub.docker.com/r/linuxserver/mariadb/
https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
```

## Code Timezone Indonesia
TZ identifier : Asia/Jakarta
Country Code : ID


## JIKA INGIN UPDATE PULL IMAGE DENGAN VERSI BERTAHAP
missal dari versi 17 ke 18 dulu kemudian ke 19 dan selanjutnya.
karna mempengaruhi struktur data yg sudah ada di docker volume

# Cara Install di docker
## docker compose
buat folder terlebih dahulu di lokasi yang diinginkan. contoh di lokasi `/mnt/storage/nextcloud`.
dan buat folder didalamnya `config`, `data`, `db`
```
/mnt/storage/nextcloud
--- config
--- data
--- db
```

# run docker compose up atau dari stack portainer
# dari docker file
buat file docker-compose.yml. sesuaikan port, dan password
```
version: '3'

services:
  db:
    image: mysql:8.0.39
    container_name: nextcloud-db
    restart: always
    command: --transaction-isolation=READ-COMMITTED --log-bin=binlog --binlog-format=ROW
    volumes:
      - /mnt/storage/nextcloud/db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=InsertPassword
      - MYSQL_PASSWORD=InsertPassword
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
    ports:
      - 33061:3306 
  app:
    image: lscr.io/linuxserver/nextcloud:latest
    container_name: nextcloud-app
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Jakarta
    volumes:
      - /mnt/storage/nextcloud/config:/config
      - /mnt/storage/nextcloud/data:/data
    ports:
      - 443:443
    restart: always

```

# Setting awal nextcloud
1. buka `https://alamat-ip:443` 
2. masukkan username & password yg diinginkan.
3. setting koneksi database.
untuk  database host sesuaikan dengan nama service nextcloud-db sesuai docker compose
```
dbhost => nextcloud-db
```
atau bisa juga konfigurasi setelah tersetup docker
```
masuk ke path volume docker nextloud misalkan `/mnt/storage/nextcloud/config/www/nextcloud/config`
```
```
nano config.php
```
```
sesuaikan host, port dll.
```

# Aktifkan aplikasi yang diperlukan
1. masuk ke menu aplikasi -> Matikan Aplikasi
2. Tekan Tombol Aktifkan pada aplikasi yg diperlukan misalkan :
```
External storage support
Brute-force settings
Auditing/Logging
```

# Add Trusted Domain
## masuk ke terminal bash nextcloud atau bisa juga lgsung masuk ke path lokasi volume di assign contoh `/mnt/storage/nextcloud/config/www/nextcloud/config`
masuk ke bash nextcloud
```
docker exec -it nextcloud-app bash 
```
```
cd /config/www/nextcloud/config
```
```
ls -l
```
```
nano config.php
```
tambahkan domain contoh
```
  'trusted_domains' =>
  array (
    0 => '192.168.88.10',
    1 => 'test.msfebrian.my.id,
  ),
```
```
restart container
```
