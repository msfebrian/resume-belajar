# Cara restore ke kernel default
Tahapan kembali ke kernel lama
## lihat daftar kernel terinstall
```
dpkg -l | grep linux-image
```
atau 
```
dpkg -l | grep linux-image | grep -v hwe 
```

## lihat kernel yang digunakan
- lihat kernel terinstall
```
uname -srn
```
utk lebih detil arsitektur
```
uname -srnm
```
- lihat array kernel pada grub.cfg dengan filter berdasarkan versi distro contoh utk LMDE 6. (list versi lengkap bisa di lihat dari `nano /boot/grub/grub.cfg` pada bagian menu entry) 
```
cat /boot/grub/grub.cfg | grep -iE "menuentry 'LMDE 6 Faye, with Linux" | awk '{print i++ " : "$1, $2, $3, $4, $5, $6, $7}'
```
contoh tampilnya setelah filter
```
0 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-25-amd64'
1 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-25-amd64
2 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-23-amd64'
3 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-23-amd64
4 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-12-amd64'
5 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-12-amd64
```

## modify GRUB_DEFAULT=0 dengan memilih kernel yang akan diload
- jika `GRUB_DEFAULT=0` maka hanya kernel yg terakhir saja yg diload
- jika `GRUB_DEFAULT="1>2"` maka hanya kernel array ke 1 sampai ke 2 yang diload
- lihat konfigurasi grub yg sudah di edit 
```
grep GRUB_DEFAULT /etc/default/grub
```

## Regenerate GRUB config file with midified settings
```
sudo update-grub
```

## Reboot and validate booted kernel
```
sudo systemctl reboot
```
