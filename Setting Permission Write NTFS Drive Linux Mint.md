# Setting Permission Write NTFS Drive Linux Mint
## Buka Aplikasi Disk
Buka Aplikasi Disk bisa cari dari start menu

## Unmount Disk
Unmout Disk yang akan dirubah permission nya

## Catat atau Copy Path Disknya

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
