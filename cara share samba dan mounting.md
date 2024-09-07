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

atau
```
sudo mount -t cifs //<alamat_IP_server>/<nama_berbagi> /mnt/smb -o username=<nama_pengguna>,password=<kata_sandi>
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

lakukan verifikasi & mount 
```
sudo findmnt --verify
```

```
sudo mount -a
```

## Buat file untuk menyimpan kredensial pengguna (opsional tapi direkomendasikan).
Untuk keamanan, disarankan untuk menyimpan kredensial pengguna dalam file terpisah. Buat file teks yang berisi kredensial pengguna di lokasi yang aman. Misalnya:
```
sudo nano /path/to/credentials_file
```

isi file seperti ini
```
username=nama_pengguna
password=sandi_pengguna
```

atur permission agar hanya dpt diakses oleh root
```
sudo chmod 600 /path/to/credentials_file
```

lakukan verifikasi & mount 
```
sudo findmnt --verify
```

```
sudo mount -a
```

# Contoh Implementasu set SMB untuk dua folder dengan dua user berbeda
contoh share folder yang akan dibuat yaitu folder sigit dan dadan
## Buat Folder
```
sudo mkdir /smb/sigit
sudo mkdir /smb/dadan
```

## Tambahkan konfigurasi di file configurasi smb
untuk default debian dan ubuntu konfigurasi file gunakan smb.conf 
```
sudo nano /etc/samba/smb.conf
```
untuk casa os konfigurasi file gunakan smb.conf 
```
sudo nano /etc/samba/smb.casa.conf
```
Tambahkan konfigurasi berikut di akhir file:
```
[sigit]
path = /smb/sigit
valid users = sigit
read only = no
browseable = yes

[dadan]
path = /smb/dadan
valid users = dadan
read only = no
browseable = yes
```

## Buat user Samba dan set password:
```
sudo adduser sigit
sudo smbpasswd -a sigit
# Masukkan password: sigitAuth

sudo adduser dadan
sudo smbpasswd -a dadan
# Masukkan password: dadanAuth
```

## Restart layanan Samba:
```
sudo systemctl restart smbd
```



