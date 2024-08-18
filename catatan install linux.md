# Setting Penyimpanan Instalasi Ubuntu Server
## standar alokasi disk
ukuran swap disesuaikan dengan ram (tidak menggunakan jg tidak apa2) penggunaan swap terlalu banyak membuat system jd lemot. 
jika RAM besar lebih baik tidak digunakan akan performa cepat dan mengurangi TBW HDD/SSD
```
  contoh swap 4G
```
contoh ukuran boot disk
```
  boot disk 500mb
  lokasi mount /boot
```

root disk ubuntu server
```
  root disk 20G
  lokasi mount /
```
sisanya external storage
```
  external storage jangan dlu dimount
  pilih leave unmount
```
## alokasi partition di ubuntu lts 24.04
1. pilih partition `vfat` untuk booting misalkan `500mb`
2. partition lainnya sama seperti versi ubuntu lama contoh.
   ```
   create `ext4` lalu pilih mount ke root `/` dan sesuaikan ukuran yang diinginkan 
   ```
   ```
   jika masih ada sisa ingin dibuat partition terpisah dari os, create `ext4` dan `mount` ke lokasi yg diinginkan contoh `/home`
   ```

## Install Dual Boot Ubuntu LTS 24.04 dengan Windows 11
1. pastikan partition disk sudah tersedia.
2. cek tipe disk `MBR` atau `GPT`
3. Jika MBR boot pakai `legacy` jika `GPT` boot dengan EFI. bisa menggunakan `Ventoy Multi Bootable`
4. saat pembuatan partition jangan pilih along side tapi pilih `CUSTOM`
5. Buat partition root dengan ukuran yg dinginkan misalkan 50GB dan mount ke root `/` dengan file system `EXT4`
6. Jika ingin membuat partition SWAP pilih ukuran yg dinginkan. (Jika RAM cukup besar disarankan tidak menggunakan swap.
7. Jika masih ada sisa partition atau disk lain yg `ingin diformat dan otomatis termount` pilih disk dan `mount pointnya` 

## Install Dual Boot linux Mint Debian (LMDE6) dengan Windows 11
1. pastikan partition disk sudah tersedia.
2. cek tipe disk `MBR` atau `GPT`
3. Jika MBR boot pakai `legacy` jika `GPT` boot dengan EFI. bisa menggunakan `Ventoy Multi Bootable`
4. saat pembuatan partition jangan pilih otomatis tapi pilih `CUSTOM`
5. Buka GParted
6. Buat Partisi Boot EFI (jika menggunakan EFI) format pilih fat32 ukuran 500mb sudah cukup. ksh nama tag EFI atau bebas
7. Buat Partisi Root Linux dan pilih format ext4
8. Apply dahulu pada GParted kemudian rubah flag mengikuti EFI Windows. Kemudian Apply
9. pada menu instalasi pointing partisi EFI Linux ke /boot/efi
10. pointing juga root partition
11. jika ada partition lain pointing mount ke /home atau ke path yg diinginkan TANPA MEMFORMAT!
12. ingat path partition efi dan kemudian pilih boot manager ke partition efi linux.

## penanganan error saat pembuatan partisi disk
hapus semua partisi dlu menggunakan acronis partition atau software lainnya.

# Konfigurasi Swapp
Swappiness pada sistem Ubuntu mengontrol seberapa sering file swap digunakan. Nilai swappiness mengatur kecenderungan kernel untuk memindahkan proses dari memori fisik ke disk swap.
## 1. Swappiness pada sistem Ubuntu mengontrol seberapa sering file swap digunakan. Nilai swappiness mengatur kecenderungan kernel untuk memindahkan proses dari memori fisik ke disk swap. 
```
nano /etc/sysctl.conf
```
Tambahkan atau ubah baris berikut:
```
vm.swappiness = 10
```
- Simpan perubahan dan tutup berkas.
- Nilai ini menunjukkan bahwa swap hanya akan digunakan ketika penggunaan RAM mencapai sekitar 80 hingga 90 persen.
- Terapkan perubahan swapp
```
sudo sysctl -p
```

## 2. Mengubah Nilai Swappiness Secara Langsung (terkadang cara ini suka ke reset)
Jika Anda ingin mengubah nilai swappiness tanpa harus me-reboot sistem, jalankan perintah berikut:
```
sysctl vm.swappiness=10
```

## 3. Verifikasi Nilai Swappiness
Anda dapat memeriksa nilai swappiness saat ini dengan menjalankan:
```
cat /proc/sys/vm/swappiness
```
Nilai default di Ubuntu adalah 60, tetapi menguranginya menjadi 10 akan meningkatkan performa secara keseluruhan untuk instalasi desktop Ubuntu yang umum.

## 4. Sangat dianjurkan untuk reboot utk alokasi ulang RAM

# penggunaan volume grup (vg) & lvm (list volume manager) .....

# install snap sdh di test di armbian
```
sudo apt update
sudo apt install
sudo reboot now
sudo snap install core
```

# Setting Allow Locked Remote Desktop di Ubuntu Gnome Desktop
1. Install Goole Chrome
2. Install Extensi Gnome Shell Integration
3. Install chrome-gnome-shell
   ```
   sudo apt install chrome-gnome-shell
   ```
5. Buka Extensi Gnome Shell Instegration di Google Chrome
6. Cari Extensi `Allow Locked Remote Desktop` dan aktifkan nyalakan ke `on` 

# install font Microsoft windows
```
sudo apt update
sudo apt install ttf-mscorefonts-installer
sudo fc-cache -f -v
```

# Konfigurasi Keyboard agar dapat menekan key yg sama secara berulang dengan cepat
Masalah tidak dapat menekan tombol keyboard yang sama berulang dengan cepat, bisa disebabkan oleh beberapa hal. Salah satu penyebab umum adalah fitur Accessibility yang disebut Bounce Keys. Fitur ini dirancang untuk membantu pengguna yang secara tidak sengaja menekan tombol berulang kali. Jika Bounce Keys diaktifkan, sistem akan mengabaikan input berulang dari tombol yang sama dalam waktu singkat.

Untuk memeriksa dan menyesuaikan pengaturan ini, Anda dapat mengikuti langkah-langkah berikut:

Buka Settings (Pengaturan) di Ubuntu.
Pilih Universal Access (Akses Universal) atau Accessibility (Aksesibilitas), tergantung versi Ubuntu Anda.
Cari opsi Bounce Keys atau Keyboard (Tombol Keyboard) dan pastikan fitur tersebut tidak diaktifkan.
Jika masalah tidak terkait dengan Bounce Keys, mungkin ada pengaturan lain yang mempengaruhi responsivitas keyboard. Anda juga bisa mencoba mengecek pengaturan Repeat Keys (Ulangi Tombol) di bagian Keyboard dari Settings. Pastikan pengaturan untuk kecepatan ulang dan penundaan ulang sesuai dengan preferensi Anda.

# CARA AUTO MOUNT DISK SAAT BOOT DI LINUX UBUNTU SERVER

masuk terlebih dahulu ke super admin
```
sudo su
```

## Copy UUID Disk
Baca UUID Disk dengan perintah 
``` 
sudo lsblk -f
atau
sudo blkid
```
kemudian Copy UUID untuk dimasukkan ke fstab

pastikan folder tujuan mount sudah ada

misalkan disk akan di mount di /mnt/disk2

## Edit fstab
buka fstab dengan nano
```
sudo nano /etc/fstab
```
tambahkan baris di akhir seperti berikut
contoh 

```
# Auto Mount Disk2 ke /mnt/disk2
UUID=41267f53-8a46-4e67-971b-c02d55538ecb /mnt/disk2 ext4 defaults 0 2

```

contoh lainnya
```
  # Automount partisi 2 to /mnt/second-partition
  /dev/disk/by-uuid/570e6d84-9164-40f8-9d0f-dec46e7f7254 /mnt/second-partition ext4 defaults 0 2
```

jika error jangan gunakan/hapus default 0 2.


## Execute mount dan cek hasil konfigurasi mount sebelum reboot

lakukan verifikasi dan pastikan tidak error sebelum reboot. jika error dan di reboot akan menyebabkan ke gagalan booting os
```
sudo findmnt --verify
```

execute mount fstab
```
sudo mount -a
```

pastikan disk2 sudah termount
```
sudo ls -l /mnt/disk
```

jika tidak ada error maka rebbot os
```
sudo reboot now
```


## catatan dari open ai

Pada baris /etc/fstab, terdapat enam field yang mendefinisikan opsi dan parameter untuk mount point. Dua dari field tersebut adalah pass dan dump. Dalam konteks yang Anda tanyakan, yang dimaksud adalah:

1. pass (field ke-5): Angka ini menunjukkan apakah sistem akan melakukan pemeriksaan terhadap filesystem ketika booting. Jika nilainya 0, sistem tidak akan melakukan pemeriksaan. Jika nilainya 1 atau lebih, sistem akan memeriksa filesystem secara berurutan sesuai dengan angka tersebut. Misalnya, jika ada lebih dari satu filesystem yang diberi nilai 1, sistem akan memeriksa satu per satu dengan urutan angka.

2. dump (field ke-6): Field ini menunjukkan apakah sistem akan mencadangkan (dump) filesystem tersebut. Jika nilainya 0, filesystem tidak akan dicadangkan saat menjalankan perintah dump. Jika nilainya 1, sistem akan mencadangkan filesystem tersebut saat menjalankan perintah dump.

Dalam contoh yang Anda berikan, 0 dan 2 adalah nilai untuk field pass dan dump berturut-turut. Dalam kebanyakan kasus, nilai 0 pada field pass menunjukkan bahwa sistem tidak akan memeriksa filesystem tersebut saat boot, dan nilai 2 pada field dump menunjukkan bahwa filesystem akan dicadangkan jika diperintahkan untuk mencadangkan filesystem.

Field pass dan dump mungkin memiliki nilai yang berbeda-beda tergantung pada konfigurasi spesifik dari filesystem tersebut dan kebutuhan sistem Anda. Biasanya, untuk filesystem root (biasanya ditunjukkan dengan / dalam /etc/fstab), pass diberi nilai 1 agar sistem memeriksa secara urut saat booting. Sedangkan untuk filesystem lainnya, nilai 0 atau 2 adalah nilai yang umum diberikan.

Pastikan untuk merujuk pada dokumentasi sistem file dan kebutuhan spesifik dari sistem Anda untuk menentukan nilai yang tepat untuk field pass dan dump dalam konfigurasi /etc/fstab.


