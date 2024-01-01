# Cara share dengan samba dan mounting share smb
## Cara buat share folder dengan samba
.......



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
