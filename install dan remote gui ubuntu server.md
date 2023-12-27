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
