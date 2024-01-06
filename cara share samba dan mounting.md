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

## cara akses difile explorer
```
smb://alamat_IP_server/nama_share
```

## cara buka smb dr terminal
```
smbclient //alamat_IP_server/nama_share -U nama_pengguna
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

# cara automount dari fstab
## buat directory jika blm ada
```
sudo mkdir /mnt/nama_mount_point
```

## edit fstab
```
sudo nano /etc/fstab
```

## tambahkan pada fstab
```
//alamat_IP_server/nama_share /mnt/nama_mount_point cifs credentials=/path/to/credentials_file,uid=nama_pengguna_lokal,gid=nama_grup_lokal 0 0
```

1. alamat_IP_server: Alamat IP server.
2. nama_share: Nama folder yang dibagikan.
3. /mnt/nama_mount_point: Lokasi di mana folder tersebut akan di-mount.
4. credentials=/path/to/credentials_file: Path menuju file yang berisi kredensial pengguna (biasanya berisi username=nama_pengguna\npassword=sandi_pengguna).
5. nama_pengguna_lokal: Nama pengguna lokal di Ubuntu.
6. nama_grup_lokal: Nama grup pengguna lokal di Ubuntu.