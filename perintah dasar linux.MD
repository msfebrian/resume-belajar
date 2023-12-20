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

## Menambahkan user dan merubah password
Merubah password
```
  sudo passwd nama_user
```
Tekan enter masukkan password lama kemudian password baru 2 kali

Menambah user
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

Lihat list user & foldernya
```
   cat /etc/paswd atau getent passwd
```
Lihat hanya nama pengguna (cut dan awk)
```
   cut -d: -f1 /etc/passwd
```
Lihat direktori pengguna
```
   ls /home
```

## Konfigurasi Date Timezone
Set Default timezone cara mudah (rekomeded)
```
   sudo dpkg-reconfigure tzdata
      pilih asia
          Pilih jakarta
             tampil target timezone
               ..... timezone 'Asia/Jakarta'
```

Cara lainnya :

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

## Mengelola File & Folder

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

melihat semua file lengkap
```
ls –l 
```

buat folder
```
    mkdir nama-folder
    
    bisa juga tambahkan path
    mkdir /path_directorinya/nama_foldernya
```

Hapus folder
```
    rmdir nama_folder
    
    bisa juga tambahkan path
    rmdir /path_directorinya/nama_foldernya
```

Hapus file
```
  rm nama_file
```

menghapus semua file nya saja, tanpa direktori dan subdirektorinya
```
    rm –f
    
    bisa juga tambahkan path
    rm -f /path_directorinya/..
```

menghapus file, direktori dan subdirektorinya (recursive remove)
```
    rm –r  
    bisa juga tambahkan path
    rm -r /path_directorinya/..
```

Memindahkan file, bisa juga untuk merubah nama sebuah file
```
pindah folder
    mv /lokasi_folder /path_tujuan_atau_nama_folder_yg _diinginkan

ganti nama file
    mv /lokasi_folder/nama_file /path_tujuan/nama_file_yg_diinginkan
    
    contoh
    mv /home/data.txt /mnt/disk2 
    mv /home/datapribadi /mnt/disk2
```

Copy file
```
    cp /path/sumber /path/tujuan 
```

text editor (notepad)
```
    sudo nano 
```

Buat, Buka dan edit file dengan nano
```
  sudo nano nama_file

    untuk simpan ctr + o kemudian masukkan nama file dan enter
    untuk keluar ctr + x
```

Membuat file

```
    cat > nama_file
```

Membuat file bernama “bin” dengan isi file “Biodata”
```
Contoh : 
    cat > bin lalu “enter” Biodata
    
    ctrl+c (utk mengakhiri dan simpan)
```

Membuka file dan edit dengan vim 
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

## Mengelola dan Memformat Disk 
Melihat list disk & mounting point
```
  lsblk
```
tampil kan jg UUID
```
  lsblk -f
``` 

Tampil disk
```
    fdisk -l | grep /dev/sd
    Atau
    fdisk /dev/sd
```

Melihat list disk & partisi
```
  sudo parted -l
```

Cara mount
```
    mkdir /mnt/disk2
    mount /dev/sdb1 /mnt/disk2
```


Tampil disk file system list
```
  df -h atau df -Th
```

Tampil disk usage / usage file atau disk
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
```

Melihat list disk & partisi
```
  sudo parted -l
```

Format disk / Buat filesystem
```
  mkfs -t ext4 /dev/namadisk
     Contoh 
        mkfs -t ext4 /dev/sdb1
     Contoh format langsung diberi label disk
        mkfs.ext4 -L DATA /dev/sdb1
```

## Install dan Remove Aplikasi Native dan Snap

Install Aplikasi 
```
   sudo apt install nama_package
```
List Aplikasi Terinstall
```
   sudo apt list --installed
```
Hapus Instalasi (remove package apt)
```
   sudo apt remove nama_package
```


List Aplikasi/package yang diinstal dengan snap
```
  snap list
```
 
Update package snap
```
  sudo snap refresh nama_package
```

Mengelola service snap
```
  sudo snap enable nama_package
  sudo snap start nama_package
  sudo snap stop nama_package
  sudo snap disable nama_package
```


Melihat service
```
  systemctl --type=service --state=running
```

Start, stop, restart dan status service
```
  systemctl start <nama-service>
  systemctl stop <nama-service>
  systemctl restart <nama-service>
  systemctl status <nama-service>
```

Aktifkan atau non aktifkan service saat boot
```
  systemctl enable <nama-service>
  systemctl disable <nama-service>
```

Setting firewall dasar
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

  sudo ufw enable

  sudo ufw status
  
```

hapus firewall
```
   sudo ufw delete allow 8291
```


Lihat ip address 
```
  sudo ip add
```

Konfigurasi ip address
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

## Perintah service docker
Perintah service pada docker contoh
```
  sudo service docker stop
  sudo service docker start
  sudo service docker status
```

## Aplikasi-aplikasi yang sering digunakan

Zerotier install
```
 curl -s https://install.zerotier.com | sudo bash
```

Join zerotier network
```
 sudo zerotier-cli join <network-id>
```

Monitoring penggunaan hardware prosesor, ram, disk network dll 
```
    sudo apt install glances
```
```
    sudo glances
```

Lihat properties pc
```
   sudo apt install neofetch 
```
```
   sudo neofetch 
```

## Perintah Lainnya


melihat hardware yang sedang beraktifitas 
```
Dmesg 
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

menampilkan/melihat kalender
```
cal 
```

melihat printer yang telah disetup
```
cat /etc/printcap 
```

melihat file pada /proc directori yang bukan merupakan file nyata
```
cat /proc/cpuinfo 
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



