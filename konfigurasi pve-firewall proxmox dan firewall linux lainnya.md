# config pve-firewall proxmox
## dasar command
```
pve-firewall add rule <VM_ID> allow tcp --source <IP_ADDRESS> --dest <VM_IP_ADDRESS> --dest-port 22
```

Ganti <VM_ID> dengan ID VM yang ingin Anda akses, <IP_ADDRESS> dengan alamat IP Anda dari luar yang akan diizinkan, dan <VM_IP_ADDRESS> dengan alamat IP VM yang terkait.

## contoh mengijinkan port 22 diakses dari mana saja

```
pve-firewall add rule all allow tcp --dport 22
```

## contoh lainnya
```
pve-firewall add rule 100 allow tcp --source 192.168.88.0/24 --dest 0.0.0.0/0 --dest-port 22

```
Penjelasan dari perintah di atas:

--source 192.168.88.0/24 memungkinkan akses dari jaringan dengan alamat IP dalam rentang 192.168.88.1 hingga 192.168.88.254.
--dest 0.0.0.0/0 mengizinkan akses ke semua alamat IP VM yang ada pada Proxmox (VM dengan alamat IP apa pun).
--dest-port 22 mengizinkan akses hanya ke port 22 (port SSH).
Perintah tersebut akan menambahkan aturan firewall dengan nomor aturan 100 yang memungkinkan akses SSH dari jaringan 192.168.88.0/24 ke semua VM di Proxmox pada port 22.

## melihat daftar aturan firewall
```
pve-firewall list
```

## melihat status firewall
```
pve-firewall status
```

## mengaktifkan dan menonaktifkan firewall
```
pve-firewall stop
pve-firewall start
```

## Menonaktifkan (Disable) Layanan pve-firewall
```
systemctl stop pve-firewall.service
systemctl disable pve-firewall.service
```

## Mengaktifkan (Enable) Layanan pve-firewall
```
systemctl enable pve-firewall.service
systemctl start pve-firewall.service
```

# konfigurasi remote ssh umum dilinux
## mengizinkan akses SSH ke sistem Ubuntu dari sistem operasi selain Linux

```
sudo apt update
sudo apt install openssh-server
```

```
sudo systemctl enable ssh
sudo systemctl start ssh
```


# konfigurasi firewall dasar linux ubuntu
## Setting firewall dasar
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

## hapus firewall
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

## Melihat list port dan service yang berjalan
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


