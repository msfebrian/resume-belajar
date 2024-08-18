# Setting Permission Write NTFS Drive Linux Mint
## 1. Unmount Disk
Unmout Disk yang akan dirubah permission nya

## 2. Lihat lokasi disk
```
df -h
```
## 3. Catat atau Copy Path Disk yang ingin di enable write dari `df -h`
misalkan
```
/dev/sda2
```

## 4. Enable Write path disk yang diinginkan misalkan
```
sudo ntfsfix /dev/sda1
```
## 5. Mount kembali disk
```
bisa melalui gui atau terminal
```
contoh mount melalui terminal (sesuaikan path) contoh.
```
mount /dev/sda1 /media/user/Storage
```
atau mount pada lokasi folder yg diinginkan
```
mkdir /mnt/disk2
mount /dev/sda1 /mnt/disk2
```
atau jika tidak bisa 
```
sudo mount -t ntfs-3g /dev/sda2 /media/user/Storage
```

# Jika cara diatas gagal gunakan cara ini
## Install Gnome Disk Utility
```
sudo apt install -y gnome-disk-utility
```

## fix path disk dengan ntfsfix
contoh
```
sudo ntfsfix /dev/sda5
```
## Mount disk ntfs dan coba write
Mount disk ntfs dan coba write jika tidak bisa reset dan install ulang packet ntfg-3g

## Jika gagal write. reset dan install ulang packet ntfg-3g jalankan perintah
```
sudo apt purge ntfsprogs -y
```
```
sudo apt purge ntfs-3g -y
```
```
sudo apt install ntfs-3g -y
```
