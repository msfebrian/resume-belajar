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

## install nano ke container utk edit conf
```
docker exec -it [container name or ID] bash -c 'apt-get -y update && apt -y install nano'
```
jika stuck tengah jalan coba lagi hapus perintah update

## contoh running dalam docker
```
docker exec -it mariadb-lts-jammy bash
```

```
docker exec -it [container name or ID] nano /etc/host.conf
```
