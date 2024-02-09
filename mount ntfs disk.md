# Mount NTFS
Disk NTFS dapat dimount di Armbian. Berikut adalah beberapa langkah untuk melakukannya:

## Instal paket ntfs-3g (jika belum terpasang):
Pada Debian/Ubuntu, jalankan perintah berikut:
```
sudo apt install ntfs-3g
```

Pada Fedora/CentOS/AlmaLinux, gunakan perintah ini:
```
sudo dnf install ntfs-3g
```

Pada Arch Linux/Manjaro, gunakan perintah berikut:
```
sudo pacman -S ntfs-3g
```

## Identifikasi path dari partisi NTFS yang ingin Anda mount. 
Anda dapat menggunakan perintah 
```
parted -l 
```
untuk mengetahui pathnya. 

Misalnya, jika partisinya adalah 
```
/dev/sdb1, maka pathnya adalah /mnt/ntfs.
```

Buat direktori mount (misalnya /mnt/ntfs):
```
sudo mkdir -p /mnt/ntfs
```

## Mount partisi NTFS dengan izin baca-tulis:
```
sudo mount -t ntfs-3g /dev/sdb1 /mnt/ntfs
```

## Verifikasi izin mount dengan menjalankan perintah berikut:
```
mount | grep ntfs
```

Jika Anda melihat rw dalam output, itu berarti izin baca-tulis berhasil.

## Opsional: Agar partisi NTFS otomatis ter-mount saat sistem boot, 
tambahkan entri berikut ke file /etc/fstab:
```
/dev/sdb1 /mnt/ntfs ntfs defaults 0 0
```

Simpan dan tutup file, lalu jalankan perintah
```
 mount -a 
```
untuk memasang semua partisi yang dikonfigurasi dalam /etc/fstab.