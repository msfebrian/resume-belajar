# RESUME BELAJAR DOCKER
## Tahapan Install Docker for Windows
Install dulu Windows Subsystem for Linux Jika belum terinstal WSL.
caranya Run As Administrator Command Prompt
kemudian ketik perintal 
```
wsl --install
```

untuk cek wsl via power shell
```
wsl --version
```

untuk cek tipe os apa saja pada wsl
```
wsl --list v
```
setelah terinstall wsl kemudian install Docker Desktop Install, 
kemudian Restart PC.

# Install Docker di Debian
1. Run the following command to uninstall all conflicting packages:
   ```
   for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
   ```
2. Add Docker's official GPG key:
   ```
   sudo apt-get update
   ```

   ```
   sudo apt-get install ca-certificates curl
   ```
   
   ```
   sudo install -m 0755 -d /etc/apt/keyrings
   ```

   ```
   sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
   ```

   ```
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```
4. Add the repository to Apt sources:
   ```
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

   replace $VERSION_CODENAME dengan code release debian misalkan bookworm contoh
   ```
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
   $(. /etc/os-release && echo "bookworm") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. ambil list data repositori
   ```
   sudo apt-get update
   ```
5. Install Latest Version
   ```
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

6. View Version & Test
   ```
   docker --version
   ```
   ```
   sudo docker run hello-world
   ```

5. Konfigurasi Pengguna untuk Akses Docker
   manambahkan pengguna saat ini ke group docker
   ```
   sudo usermod -aG docker $USER
   ```
   ```
   kemudian masukkan password user yang digunakan saat ini
   ```

   menambahkan pengguna lain ke grup docker.
   ```
   sudo usermod -aG docker nama_username
   ```
   

# Install Dokcer di ubuntu
## 1. Hapus Versi Lama (Opsional):
```
   sudo apt remove docker docker-engine docker.io containerd runc
```

## 2. Instal Dependensi yang Diperlukan:
```
   sudo apt update
```
```
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

## 4. Tambahkan Kunci GPG Resmi Docker:
rekomendasi pakai ini.
- Add Docker's official GPG key:
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

atau gunakan ini jika diatas tidak bisa
```
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## 5. Tambahkan Repositori Docker ke APT Sources:
rekomendasi pakai ini
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
## Note
```
If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use UBUNTU_CODENAME instead of VERSION_CODENAME.
```

atau pakai ini jika tidak bisa
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

atau untuk Ubuntu 20.04 (Focal Fossa): sesuaikan dengan versi ubuntu
```
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

## 6. Update Database Paket: (bisa di run saat akhir setelah docker install selesai)
```
   sudo apt update
```

## 7. Instal Docker Engine:
rekomendasi pakai ini
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

atau gunakan ini jika diatas tidak bisa
```
   sudo apt install docker-ce docker-ce-cli containerd.io
```

atau gunakan ini jika diatas tidak bisa
```
sudo apt update | sudo apt-cache policy docker-ce
```
```
sudo apt install docker-ce
```

## 8. Verifikasi Instalasi:
```
   sudo systemctl status docker
```

## 9. Konfigurasi Pengguna untuk Akses Docker (Rekomended):
Untuk dapat menjalankan perintah Docker tanpa perlu menggunakan sudo, tambahkan pengguna ke grup docker.
```
   sudo usermod -aG docker nama_username
```

## 10. Uji Docker:
Untuk menguji apakah Docker terinstal dengan baik, jalankan perintah:
```
   docker --version
   docker run hello-world
```

#  Memindahkan lokasi default penyimpanan volume docker di partisi lain saat run image

Untuk menyimpan volume docker di partisi lain, perlu mengubah lokasi default dari direktori

```
    /var/lib/docker/volumes
```

ke partisi yang Anda inginkan. Ada beberapa cara untuk melakukannya, antara lain:

Menggunakan opsi -g atau --data-root saat menjalankan perintah dockerd. Misalnya, jika Anda ingin menyimpan volume docker di partisi /mnt/data, bisa mengetikkan:

```
    $ dockerd -g /mnt/second-partition
```

##  setting default penyimpanan volume docker di partisi D dalam system operasi windows

Untuk menyimpan volume docker di partisi D dalam sistem operasi Windows, Anda perlu mengubah lokasi default dari direktori 
```
    C:\\ProgramData\\Docker
```
ke partisi yang diinginkan. Ada beberapa cara untuk melakukannya, antara lain:

Menggunakan file konfigurasi daemon.json yang berada di "C:\\ProgramData\\Docker\\config". Anda bisa menambahkan baris berikut di file tersebut:

```
{
    "data-root": "D:\\Docker"
}
```

Menggunakan opsi --data-root saat menjalankan perintah dockerd. Misalnya, jika ingin menyimpan volume docker di partisi D, bisa mengetikkan:

```
    $ dockerd --data-root D:\\Docker
```

Setelah Anda mengubah lokasi default dari volume docker, Anda bisa membuat dan menggunakan volume docker seperti biasa dengan perintah

```
    docker volume create
    dan 
    docker run -v
```

Anda juga bisa melihat informasi lebih detail tentang volume docker dengan perintah

```
    docker volume inspect
```

# Tahapan Pindah Default Lokasi Penyimpanan Docker di linux
1. Stop Service
   ```
   sudo systemctl disable docker
   ```
   
3. Copy Library Docker ke lokasi yang diinginkan
   ```
   sudo rsync -aP /var/lib/docker /mnt/storage/
   ```
   
4. Buat file konfigurasi daemon.json di /etc/docker/. jika sudah ada tambahkan baris berikut di file daemon.json:
```
    {
        "data-root": "/mnt/second-partition/docker"
    }
```

5. restart dan cek service docker
```
   sudo systemctl enable docker
   sudo systemctl start docker
   sudo systemctl status docker
```

6. Uji dan test create volume apakah sudah masuk di lokasi yang diinginkan

## Cara lain menggunakan link
Menggunakan symbolic link untuk menghubungkan direktori /var/lib/docker/volumes ke partisi yang diinginkan. Misalnya, jika ingin menyimpan volume docker di partisi /mnt/data, bisa mengetikkan:

```
   sudo systemctl disable docker
   sudo mv /var/lib/docker/ /mnt/second-partition
```
binding (opsional)
```
   sudo ln -s /mnt/second-partition/docker /var/lib/docker/
```

restart dan cek service docker
```
   sudo systemctl enable docker
   sudo systemctl start docker
   sudo systemctl status docker
```

Setelah mengubah lokasi default dari volume docker, Maka bisa membuat dan menggunakan volume docker seperti biasa dengan perintah 
```
    docker volume create2
    dan 
    docker run -v3
``` 
Kemudian juga bisa melihat informasi lebih detail tentang volume docker dengan perintah docker 
```
    volume inspect
```

# Perintah-perintah docker
running docker desktop kemudian cek via powershell
```
docker info
```

## memastikan docker sudah terinstall dengan baik
```
docker run hello-world
```

## melihat image docker yang sudah di download atau yang tersedia
```
docker image ls
```
atau 
```
docker images
```

## melihat image docker yang sedang berjalan
```
docker ps
```
untuk keluar dari terminal docker image tekan ctrl+c


## melihat history image yang pernah berjalan
```
docker ps -a
```


## lihat list container
```
docker container ls
```

## pull image
Mendowload image dari repository hub docker
silahkan copy nama image yg akan didownload dari hub docker contoh format
```
   docker pull namaimage
```

## menjalankan image docker
nama image sesuaikan dari docker images
```
   docker run namaimage
```
perintah menjalankan lainnya 
contoh untuk mode lepas yg hanya menampilkan id nya saja (tanpa menampilkan log error atau sukses)
```
docker run -d namaimage
```

## untuk menjalankan image dengan versi lainnya
```
docker run -d namaimage:versi
```

```
docker run --option
```

## untuk menjalankan container image dari list docker images
```
    docker start IdImage
```

## Menghentikan image yang sedang berjalan
```   
    docker stop IdImage
```

## mengakses docker container
```
    docker exec -it
    docker logs
```

Masuk ke terminal container misalkan postgresql
```
    docker exec -it nama_container/id bash
    contoh
    docker exec -it postgresql bash
    
    coba login postgresql
    psql -U nama_username --password --db mydb
```


## mematikan dan menyalakan service container
```
   docker stop nama_container
   docker start nama_container
```

## menghapus container. biasanya distop dlu servicenya kemudian hapus
```
   docker rm nama_container_atau_ID_container
```

## Hapus semua container
Menampilkan ID dari semua container (termasuk yang sedang berjalan dan yang tidak).
```
   docker ps -aq
```

Menghapus semua container dengan menggunakan ID yang didapatkan dari docker ps -aq.
```
   docker rm -f $(docker ps -aq)
```

Hapus Container yang Tidak Digunakan
```
   docker container prune -f
```

# docker volume
## buat volume docker
```
docker volume create nama_volume
```
volume akan disimpan di default penyimpanan docker

## menyimpan volume docker pada folder host
pada docker run di bagian volume masukkan path folder di host dan kontainer 
```
-v /lokasi/host/utkSimpanVolume:/lokasi/path/kontainer
```
contoh utk os windows slash path gunakan back slash 
```
-v c:/lokasi/host/utkSimpanVolume:/lokasi/path/kontainer
```


## cara binding port
dalam satu host beberapa container dapat memiliki port yang sama
namun harus di binding port terlebih dahulu
contoh : 
container redis versi lates memiliki port 6379 dengan binding port 6000
container redis versi 5.0 memiliki port 6379 dengan binding port 6001

contoh perintah
```
    docker run -p 6000 -d redis
```
```
    docker run -p 6001 -d redis:5.0
```
kemudian lihat image yg berhasil run
```
    docker ps
```

## Cara lihat ip docker dll
untuk melihat id & nama container yg running
```
docker ps -a
```

melihat informasi container
```
docker inspect id_container
```

## Install package ke dalam container docker
contoh install package nano ke container utk edit conf
```
docker exec -it [container name or ID] bash -c 'apt-get -y update && apt -y install nano'
```
jika stuck tengah jalan coba lagi, atau hapus perintah update jika tidak bisa



# Install Portainer
```
docker pull portainer/portainer-ce:latest
```

## Install Portainer pada volume dokcer dilokasi default docker
buat docker volume untuk penyimpanan portainer
```
docker volume create portainer_data
```
run image portainer
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

## Install Portainer dilokasi yang diinginkan
buat direktori dilokasi yang diinginkan untu menyimpan data portainer. contoh di lokasi `/home/docker`.
```
 mkdir portainer-data
```
run image portainer
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /home/docker/portainer-data:/data portainer/portainer-ce:latest
```

buka portainer via browser
```
https://localhost:9443
```

## Contoh run docker mysql via docker run
Berikut contoh run docker dgn konfigurasi root password = admin portExpose:portDocker 3306:3306 dan create volume my-db
image daemon yang akan dijalankan adalah mysql dengan tag versi 5.7.44
```
docker run \
--name=mysql \
-e MYSQL_ROOT_PASSWORD=admin \
-p 3306:3306 \
-v my-db:/var/lib/mysql \
-d mysql:5.7.44
```

## Contoh Docker Compose
menjalankan maupun langsung pull image jika belum tersdia dapat langsung mengkonfigurasi melalui file docker-compose
```
   buat file docker-compose.yml
```
isi file docker seperti contoh berikut. pada contoh ini akan mempull/men-run adminer (pengganti phpmyadmin)
```
# Use root/example as user/password credentials
version: '3.1'

services:

  db:
    image: mysql:5.7.44
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: admin
    ports:
      - 3306:3306
    volumes:
      - my-db:/var/lib/mysql

  # Aplikasi semacam phpmyadmin jika tidak ingin pakai hapus
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

# Names our volume
volumes:
  my-db:  
```
mariadb & phpmyadmin.
sesuaikan port
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
  hpmyadmin:
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

# Network Docker
## Membuat Network Docker
```
docker network create nama-network
```

## Melihat list Network
```
docker network ls
```

## Mengkoneksikan Service Docker ke Network
format perintah
```
docker network connect nama-network nama-container-yg-ingin-dikoneksikan
```

contoh mengkoneksikan service container mariadb dan nextcloud ke network nextcloud-net
```
docker network connect nextcloud-net mariadb
```
```
docker network connect nextcloud-net nextcloud-app
```

## melihat service container yang terkoneksi di docker network
```
docker network inspect nama-network
```
