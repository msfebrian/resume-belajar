## Cara fix gagal booting ubuntu server

# Langkah 1: Gunakan Mode Recovery pada Grub

1. Restart Server:
Restart server Anda dan tekan tombol Shift atau Esc saat Grub (boot loader) mulai muncul.

2. Pilih Mode Recovery:
Pilih opsi "Recovery Mode" atau "Advanced Options" dari menu Grub. Kemudian, pilih kernel yang ingin Anda jalankan dalam mode recovery.

3. Pilih Opsi Perbaikan:
Di menu selanjutnya, Anda akan memiliki beberapa opsi untuk memperbaiki sistem, seperti 'fsck' untuk memeriksa dan memperbaiki sistem file, atau opsi untuk memperbaiki paket-paket yang rusak.

4. Ikuti Instruksi yang Diberikan:
Ikuti instruksi yang muncul di layar dan jalankan pilihan yang sesuai untuk memperbaiki masalah yang terjadi.

# Langkah 2: Gunakan Live CD/USB
1. Boot dari Live CD/USB:
Gunakan live CD/USB Ubuntu untuk mem-boot sistem Anda.

2. Akses Partisi Sistem:
Buka terminal dan mount partisi root sistem ke direktori yang sesuai. Misalnya, jika partisi root adalah /dev/sdXY, gunakan perintah berikut untuk mount:
```
sudo mount /dev/sdXY /mnt
```

4. Periksa dan Perbaiki Sistem File:
Gunakan perintah fsck untuk memeriksa dan memperbaiki kesalahan pada partisi root. Gantilah /dev/sdXY dengan path partisi root yang sesuai.
```
sudo fsck -f /dev/sdXY
```

5. Cek Konfigurasi Sistem:
Periksa konfigurasi sistem, seperti file /etc/fstab, grub configuration, atau konfigurasi lain yang mungkin mengakibatkan masalah booting.

6. Reinstall Grub:
Buka terminal dan jalankan perintah berikut untuk menginstal ulang Grub:
```
sudo grub-install /dev/sdX
sudo update-grub
```
Pastikan untuk mengganti /dev/sdX dengan disk tempat bootloader Grub berada.


