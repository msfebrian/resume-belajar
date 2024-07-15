# Install dan Konfigurasi Penyimpanan Nextcloud Snap Ubuntu Server

install package
```
  sudo snap install nextcloud
```

Lihat konfigurasi nextcloud 
```
  sudo snap info nextcloud
```

Lihat list perintah2 nextcloud
```
  cat /snap/nextcloud/current/meta/snap.yaml
```

Mengaktifkan https ssl self-signed
```
  sudo nextcloud.enable-https self-signed
```

Tambahkan Trusted Domain
```
  sudo snap run nextcloud.occ config:system:set trusted_domains 1 --value=nama_domain
  
  Setelah itu restart
     sudo snap restart nextcloud
```

Melihat List Trusted Domain
```
  sudo snap run nextcloud.occ config:system:get trusted_domains 1
  sudo snap run nextcloud.occ config:system:get trusted_domains 0
```

Enable external storage
```
  sudo snap connect nextcloud:removable-media
```

Rubah config.php lokasi penyimpanan data 
(disable dahulu service)
```
   sudo snap stop nextcloud
```
Kemudian rubah lokasi datadirectory
```
  sudo nano /var/snap/nextcloud/current/nextcloud/config/config.php 
  datadirectory = /lokasi_yg_diinginkan
```
aktifkan lagi service
```
   sudo snap enable nexcloud 
```


Mengelola service
```
  sudo snap start nexcloud
  Atau
  sudo snap stop nexcloud
```

Setting firewall
```
  sudo ufw allow 80,443/tcp
```



Contoh setting permission
```
   chown -R www-data:www-data /mnt/disk2
   chmod -R 0750 /mnt/disk2
   Atau 
   chmod -R 0770 /mnt/disk2 
```

# Cara Akses MySQL Nextcloud
## Menggunakan utilitas CLI yang disertakan dari Nextcloud Snap dengan perintah berikut
```
sudo nextcloud.mysql-client -S /tmp/sockets/mysql.sock -u nextcloud -p
```
## Dump dan Restore Database
```
sudo nextcloud.export -b
```
File dump akan disimpan di direktori tertentu.
Kemudian, Anda dapat mengunggah file dump ini ke database uji (misalnya, database MySQL lokal) dan menggunakan alat grafis untuk melihat dan mengelola data.
