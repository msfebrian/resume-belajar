# Cara share dengan samba dan mounting share smb
# Cara buat share folder dengan samba

## Instalasi samba
```
sudo apt update
sudo apt install samba
```

## backup default config samba

```
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

## edit konfigurasi samba
```
sudo nano /etc/samba/smb.conf
```

## tambahkan di bagian akhir file smb.conf:
```
[nama_share]
   path = /lokasi/folder/anda
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no
   create mask = 0777
   directory mask = 0777
```
1. Ganti [nama_share] dengan nama yang ingin Anda gunakan untuk share folder.
2. path adalah lokasi dari folder yang ingin Anda bagikan.
3. Sesuaikan opsi lain sesuai dengan kebutuhan akses yang Anda inginkan.

## buat user utk smb

```
sudo smbpasswd -a nama_pengguna
```

## restart samba
```
sudo systemctl restart smbd
```

## ijinkan firewall 
```
sudo ufw allow samba
```


## Cara Mounting Share smb
Buat folder utk lokasi mount contoh
```
 mkdir /home/samba
```
install aplikasi cifs-utils dengan perintah:
```
sudo apt-get install cifs-utils
```
contoh cara mounting smb
```
sudo mount -t cifs -o user=nama_user //192.168.2.1/shared /home/samba/
```
lihat apakah sdh ada share smb yg sudah dimount 
```
df -h
```
untuk unmounting caranya
```
sudo umount /home/samba
```
