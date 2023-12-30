# Cara Install GUI di Ubuntu Server 22.04 dan Remote VNC Server
## Install xfce desktop
```
sudo apt install -y xfce4 xfce4-goodies
```

## Install TigerVNC server untuk remote
```
sudo apt install -y tigervnc-standalone-server
```

## tambah user baru
tidak disarankan pakai user root
```
 sudo useradd -m -s /bin/bash nama_user_baru
```
-m : membuat direktori home untuk user baru

## login ke user baru
```
su - nama_user_baru
```

## buat file konfigurasi start up VNC Server
```
vim .vnc/xstartup
atau
nano .vnc/xstartup
```

iskan konfigurasi seperti ini :
```
/usr/bin/startxfce4
```
jika gunakan vim ketik :wq utk simpan dan keluar

## beri akses execute pada .vnc/xstartup
```
chmod +x .vnc/xstartup
```

## jalankan service & atur agar dapat diakses dari luar jaringan
```
  vncserver -localhost no
```
```
  atur password baru VNC Server
```
Pilih n (NO) agar view only tidak digunakan
```
  would yo like to enter a view-only password (y/n)? n
```

## atur Port VNC di firewall
VNC menggunakan port 5901
```
ufw allow 5901
```

## untuk remote client
untuk remote client di platform mobile bisa gunakan bVNC
```
download aplikasi bVNC
```
```
untuk login gunakan username baru
masukkan ip & port 5901
masukkan username & password
```

# Install XRDP Server
xrdp berfungsi untuk mengakses ubuntu via aplikasi remote desktop connection microsoft

## Install Package xrdp
```
sudo apt install xrdp
```

## add xrdp user to the group
by default xrdp menggunakan /etc/ssl/private/ssl-cert-snakeoil.key. 
file ini hanya dapat dibaca oleh member group "ssl-cert.
```
sudo adduser xrdp ssl-cert
```

## restart xrdp dan cek service 
```
sudo systemctl restart xrdp
```
```
sudo systemctl status xrdp
```

## Ijinkan port xrdp di port 3389
```
sudo ufw allow 3389 
```

# Mengaktifkan RDP di ARMBIAN 21
masuk ke terminal
```
sudo armbian-config
```
```
pilih software
pilih rdp enable
```
```
jika tulisan pada config rdp enable (berarti belum aktif)
jika tulisan pada config rdp disable (berarti sudah aktif)
```
jika ingin ganti nama hostname
```
pada armbian-config
pilih personal
kemudian pilih hostname
dan rumah nama hostname
kemudian reboot
```





