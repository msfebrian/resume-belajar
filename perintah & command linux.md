# Perintah-perintah / Command yang umum digunakan di linux
## shutdown dan reboot

Shutdown langsung
```
  sudo shutdown now
```
Shutdown jeda 10 detik
```
  sudo shutdown -h 10
```

Restart OS
```
  sudo reboot now
```
```
Atau
  sudo shutdown -r now
Timing 10 detik
  sudo shutdown -r 10
```

## Keluar dari proses diterminal
```
ctrl + c
```

## Melihat panduan perintah linux
Contoh Panduan detail perintah copy
```
man cp
```

Contoh Panduan ringkas perintah copy
```
cp --help
```

# User Management
## Melihat user saat ini yang digunakan
```
whoami
```

## Masuk ke dalam user di terminal
mengaktifkan atau membuat user root
```
sudo passwd root
```
kemudian masukkan password yg diinginkan 2x utk verifikasi

## masuk ke user dgn grup root
```
sudo su
```

## masuk ke user tertentu
```
sudo su - nama_user
```

## Menambahkan user dan merubah password
Merubah password
```
  sudo passwd nama_user
```
Tekan enter masukkan password lama kemudian password baru 2 kali

## Menambah user
```
sudo adduser nama_user
```   

Menambahkan user ke grup admin
```
   sudo usermod -aG sudo nama_user
```
Keterangan :
```
    -a : tambahkan ke grup
    -G : grup yg dituju (misalkan grup sudo)
```

## Merubah nama user
format perintah
```
sudo usermod -l nama_baru nama_lama
```
contoh
```
sudo usermod -l febrian admin
```

## lihat daftar user dan status groupnya.

### Lihat list user & foldernya
```
   cat /etc/paswd atau getent passwd
```

### Lihat hanya nama pengguna (cut dan awk)
```
   cut -d: -f1 /etc/passwd
```

### melihat semua list group
```
cat /etc/group
```
atau tampil output group lebih bersih
```
getent group
```

### melihat list user yg ada digroup sudo
```
   sudo getent group sudo
```

### Menggunakan Perintah `cut` atau `awk`: Untuk hanya menampilkan nama grup:
```
cut -d: -f1 /etc/group
```
```
getent group | awk -F: '{print $1}'
```

### melihat status grup username
```
   sudo groups nama_username
```

## Menghapus Group
```
sudo groupdel nama_grup
```

### Jika grup terkait dengan file atau direktori, gunakan perintah find untuk mencari semua file yang dimiliki oleh grup tersebut:
```
find / -group nama_grup
```


## Lihat direktori pengguna
```
   ls /home
```

# melihat source repository package
```
nano /etc/apt/sources.list
```

# Update dan upgrade package
## Update
```
sudo apt update
```

## Lihat berapa banyak paket yang dapat diupgrade
```
apt list --upgradable
```

## Upgrade
```
sudo apt upgrade
atau 
sudo apt upgrade -y
```

bisa juga digabung
```
sudo apt update && upgrade -y
```

## mengupgrade semua paket yang tersedia,
```
sudo apt full-upgrade
```

## remove package yg sudah tidak digunakan setelah update / upgrade
```
sudo apt auto remove
```

# Perintah update distro dan kernel baru
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
5. upgrade ke versi distro yang lebih baru atau menginstall kernel baru
```
sudo apt dist-upgrade
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

## Install paket update-manager-core (kernel)
```
sudo apt install update-manager-core
```

## mendapatkan versi terbaru yang belum resmi di rilis
```
sudo do-release-upgrade -d
```

## Cara Melihat Versi Linux
```
sudo lsb_release -a
```
atau
```
cat /etc/os-release
```

## Melihat versi linux dan nama host
```
hostnamectl
```

# Merubah Nama Hostname
1. Cek Hostname Saat Ini :
   ```
   hostnamectl
   ```
2. Ubah Hostname: Gunakan perintah `hostnamectl` untuk mengubah hostname. Misalnya, untuk mengubah hostname menjadi `new-hostname`:
   ```
   sudo hostnamectl set-hostname new-hostname
   ```
3. Edit File `/etc/hosts`: Buka file `/etc/hosts` dan ganti nama `hostname` lama dengan yang baru:
   ```
   sudo nano /etc/hosts
   ```
   Ganti semua entri yang mengandung hostname lama dengan yang baru.
4. Verifikasi Perubahan: Jalankan kembali perintah `hostnamectl` untuk memastikan perubahan telah diterapkan:
   ```
   hostnamectl
   ```
5. reboot
   ```
   sudo reboot
   ```

# Konfigurasi Date Timezone
## Set Default timezone cara mudah (rekomeded)
```
   sudo dpkg-reconfigure tzdata
      pilih asia
          Pilih jakarta
             tampil target timezone
               ..... timezone 'Asia/Jakarta'
```

## Cara lainnya :

untuk lihat list timezone (cara aga pusing bnyak)
```
    sudo timedatectl list-timezones
```

Jika kode area sudah diketahui timezone bisa langsung
```
    sudo timedatectl set-timezone Asia/Jakarta
```

Rubah config timesync
```
   sudo nano /etc/systemd/timesyncd.conf
      Hapus # di belakang NTP & Fallback
      NTP=ur.ntp.srv ( diisi alamat server )
```

Install NTP
```
   sudo apt install ntp
      Cek ntpq -p
```

Cek status time 
```
   sudo timedatectl status
```

Jika ntp & syncron blm aktiv
```
   sudo apt install chrony
   sudo nano /etc/chrony/chrony.conf
      Cek alamat server ntp sdh ada/blm
   sudo /etc/init.d/chrony restart
```
```
    sudo timedatectl set-ntp on atau true

    sudo reboot now
```
    
Kemudian cek status ntp & sync time 
```
    sudo timedatectl status
```

Contoh tampil status timedatectl status
```
root@ubuntu-server:/home/administrator# timedatectl status
               Local time: Fri 2023-12-08 09:58:00 WIB
           Universal time: Fri 2023-12-08 02:58:00 UTC
                 RTC time: Fri 2023-12-08 02:58:00
                Time zone: Asia/Jakarta (WIB, +0700)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no

```

Pastikan tgl & jam sdh sesuai cek date komputer
```
    sudo date
```
```
contoh tampil 
    Fri Dec  8 10:00:20 AM WIB 2023
```

## Proses perintah secara interval dengan watch
contoh menampilkan waktu secara interval 1 detik
```
watch -n 1 date
```
menampilkan proses
```
watch -n 1 ps
```

## update dan upgrade repositories & os
perintah satu-satu
```
  sudo update
  sudo upgrade
```

perintah sekaligus
```
  sudo update && sudo upgrade -y
```

# Mengelola File & Folder

Melihat dan mengelola file  dengan Midnight Commander (mc)
```
    sudo apt install mc
    sudo mc
```

menampilkan semua file atau direktori yang tersembunyi
```
ls -a 
```

menampilkan semua file atau direktori tanpa proses shorting
```
ls -f 
```

melihat semua file lengkap & sorting dari A-Z
```
ls –l 
```

melihat semua file lengkap & sorting dari Z-A
```
ls –lr 
```

melihat semua file lengkap & sorting berdasarkan waktu
```
ls –lrt 
```

melihat semua file dengan informasi ukuran simple dengan menambahkan "h" dibelakang
```
ls -lh
```

## buat folder
```
    mkdir nama-folder
    
    bisa juga tambahkan path
    mkdir /path_directorinya/nama_foldernya
```

## Hapus folder
```
    rmdir nama_folder
    
    bisa juga tambahkan path
    rmdir /path_directorinya/nama_foldernya
```

## Hapus file
```
  rm nama_file
```

## menghapus semua file nya saja, tanpa direktori dan subdirektorinya
```
    rm –f
    
    bisa juga tambahkan path
    rm -f /path_directorinya/..
```

## menghapus file, direktori dan subdirektorinya (recursive remove)
```
    rm –r  
    bisa juga tambahkan path
    rm -r /path_directorinya/..
```

## Memindahkan file, bisa juga untuk merubah nama sebuah file
```
pindah folder
    mv /lokasi_folder /path_tujuan_atau_nama_folder_yg _diinginkan

ganti nama file
    mv /lokasi_folder/nama_file /path_tujuan/nama_file_yg_diinginkan
    
    contoh
    mv /home/data.txt /mnt/disk2 
    mv /home/datapribadi /mnt/disk2
```

## Copy file
```
    cp /path/sumber /path/tujuan 
```

## Mencopy dengn rsync (ada fitur sinkronisasi antara folder sumber dengan tujuan)
## rsync hanya mencopy file2 yg belum ada ditujuan
perintah standar
```
rsync /path/sumber /path/tujuan
```
perintah dengan fungsi tambahan copy hiden dan compres during transfer (informasi lainnya lihat rsync --help)
```
rsync -avzh /path/sumber /path/tujuan
```

## Copy antar server dengan scp
```
scp [sumber_file] [namauser@alamat_server]:~
```
contoh
```
scp file-saya.txt admin@192.168.2.2:~
```

contoh copy folder `/mnt/docker` ke `/mnt/home` pada remote server (fungsi `-r` digunakan untuk mengkopi secara rekursif, sehingga seluruh isi folder akan disalin)
```
scp -r /mnt/docker username@alamat_remote_server:/mnt/home/docker
```


## text editor (notepad)
```
    sudo nano 
```

## Buat, Buka dan edit file dengan nano
```
  sudo nano nama_file

    untuk simpan ctr + o kemudian masukkan nama file dan enter
    untuk keluar ctr + x
```

## Membuat file
```
touch /home/nama_file.txt
```

atau
```
    cat > nama_file
```

Membuat file bernama “bin” dengan isi file “Biodata”
```
Contoh : 
    cat > bin lalu “enter” Biodata
    
    ctrl+c (utk mengakhiri dan simpan)
```

## melihat isi file perhalaman
```
more /etc/log_sys.txt
```

## memfilter dgn fungsi grep
contoh memfilter kata error dan menjumlah kata error pada file dgn word count (wc)
```
sudo cat /var/log/syslog | grep error | wc -l
```

# melihat file secara realtime
```
tail -f filelog.txt
```

# mencari file atau folder
contoh mencari file dgn nama cloud-init.log
```
find /var/log/ | grep cloud-init.log
```

Untuk mencari file di Command Line Interface (CLI) Linux, ada beberapa perintah yang dapat digunakan, tergantung pada kebutuhan spesifik. Berikut adalah perintah-perintah yang sering digunakan:

---

### 1. **Perintah `find`**
Perintah ini sangat fleksibel untuk mencari file berdasarkan nama, tipe, ukuran, atau parameter lainnya. Contoh:

#### a. Mencari file berdasarkan nama:
```bash
find /path/ke/direktori -name "nama_file"
```
**Contoh:**
```bash
find /home/user -name "dokumen.txt"
```

#### b. Mencari file tanpa memperhatikan huruf besar/kecil:
```bash
find /path/ke/direktori -iname "nama_file"
```

#### c. Mencari file berdasarkan tipe:
- Hanya file biasa:
  ```bash
  find /path/ke/direktori -type f
  ```
- Hanya direktori:
  ```bash
  find /path/ke/direktori -type d
  ```

#### d. Mencari file berdasarkan ukuran:
- File lebih besar dari 10MB:
  ```bash
  find /path/ke/direktori -size +10M
  ```
- File lebih kecil dari 1KB:
  ```bash
  find /path/ke/direktori -size -1k
  ```

#### e. Mencari file berdasarkan tanggal modifikasi:
- File yang diubah dalam 7 hari terakhir:
  ```bash
  find /path/ke/direktori -mtime -7
  ```
- File yang diubah lebih dari 30 hari lalu:
  ```bash
  find /path/ke/direktori -mtime +30
  ```

---

### 2. **Perintah `locate`**
Perintah ini lebih cepat dibandingkan `find`, tetapi membutuhkan database yang diperbarui secara berkala menggunakan `updatedb`.

#### a. Mencari file:
```bash
locate nama_file
```
**Contoh:**
```bash
locate dokumen.txt
```

#### b. Perbarui database `locate` (opsional jika hasil pencarian tidak sesuai):
```bash
sudo updatedb
```

---

### 3. **Perintah `grep`**
Jika Anda ingin mencari file berdasarkan kontennya, gunakan `grep`.

#### a. Mencari teks dalam file:
```bash
grep "teks_yang_dicari" /path/ke/direktori/*
```
**Contoh:**
```bash
grep "password" /home/user/*
```

#### b. Rekursif mencari di semua subdirektori:
```bash
grep -r "teks_yang_dicari" /path/ke/direktori
```

---

### 4. **Perintah `find` dengan `grep`**
Untuk mencari file berdasarkan nama dan memeriksa kontennya, kombinasikan `find` dengan `grep`.

#### Contoh:
Mencari file bernama `*.txt` yang mengandung kata "laporan":
```bash
find /path/ke/direktori -name "*.txt" | xargs grep "laporan"
```

---

### 5. **Perintah `ls` dengan `grep`**
Jika direktori tidak terlalu besar, gunakan:
```bash
ls -R /path/ke/direktori | grep "nama_file"
```

---

### Tips Tambahan
- Gunakan wildcard (`*` atau `?`) untuk pencarian lebih fleksibel.
  - `*.txt` untuk semua file dengan ekstensi `.txt`.
  - `file?` untuk file dengan nama lima karakter, di mana karakter terakhir bervariasi.
- Tambahkan opsi `-exec` pada `find` untuk menjalankan perintah pada file yang ditemukan, misalnya:
  ```bash
  find /path/ke/direktori -name "*.log" -exec rm {} \;
  ```

## Membuka file dan edit dengan vim 
```
   contoh configurasi batasan file nextcloud
   sudo vim ~/.nextcloud/proxy/conf.d/custom.conf
   
   client_max_body_sze 500M 
   Save dgn :wq dan sudo reboot 
```
```
    :q berfungsi untuk keluar dari editor tanpa menyimpan file
    :w berfungsi untuk menyimpan file (save)
    :wq berfungsi untuk keluar dari editor sekaligus menyimpan file
```

# Permission Folder & File dengan CHMOD
```
- owner	Pengguna yang membuat dan memiliki file / direktori.
- group	Semua pengguna yang merupakan anggota dalam grup yang sama.
- others	Semua pengguna lainnya dalam sistem yang bukan owner atau member dari sebuah grup..
```
struktur permission
```
r (read) – 4
w (write) – 2
x (execute) – 1
```
contoh format
```
  rwxrwxrwx pemilik:group folder/file
  owner group other
  rwx   rwx   rwx    pemilik:group folder/file
```
contoh perintah
```
chmod 746 file1.txt
atau
chmod 746 namafolder
```
penjelasan
```
permission untuk owner
7 = rwx (dari kode perhitungan 4+2+1 = 7)

permission untuk group
4 = r-- (dari kode permission read adalah 4)

permission untuk other
6 = rw- (dari perhitungan 4+2 = 6)
```
## Config kepemilikan / Permission folder atau file dengan CHOWN
format
```
chown [owner/group owner] [nama file]
```

contoh owner bambang
```
chown bambang demo.txt
```

contoh group client
```
chown :clients demo.txt
```

contoh owner bambang group client
```
chown bambang:clients demo.txt
```

tambahkan opsi "chown -R" utk menerapkan ke dalam folder dan semua file yang ada pada folder
```
chown -R nama_user:nama_group nama_folder
```

# Mengelola dan Memformat Disk 
## Melihat list disk & mounting point
```
  lsblk
```

## tampil kan jg UUID
```
  lsblk -f
``` 

## Tampil disk
```
    fdisk -l | grep /dev/sd
    Atau
    fdisk /dev/sd
```

## Melihat list disk & partisi
```
  sudo parted -l
```

## Cara mount
```
mount sumber_yang_akan_dimount lokasi_tujuan_mount 
```
contoh
```
    mkdir /mnt/disk2
    mount /dev/sdb1 /mnt/disk2
```

## Cara membuat link
Tentu, dengan senang hati saya akan jelaskan fungsi `ln` di Linux beserta contohnya:

**Fungsi `ln` di Linux**

Perintah `ln` di Linux digunakan untuk **membuat link (tautan)** antara dua file atau direktori. Link ini berfungsi sebagai "jalan pintas" menuju file atau direktori asli. Ada dua jenis link yang bisa dibuat dengan `ln`:

1. **Hard Link:**
   * Merupakan pointer langsung ke inode (data struktur yang menyimpan metadata file) dari sebuah file.
   * Jika ada hard link ke sebuah file, menghapus file tersebut tidak akan menghapus file aslinya selama masih ada hard link lain yang menunjuk ke file tersebut.
   * Hard link hanya bisa dibuat untuk file yang berada di partisi yang sama.
   * **Contoh:** `ln /home/user/dokumen.txt /home/user/Desktop/dokumen.txt`

2. **Symbolic Link (Symlink):**
   * Adalah file khusus yang berisi path ke file atau direktori lain.
   * Symlink bisa menunjuk ke file atau direktori di partisi yang berbeda.
   * Ketika Anda mengakses symlink, sistem operasi akan mengikuti path yang tersimpan di dalamnya untuk mengakses file atau direktori tujuan.
   * **Contoh:** `ln -s /home/user/gambar /var/www/html/images`

**Opsi `-s`**

Opsi `-s` digunakan untuk membuat symbolic link. Tanpa opsi ini, perintah `ln` akan membuat hard link.

**Contoh Penggunaan**

* **Membuat hard link:**
   ```bash
   ln /home/user/musik.mp3 /mnt/usb/musik.mp3
   ```
   Perintah di atas akan membuat hard link dari file `musik.mp3` di direktori `home/user` ke direktori `mnt/usb`. Kedua file ini sebenarnya adalah file yang sama, hanya memiliki nama yang berbeda.

* **Membuat symbolic link:**
   ```
   ln -s sumber_path lokasi_tujuan
   ```
   contoh link mount dari /mnt/storage ke path /DATA
   ```
   ln -s /mnt/usb_storage /DATA
   ```
   
   contoh lainnya
   ```bash
   ln -s /usr/local/bin/python3 /usr/bin/python3
   ```
   Perintah di atas akan membuat symbolic link dari `python3` yang ada di `usr/local/bin` ke `usr/bin`. Ketika Anda mengetik `python3` di terminal, sistem akan mencari eksekusi di `usr/bin` dan secara otomatis akan diarahkan ke `usr/local/bin/python3`.

**Ringkasan**

* **Hard link:** Pointer langsung ke inode, hanya bisa di partisi yang sama.
* **Symbolic link:** File khusus berisi path, bisa menunjuk ke partisi berbeda.
* **Opsi -s:** Untuk membuat symbolic link.

## menghapus link
cara menghapus link dari perintah ln -s yang dibuat dari perintah ini
```
ln -s /mnt/usb_storage /DATA
```
menghasilkan folder usb_storage pada PATH data. maka cara menghapusnya sebagai berikut
```
rm /DATA/usb_storage
```

## Tampil penggunaan disk & file system list
Perintah df di Linux digunakan untuk melihat penggunaan ruang disk.
```
  df -h 
```
menampilkan tipe file system
```
  df -Th
```
keterangan
```
df: Perintah ini menampilkan informasi tentang ruang disk yang tersedia dan digunakan di perangkat Anda.
-T: Opsi ini menampilkan jenis sistem berkas (file system) yang digunakan pada setiap partisi.
-h: Opsi ini menghasilkan hasil dalam format yang mudah dibaca oleh manusia (dalam megabyte / gigabyte)
```

## Tampil disk usage / usage file atau disk
```
   du -h
   du -h /mnt/disk2
   du -sh /mnt/disk2 (utk lihat ukuran folder)
   du -h -time /mnt/disk2
   du -h /mnt/disk2 | sort -rn (sort ukuran)
   du -m /mnt/disk2
   atau lainnya du --help
```

## Buat partisi dan Format disk 
Jika ingin dipartisi unmount dahulu

Select disk yg diinginkan contoh :
```
    unmount /mnt/disk2
```

## Pilih MBR atau GPT
```
  untuk MBR menggunakan fdisk
  untuk GPT menggunakan gdisk
```
```
  jika fdisk / gdisk belum ada install package
  sudo apt install fdisk
  atau
  sudo apt install gdisk
```

## Contoh penggunaan fdisk
Tampil disk
```
    fdisk -l | grep /dev/sd
    Atau melihat disk dgn awalan nama sd**
    fdisk /dev/sd
```

Kemudian pilih disk yg diinginkan misalkan sdb1
```
    fdisk /dev/sdb1
    
    Tekan p utk tampilkan
    tekan m untuk help
    tekan t untuk change partition type dan pilih L untuk lihat code
```

## Contoh penggunaan gdisk
secara umum perintah gdisk hampir sama dengan fdisk.

Tampil disk
```
    gdisk -l | grep /dev/sd
    Atau melihat disk dgn awalan nama sd**
    gdisk /dev/sd
```

Kemudian pilih disk yg diinginkan misalkan sdb1
```
    gdisk /dev/sdb1
    
    Tekan p utk tampilkan
    tekan ? untuk help
    tekan t untuk change partition type dan pilih L untuk lihat code

    contoh new partition
    saat first sector di enter saja
    last sector baru diisikan misalkan +200GB
```

Melihat list disk & partisi
```
  sudo parted -l
```

Format disk / Buat filesystem
```
  mkfs -t tipeFileSystem /dev/namadisk
  atau
     mkfs.tipeFileSystem /dev/namadisk

  Contoh 
    mkfs -t ext4 /dev/sdb1
  Contoh format langsung diberi label disk
    mkfs.ext4 -L DATA /dev/sdb1
```



# Install dan Remove Aplikasi Native dan Snap
## Mencari aplikasi
contoh mencari aplikasi yg berkaitan dengan php
```
apt search php
```

## Install Aplikasi 
```
   sudo apt install nama_package
```

## List Aplikasi Terinstall
```
   sudo apt list --installed
```

## Hapus Instalasi (remove package apt)
```
   sudo apt remove nama_package
```
untuk full remove package
```
  sudo apt --purge autoremove nama_package
```

## install package deb
```
apt install ./nama-package.deb
```
atau
```
dpkg -i nama-package.deb
```

## Menampilkan informasi lokasi package (binary) beserta lokasi file confignya
contoh mencari package ssh
```
whereis ssh
```

## List Aplikasi/package yang diinstal dengan snap
```
  snap list
```
 
## Update package snap
```
  sudo snap refresh nama_package
```

# Service pada linux
## Mengelola service snap
```
  sudo snap enable nama_package
  sudo snap start nama_package
  sudo snap stop nama_package
  sudo snap disable nama_package
```


## Melihat service
```
  systemctl --type=service --state=running
```

## Me-restart service linux
```
sudo systemctl daemon-reload
```

## Start, stop, restart dan status service
```
  systemctl start <nama-service>
  systemctl stop <nama-service>
  systemctl restart <nama-service>
  systemctl status <nama-service>
```

## Aktifkan atau non aktifkan service saat boot
```
  systemctl enable <nama-service>
  systemctl disable <nama-service>
```

# Membuat service dari file binari linux
## contoh membuat service prometheus.
1. Lihat list file binary biasanya yang berwarna hijau
```
ls -l
```
2. copy file prometheus & promtool ke folder binary linux
```
sudo mv prometheus promtool /usr/local/bin/
```
3. membuat usergroup dan username khusus utk service prometheus agar aplikasi dapat mengakses tanpa sudo
```
sudo groupadd --system prometheus
```
```
sudo useradd --system -s /sbin/nologin -g prometheus prometheus
```
4. pindahkan folder konfigurasi & membuat folder konfigurasi prometheus
```
sudo mkdir /etc/prometheus
```
```
sudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheussudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheus
```
5. Membuat akses permission
```
sudo chown -R prometheus:prometheus /var/lib/prometheus/
```
6. Aktifkan Service
```
sudo systemctl enable --now prometheus.service
```

# Setting firewall dasar
Pastikan OpenSSH sudah diijinkan terlebih dahulu sebelum enable firewall agar dapat diremot
```
  sudo ufw app list

  sudo ufw allow OpenSSH
  
  Utk port SSH
  sudo ufw allow 22/tcp

  sudo ufw allow 80,443/tcp

  Utk zerotier
  sudo ufw allow 9993/udp
  sudo ufw allow 9993/tcp

  Utk zero trust cloudflare
  sudo ufw allow 7844/tcp
  sudo ufw allow 7844/udp

  Port utk peer-to-peer
  sudo ufw allow 3478/tcp
  sudo ufw allow 3478/udp

  Utk FTP Opsional jika dibutuhkan
  sudo ufw allow 21/tcp
```

sebelum enable firewall pastikan juga user telah diijinkan akses ssh agar dapat meremot
```
  sudo ufw enable

  sudo ufw status
  
```

format perintah hapus firewall
```
  ufw delete action port/app/number/protocol
```
contoh hapus firewall
```
  sudo ufw delete allow 8291
  sudo ufw delete allow 8088/tcp
```

## Konfigurasi Trusted Domain OpenSSH
```
sudo nano /etc/ssh/sshd_config
```
Cari baris yang mengandung opsi ListenAddress atau Listen (jika ada). Anda dapat menambahkan alamat IP atau nama domain yang ingin Anda jadikan trusted domain. Misalnya:
```
ListenAddress 192.168.1.100
ListenAddress mydomain.com
```
Restart OpenSSH Service
```
sudo systemctl restart ssh
```

# Mengatasi error remote SSH
## Reset key ssh client di windows
```
  contoh di windows hapus line ip dan key yg error
  C:\Users\User\.ssh\known_hosts
```

## Reset key ssh client di linux
```
cat ~/.ssh/known_hosts
```
atau
```
nano ~/.ssh/known_hosts
```
atau dengan perintah -R (remove)
```
ssh-keygen -R hostname/ip
```
contoh
```
ssh-keygen -R 192.168.88.2
```

## Cara lainnya jika reset key tidak berhasil
menggunakan package tail (tail berfungsi melihat file secara realtime)
melihat log ssh (utk cek login atau error)
```
  tampilkan log 100 baris saja
  tail /var/log/auth.log -n 100
```
```
  menyaring log dgn kata kunci tertentu
  tail /var/log/auth.log -n 100 | grep 'sshd'
```

jika remote ssh masih error install ulang openssh dan config ulang
```
  sudo apt update
  sudo apt-get --reinstall install openssh-server
  sudo systemctl status ssh
```
atau
```
sudo apt-get remove openssh-server
sudo apt-get install openssh-server
sudo systemctl status ssh
```

Konfigurasi /etc/ssh/sshd_config
```
  sudo systemctl stop sshd

  sudo nano /etc/ssh/sshd_config

  tambahkan user diakhir baris
     AllowUsers nama_pengguna

  atau tambahkan group
    AllowGroups sudo

  hapus # pada port 22
     port 22

  pastikan ssh dapat diakses dr ip yg diinginkan (contoh utk dr mana saja
     ListenAddress 0.0.0.0

  Ijinkan user root login
    PermitRootLogin Yes

  
  save dgn ctrl+o dan ctrl+x
  
  restart service ssh
     sudo systemctl restart sshd
```

penjelasan konfigurasi PermitRootLogin nano /etc/ssh/sshd_config
```
Perintah PermitRootLogin menentukan apakah user root dapat login menggunakan ssh. Argumen yang dapat digunakan adalah yes, prohibit-password, without-password, forced-commands-only, atau no. Secara default, argumennya adalah prohibit-password1.

Jika opsi ini diatur menjadi prohibit-password atau without-password, maka autentikasi menggunakan password dan keyboard-interactive dinonaktifkan untuk user root. Ini berarti user root hanya dapat login menggunakan metode lain, seperti kunci publik12.

Jika opsi ini diatur menjadi yes, maka user root dapat login menggunakan semua metode autentikasi, termasuk password. Ini berisiko karena user root dapat diserang secara brute force12.

Jika opsi ini diatur menjadi forced-commands-only, maka user root dapat login menggunakan kunci publik, tetapi hanya jika opsi command telah ditentukan. Ini berguna untuk menjalankan perintah tertentu secara remote, misalnya untuk backup12.

Jika opsi ini diatur menjadi no, maka user root tidak dapat login menggunakan ssh sama sekali
```

# Melihat list port dan service yang berjalan
```
sudo apt install net-tools
```
```
sudo netstat -tunlp
```
atau
```
sudo netstat -tlnp
```
filter tampil port contoh untuk filter port 80
```
sudo netstat -tlnp | grep 80
```
atau
```
  ss -ltpn

  atau dengan filter port 22
  ss -ltpn | grep :22
```

atau
```
  sudo lsof -n -i | grep nama_aplikasi
  contoh
  sudo lsof -n -i | grep openssh
```

# Lihat ip address 
## Melihat detil semua alamat ip & network name pada system
```
ifconfig
```
atau
```
  sudo ip add
```

## Melihat ip saja
```
hostname -I
```

## Konfigurasi ip address
```
sudo  nano /etc/netplan/00-installer-config.yaml
```
untuk DHCP
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: true
  version: 2
```

untuk static
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      addresses:
        - 192.168.88.3/24
      route:
        - to: default
          via: 192.168.88.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
  version: 2
```

# Perintah service docker
## Perintah service pada docker contoh
```
  sudo service docker stop
  sudo service docker start
  sudo service docker status
```

# Aplikasi-aplikasi yang sering digunakan
## Tailscale install
Install dari script tailscale
```
curl -fsSL https://tailscale.com/install.sh | sh
```
untuk cara install lengkapnya bisa lihat langsung di `https://tailscale.com/download`

## Zerotier install
```
 curl -s https://install.zerotier.com | sudo bash
```

## Join zerotier network
```
 sudo zerotier-cli join <network-id>
```

## Uninstall zerotier
```
apt remove zerotier-one
```

# Process Management
## Monitoring penggunaan hardware prosesor, ram, disk network dll 
task manager bawaan system
```
htop
```

## Filter list proses
contoh filter proses ssh
```
ps -aef | grep ssh
```

## menggunakan glances
```
    sudo apt install glances
```
```
    sudo glances
```

## Lihat properties pc
```
   sudo apt install neofetch 
```
```
   sudo neofetch 
```

## mematikan service
mematikan proses berdasarkan proses ID
```
kill nomor_id
```

mematikan proses berdasarkan nama proses
```
pkill nama_proses
```

## Menjalankan proses dibalik layar
```
nohup bash sample.sh &
```

contoh monitoring dari hasil output file sample.sh
```
tail -f filelog,txt
```

## melihat informasi file  yang dibuka oleh sebuah proses
contoh proses ssh
```
sudo lsof | grep ssh
```

# Compress dan Extract file
## Compress file tar.gz
saat eksekusi perintah hilangkan "[" & "]"
```
tar cvf [filename.tar.gz] [lokasi_directory]
```

## Extract file tar.gz
```
tar xvf nama_file.tar.gz
```

## Cara lainnya
meng-uncompress sebuah file zip (*.gz” or *.z). 
```
gunzip 
```
mengompress file (zip)
```
gzip
```

meng-uncompress file dengan format (*.bz2) dengan utiliti “bzip2”, digunakan pada file yang besar
```
bunzip2 
```

## Mengakses atau request URL
```
curl URL/bash_perintah.sh
```

## Mendownload file dari URL
```
wget URL/nama_file.extensi
```

## Melihat history perintah
```
history
```
## Menghapus history perintah
```
history -c
```

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

## 2. Mengubah Nilai Swappiness Secara Langsung
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

# Perintah Lainnya
Melihat uptime system
```
uptime -p
```

melihat hardware yang sedang beraktifitas 
```
Dmesg 
```

melihat informasi ip domain
```
nslookup [domain]
```
melihat informasi dns domain
```
dig [domain]
```
atau
```
host [domain]
```

Mencari perintah – perintah atau file yang mengandung huruf yang dimaksud
```
apropos 
```

menampilkan bantuan manual berdasarkan topik
```
apropos topic 
```

menampilkan calculator
```
bc 
```

menampilkan/melihat kalender
```
cal 
```

melihat printer yang telah disetup
```
cat /etc/printcap 
```

melihat informasi cpu
```
lscpu
```
atau
```
cat /proc/cpuinfo 
```

melihat penggunaan ram
```
free -h
```
atau lihat secara kontinu per 1 detik
```
watch -n 1 free -h
```

melihat filesystem yang digunakan
```
cat /proc/filesystems 
```

melihat alamat interrupt yang dipakai
```
cat /proc/interrupts 
```

melihat Versi dari Linux dan informasi lainnya
```
cat /proc/version 
```

melihat hardware komputer
```
Lshw 
```

melihat perangkat pci yang sedang terkoneksi ke komputer
```
Lspci 
```

melihat perangkat usb yang sedang terkoneksi ke komputer
```
Lsusb 
```

# Tentang perintah shutdown now dan reboot now pada default debian 12 tidak bisa running
Berikut adalah penjelasan mengapa perintah `shutdown now` dan `reboot now` mungkin tidak bekerja di Debian 12 GNOME, serta solusinya:

---

### **Penyebab Utama**
1. **Debian 12 menggunakan `systemd`**:  
   Perintah tradisional seperti `shutdown` atau `reboot` mungkin tidak tersedia atau memerlukan sintaks yang berbeda karena Debian menggunakan `systemd` sebagai init system.

2. **Kurangnya Hak Akses (Privilege)**:  
   Pengguna biasa (non-root) tidak memiliki izin untuk menjalankan perintah shutdown/reboot tanpa `sudo`.

3. **Sintaks Perintah Salah**:  
   - `shutdown now` tidak valid karena sintaks lengkapnya adalah:  
     ```bash
     shutdown -h now  # Matikan sistem
     shutdown -r now  # Restart sistem
     ```
   - Opsi `-h` (halt) atau `-r` (reboot) harus disertakan.

4. **Paket `sysvinit-utils` Tidak Terinstal**:  
   Pada instalasi minimal Debian, paket yang menyediakan perintah `shutdown` atau `reboot` mungkin tidak terpasang.

---

### **Solusi**

#### 1. **Gunakan `systemctl` (Direkomendasikan)**  
   Untuk sistem berbasis `systemd`, gunakan perintah berikut:  
   ```bash
   # Matikan sistem
   sudo systemctl poweroff

   # Restart sistem
   sudo systemctl reboot

   # Hentikan sistem (halt)
   sudo systemctl halt
   ```

#### 2. **Perbaiki Sintaks `shutdown`**  
   Gunakan sintaks yang benar:  
   ```bash
   # Matikan sistem
   sudo shutdown -h now

   # Restart sistem
   sudo shutdown -r now
   ```

#### 3. **Instal Paket `sysvinit-utils`**  
   Jika perintah `shutdown` atau `reboot` tidak ditemukan:  
   ```bash
   sudo apt update
   sudo apt install sysvinit-utils
   ```

#### 4. **Pastikan Pengguna Memiliki Hak Akses**  
   - Pastikan pengguna berada di grup `sudo`:  
     ```bash
     sudo usermod -aG sudo <nama_pengguna>
     ```
   - Setelah itu, **logout/login** atau reboot.

#### 5. **Periksa PolicyKit (Polkit) di GNOME**  
   GNOME menggunakan PolicyKit untuk mengontrol izin. Jika ada masalah, pastikan:  
   - File PolicyKit di `/etc/polkit-1/localauthority/50-local.d/` tidak membatasi akses shutdown/reboot.  
   - Atau jalankan perintah via `pkexec`:  
     ```bash
     pkexec shutdown -h now
     ```

---

### **Verifikasi**
- Cek apakah perintah tersedia:  
  ```bash
  which shutdown  # Harus mengembalikan /sbin/shutdown
  which reboot    # Harus mengembalikan /sbin/reboot
  ```

- Cek grup pengguna:  
  ```bash
  groups  # Pastikan pengguna termasuk dalam grup `sudo`
  ```

---

### **Catatan**
- Jika Anda menggunakan **GNOME Desktop**, Anda bisa mematikan/restart melalui GUI:  
  - Tekan `Alt + F2`, lalu ketik `gnome-session-quit --power-off`.  
  - Atau klik menu sistem di pojok kanan atas.

- Jika semua solusi gagal, pastikan tidak ada layanan atau proses yang menggantung (hang) yang menghalangi shutdown/reboot.

Semoga membantu! 🙌

# install aplikasi flatpak dari terminal 
Ya, aplikasi Flatpak dapat diinstal melalui terminal di Debian. Berikut langkah-langkahnya:

---

### **1. Instal Flatpak**  
Pastikan paket `flatpak` terinstal di sistem Anda:  
```bash
sudo apt update
sudo apt install flatpak
```

---

### **2. Tambahkan Repository Flathub**  
Flathub adalah repositori utama untuk aplikasi Flatpak. Tambahkan dengan perintah:  
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

### **3. Instal Aplikasi Flatpak**  
Cari aplikasi yang ingin diinstal (misal: GIMP) di [Flathub](https://flathub.org), lalu gunakan ID aplikasinya. Contoh:  
```bash
flatpak install flathub org.gimp.GIMP
```

- Untuk instalasi **sistem-wide** (untuk semua pengguna), tambahkan `--system`:  
  ```bash
  sudo flatpak install --system flathub org.gimp.GIMP
  ```

- Untuk instalasi **user-specific** (hanya pengguna saat ini), gunakan:  
  ```bash
  flatpak install --user flathub org.gimp.GIMP
  ```

---

### **4. Jalankan Aplikasi**  
Setelah terinstal, jalankan melalui terminal dengan:  
```bash
flatpak run <application-id>
# Contoh:
flatpak run org.gimp.GIMP
```

Atau cari aplikasi di menu aplikasi Debian (GUI).

---

### **5. Perbarui Aplikasi Flatpak**  
Update semua aplikasi Flatpak dengan:  
```bash
flatpak update
```

---

### **Catatan Penting**  
- Flatpak terpisah dari paket `.deb` Debian, sehingga aplikasi Flatpak tidak akan mengganggu paket sistem.  
- Pastikan sistem Debian Anda sudah diperbarui sebelum instalasi (`sudo apt update && sudo apt upgrade`).  
- Untuk menghapus aplikasi, gunakan:  
  ```bash
  flatpak uninstall <application-id>
  ```

Jika ada masalah, pastikan layanan `flatpak` aktif dan repositori Flathub sudah ditambahkan dengan benar.
