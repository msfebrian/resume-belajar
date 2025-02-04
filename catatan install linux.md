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

## Alokasi partition di LMDE6 single boot BIOS Legacy
1. Create root partition dengan ukuran yg diinginkan misalkan 40gb dgn launch GParted format ke ext4 dan mount ke root `/`
2. Create second partition jika msh ada space dan pointing ke lokasi mount yg diinginkan misalkan ke `/home`
3. Pilih lokasi boot media ke `parent disk` jangan ke partition

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
6. `Buat Partisi Boot EFI` (jika menggunakan EFI) format pilih `fat32` ukuran 500mb sudah cukup. `ksh nama tag EFI atau bebas`
7. `Buat Partisi Root Linux` dan pilih format `ext4`
8. `Apply dahulu` pada GParted kemudian `rubah flag mengikuti EFI Windows`. Kemudian Apply
9. pada menu instalasi `pointing` `partisi EFI Linux ke /boot/efi`
10. pointing juga root partition ke `/` dan `format ext4`
11. jika ada partition lain pointing mount ke `/home` atau ke path yg diinginkan `TANPA MEMFORMAT!`
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

# Catatan install debian
## install sudo
bawaan debian tidak ada paket sudo. berikut cara installnya.
1. masuk dengan user root.
```
su -
```
2. Install sudo
```
apt-get install sudo
```
3. Menambahkan Pengguna ke Grup `sudo`
```
sudo usermod -aG sudo nama_user
```
4. Cek Hak Akses sudo
```
sudo whoami
```
5. Melihat list group sudo
melihat list user yg ada digroup sudo
```
   sudo getent group sudo
```
melihat status grup username
```
   sudo groups nama_username
```

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

# Cara Upgrade Linux Debian tanpa install ulang
Cara Upgrade Linux Debian tanpa install ulang misalkan dari versi bullseyes ke bookworm
1. rubah source apt package
rubah semua versi source bullseyes ke bookworm di sources.list
```
nano /etc/apt/sources.list
```
2. Update Source Repository
```
apt update
```
4. Upgrade Package
```
apt upgrade -y
```
5. Upgrade distro
```
apt dist-upgrade
```
6. cek versi distro yang sudah diupgrade
```
lsb_release -a
```
7. reboot
```
sudo reboot now
```

# menghapus paket yang sudah tidak digunakan setelah upgrade
```
sudo apt --purge autoremove -y
```

# cara install flatpak
```
sudo apt install flatpak
```
## install gui di gnome & xfce
```
sudo apt install gnome-software-plugin-flatpak
```

## install gui di kde plasma
```
sudo apt install plasma-discover-backend-flatpak
```

## Add the Flathub repository
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## restart
```
sudo reboot now
```

# Mendisable atau menahan update kernel agar tidak terupdate
Pada Debian 12 (dan distro Debian-based lainnya), Anda dapat menghentikan update kernel menggunakan metode berikut:

1. **Gunakan APT Hold untuk Kernel Package:**

   Debian memiliki perintah untuk "menahan" paket agar tidak diperbarui. Anda bisa menggunakan perintah berikut untuk mencegah kernel dari pembaruan otomatis:

   ```bash
   sudo apt-mark hold linux-image-$(uname -r)
   ```

   Perintah ini akan "memegang" (hold) versi kernel yang saat ini sedang Anda gunakan, sehingga tidak akan diperbarui saat Anda melakukan `apt upgrade`.

   Untuk melepas (unhold) kernel dan mengizinkan update kembali, gunakan:

   ```bash
   sudo apt-mark unhold linux-image-$(uname -r)
   ```

2. **Blokir Kernel Update melalui File Preferences (Optional):**

   Anda juga bisa menggunakan file `preferences` di APT untuk memastikan kernel tidak diupdate. Langkah-langkahnya:

   - Buat atau edit file di `/etc/apt/preferences.d/` (misalnya, `no-kernel-upgrade`):

     ```bash
     sudo nano /etc/apt/preferences.d/no-kernel-upgrade
     ```

   - Tambahkan baris berikut untuk menahan paket kernel:

     ```plaintext
     Package: linux-image*
     Pin: release a=stable
     Pin-Priority: -1
     ```

     Dengan pengaturan di atas, APT tidak akan memperbarui paket kernel (apapun yang diawali dengan `linux-image`). Jika Anda ingin menyesuaikan agar hanya memblokir versi tertentu, Anda dapat mengganti `*` dengan versi kernel yang ingin Anda tahan.

3. **Cek dan Verifikasi:**

   Setelah melakukan perubahan, Anda bisa memverifikasi apakah kernel sudah "ditahan" dari update dengan menjalankan:

   ```bash
   apt-mark showhold
   ```

   Ini akan menunjukkan daftar paket yang ditandai sebagai "hold."

Dengan langkah-langkah di atas, kernel pada Debian Anda tidak akan diperbarui secara otomatis.

# Pilih Kernel Saat Booting (GRUB)
Setelah install kernel baru, jangan langsung hapus kernel lama. Reboot dulu, lalu di menu GRUB, pilih "Advanced options for [distro kamu]" dan pilih kernel yang baru diinstal.

Jika kernel baru berjalan lancar, kamu bisa menghapus kernel lama pakai:
```sh
sudo dnf remove kernel-5.xx.x  # Fedora  
sudo apt remove linux-image-5.xx.x  # Debian  
```
# informasi kernel untuk intel gen 6
Untuk Intel generasi ke-6 (Skylake), kernel yang paling stabil biasanya versi 5.10 hingga 6.1.

- Kernel 5.10 LTS → Stabil dan masih mendapat pembaruan keamanan. Cocok kalau butuh sistem yang solid tanpa gangguan.
- Kernel 5.15 LTS → Lebih baru dengan perbaikan pada driver i915 untuk GPU Intel.
- Kernel 6.1 LTS → Salah satu kernel terbaru dengan dukungan jangka panjang, performa lebih baik, dan lebih optimal untuk Skylake.

## cara install kernel
cari dahulu di repository
```
sudo apt search kernel-lts  
```
install versi kernel lts
```
sudo apt install kernel-lts  
```
contoh install versi kernel yang diinginkan misalkan versi 5
```
sudo apt install linux-image-5.10.0-xx-amd64  
```
Jangan lupa juga install header-nya jika perlu:
```
sudo apt install linux-headers-5.10.0-xx-amd64  
```

# cek kernel yang terpasang
```
uname -r
```

# mengatasi hang
Sepertinya kamu maksud **Intel Core i5-6600** atau **Intel UHD 660**? Kalau kamu pakai prosesor Intel generasi ke-6 (Skylake), memang ada beberapa masalah kompatibilitas dengan kernel baru, terutama terkait driver grafis.  

Kalau setiap update kernel bikin sistem hang atau lag, bisa jadi ada konflik dengan driver **i915** (driver grafis Intel di Linux). Beberapa solusi yang bisa dicoba:  

1. **Tambahkan opsi kernel di GRUB**  
   - Edit GRUB:  
     ```sh
     sudo nano /etc/default/grub
     ```  
   - Tambahkan di `GRUB_CMDLINE_LINUX_DEFAULT`:  
     ```
     i915.enable_psr=0
     ```
   - Simpan, lalu update GRUB:  
     ```sh
     sudo update-grub
     ```  
   - Reboot dan lihat apakah masalahnya berkurang.  

2. **Coba downgrade kernel**  
   Kalau versi kernel terbaru malah bikin masalah, coba pakai kernel versi sebelumnya yang lebih stabil buat Skylake.  

3. **Pastikan paket firmware Intel sudah terinstall**  
   ```sh
   sudo apt install firmware-misc-nonfree
   ```  
   atau di Fedora:  
   ```sh
   sudo dnf install linux-firmware
   ```  

4. **Gunakan Xorg, bukan Wayland**  
   Kadang Wayland kurang stabil di GPU Intel lama. Coba masuk ke Xorg di login screen sebelum masuk desktop.  

Kalau masih sering hang setelah update kernel, bisa coba cek log dengan:  
```sh
journalctl -b -1 -p err
```  
atau  
```sh
dmesg | grep -i i915
```  
Biar tahu apa penyebab spesifiknya. Kamu pakai GPU terpisah atau hanya yang bawaan Intel?

# Cara install vmware tool
Berikut adalah panduan lengkap untuk menginstal VMware Tools (atau open-vm-tools) pada Debian:

---

### **Metode 1: Menggunakan `open-vm-tools` (Rekomendasi)**
`open-vm-tools` adalah paket sumber terbuka yang dikelola komunitas dan lebih mudah diinstal. Cocok untuk Debian versi terbaru.

1. **Perbarui Daftar Paket**
   ```bash
   sudo apt update
   ```

2. **Instal Paket open-vm-tools**
   - Untuk Debian **tanpa antarmuka GUI** (server/headless):
     ```bash
     sudo apt install open-vm-tools
     ```
   - Untuk Debian **dengan GUI** (desktop):
     ```bash
     sudo apt install open-vm-tools open-vm-tools-desktop
     ```

3. **Restart Layanan** (opsional)
   ```bash
   sudo systemctl restart open-vm-tools
   ```

4. **Verifikasi Instalasi**
   ```bash
   sudo systemctl status open-vm-tools
   ```
   - Pastikan layanan aktif (`active (running)`).

---

### **Metode 2: Instal VMware Tools Manual (Lewat ISO)**
Gunakan metode ini jika Anda memerlukan fitur spesifik dari VMware Tools versi resmi.

1. **Persiapkan Dependensi**
   ```bash
   sudo apt update
   sudo apt install build-essential linux-headers-$(uname -r)
   ```

2. **Mount ISO VMware Tools**
   - Di VMware: Klik **VM > Install VMware Tools**.
   - Di terminal Debian, mount ISO ke direktori:
     ```bash
     sudo mount /dev/cdrom /mnt
     ```

3. **Ekstrak dan Jalankan Instalasi**
   ```bash
   # Salin file installer ke direktori sementara
   mkdir -p /tmp/vmware-tools
   cd /tmp/vmware-tools
   tar xzf /mnt/VMwareTools-*.tar.gz

   # Jalankan installer
   cd vmware-tools-distrib/
   sudo ./vmware-install.pl -d
   ```
   - Tekan **Enter** untuk semua prompt jika menggunakan opsi `-d` (default).

4. **Selesai dan Restart**
   ```bash
   sudo umount /mnt  # Unmount ISO
   reboot
   ```

---

### **Verifikasi Fungsi VMware Tools**
- Cek versi:
  ```bash
  vmware-toolbox-cmd -v
  ```
- Fitur seperti *copy-paste*, *drag-drop*, dan resolusi layar otomatis seharusnya bekerja.

### Catatan:
- **Reboot** sistem setelah instalasi untuk memastikan semua modul kernel aktif.
- Untuk Debian versi lama (misal: Debian 9), mungkin perlu menambahkan repositori `contrib` di `/etc/apt/sources.list`.

Jika ada masalah, pastikan ISO VMware Tools terpasang dengan benar atau gunakan `sudo apt reinstall open-vm-tools` untuk memperbaiki instalasi.
