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

## Perintah-perintah docker
running docker desktop kemudian cek via powershell
```
docker info
```

memastikan docker sudah terintah dengan baik
```
docker run hello-world
```

melihat image docker yang sudah di download atau yang tersedia
```
docker image ls
```
atau 
```
docker images
```
melihat image docker yang sedang berjalan
```
docker ps
```
untuk keluar dari terminal docker image tekan ctrl+c


melihat history image yang pernah berjalan
```
docker ps -a
```


untuk lihat list container
```
docker container ls
```

## List Perintah dasar docker
Mendowload image dari repository hub docker
silahkan copy nama image yg akan didownload dari hub docker contoh format
```
   docker pull namaimage
```
menjalankan image docker
nama image sesuaikan dari docker images
```
   docker run namaimage
```
perintah menjalankan lainnya 
contoh untuk mode lepas yg hanya menampilkan id nya saja
```
docker run -d namaimage
```

untuk menjalankan image dengan versi lainnya
```
docker run -d namaimage:versi
```

```
docker run --option
```
untuk menjalankan container image dari list docker images
```
    docker start IdImage
```
Menghentikan image yang sedang berjalan
```   
    docker stop IdImage
```


```
    docker exec -it
    docker logs
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

##  Menyimpan volume docker di partisi D dalam system operasi windows

Untuk menyimpan volume docker di partisi D dalam sistem operasi Windows, Anda perlu mengubah lokasi default dari direktori 
```
    C:\\ProgramData\\Docker
`````` 
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

##  Menyimpan volume docker di partisi lain dalam system operasi linux

Untuk menyimpan volume docker di partisi lain, perlu mengubah lokasi default dari direktori

```
    /var/lib/docker/volumes
```

ke partisi yang Anda inginkan. Ada beberapa cara untuk melakukannya, antara lain:

Menggunakan opsi -g atau --data-root saat menjalankan perintah dockerd. Misalnya, jika Ana ingin menyimpan volume docker di partisi /mnt/data, bisa mengetikkan:

```
    $ dockerd -g /mnt/data
```

Menggunakan file konfigurasi daemon.json yang berada di /etc/docker/. Anda bisa menambahkan baris berikut di file tersebut:
```
    {
        "data-root": "/mnt/data"
    }
```

Menggunakan symbolic link untuk menghubungkan direktori /var/lib/docker/volumes ke partisi yang diinginkan. Misalnya, jika ingin menyimpan volume docker di partisi /mnt/data, bisa mengetikkan:

```
    $ sudo service docker stop
    $ sudo mv /var/lib/docker/volumes /mnt/data
    $ sudo ln -s /mnt/data /var/lib/docker/volumes
    $ sudo service docker start
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
