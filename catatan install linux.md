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
## alokasi partition di ubuntu lts 24
1. pilih partition `vfat` untuk booting misalkan `500mb`
2. partiion lainnya sama seperti versi ubuntu lama contoh.
   ```
   `ext4` di mount ke root `/` dan sesuaikan ukuran yang diinginkan 
   ```
   ```
   jika masih ada sisa ingin dibuat partition terpisah dari os create `ext4` dan `mount` ke lokasi yg diinginkan contoh `/home`
   ```

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
