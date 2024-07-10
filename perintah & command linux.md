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
masuk ke root user
```
sudo su
```
masuk ke user tertentu
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

## lihat daftar user dan status grupnya.

Lihat list user & foldernya
```
   cat /etc/paswd atau getent passwd
```
Lihat hanya nama pengguna (cut dan awk)
```
   cut -d: -f1 /etc/passwd
```

melihat list user yg ada digroup sudo
```
   sudo getent group sudo
```

melihat status grup username
```
   sudo groups nama_username
```

Lihat direktori pengguna
```
   ls /home
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

## Perintah update distro dan kernel baru
upgrade ke versi distro yang lebih baru atau menginstall kernel baru
```
sudo apt dist-upgrade
```
## Install paket update-manager-core
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

## Melihat versi linux dan nama host
```
hostnamectl
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
    mkdir /mnt/disk2
    mount /dev/sdb1 /mnt/disk2
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

## install package deb
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

hapus firewall
```
   sudo ufw delete allow 8291
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

## Zerotier install
```
 curl -s https://install.zerotier.com | sudo bash
```

## Join zerotier network
```
 sudo zerotier-cli join <network-id>
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
