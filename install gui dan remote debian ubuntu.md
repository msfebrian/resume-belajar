# Cara Install GUI di Debian & Ubuntu Server 22.04 dan Remote VNC Server
# Install Desktop Environment dengan Tasksel
## Install Tasksel
Update dan update dahulu
```
sudo apt update
sudo apt upgrade
```

Install Tasksel
```
sudo apt tasksel
```

running tasksel untuk menginstall DE
```
sudo tasksel
```

```
Pilih "Ubuntu Desktop" atau DE lain yang Anda inginkan menggunakan tombol
panah, lalu tekan 'Space' untuk memilihnya. Setelah dipilih, tekan 'Enter'.
```

## Mulai Desktop Environment
```
sudo systemctl isolate graphical.target
sudo reboot now
```

## Hapus DE Melalui Tasksel
```
sudo tasksel remove nama_desktop_environment
```
contoh
```
sudo tasksel remove ubuntu-desktop
```

## Cara hapus melalui terminal
```
sudo apt autoremove --purge <narna-desktop-package>
```

contoh
```
sudo apt autoremove --purge ubuntu-desktop
```

Hapus Paket Tersisa
```
sudo apt-get purge $(dpkg -l | grep '^rc' | awk '{print $2}')
```

```
sudo apt update
sudo apt upgrade
sudo reboot now
```

# Manual Install Desktop Environment tanpa Tasksel
## Install Gnome
```
apt update
```
```
apt install gnome
```
```
apt install gnome-core gnome-shell gnome-terminal
```

# Install xfce desktop
```
sudo apt install -y xfce4 xfce4-goodies
```

# Install KDE Plasma
```
apt install kde-standard
```
```
kemudian pilih ssdm
```
```
SDDM: Ideal jika Anda menggunakan KDE Plasma sebagai lingkungan desktop utama Anda dan menginginkan tampilan yang bersih dan modern.
LightDM: Cocok jika Anda sering berganti-ganti antara berbagai lingkungan desktop atau ingin memiliki fleksibilitas tinggi dalam mengkonfigurasi tampilan login.
GDM: Jika Anda pengguna GNOME sejati dan ingin pengalaman yang konsisten dengan ekosistem GNOME.
```

## Setting Efek
Cari desktop effect
```
Wobbly Windows : untuk efek jelly saat windows di gerakkan
Magic Lamp : efek minimaize dan maximize ala mac
```

## install theme ala mac
```
1. get new global themes
2. cari apple-ventura-dark by adolfo
3. setelah terdownload ceklis semua, Appreance setting dan Desktop and Windows Layout. kemudian apply
```

# Install TigerVNC server untuk remote
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

# buat file konfigurasi start up VNC Server
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

## Edit /etc/xrdp/startwm.sh
```
etc/xrdp/startwm.sh
```
pagarkan code2 ini
```
# test -x /etc/X11/Xsession && exec /etc/X11/Xsession
# exec /bin/sh /etc/X11/Xsession
```
tambahkan code diakhir
```
exec gnome-session
```

## Ijinkan port xrdp di port 3389
```
sudo ufw allow 3389/tcp
sudo ufw reload
sudo ufw status 
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

# xrdp on linux Mint for versions older than 20.2.
Please find below the commands that need to be applied:
```
    sudo apt upgrade
    sudo apt install xrdp xorgxrdp
    sudo apt install xserver-xorg-input-all
    sudo adduser xrdp ssl-cert
    sudo ufw allow 3389/tcp
    echo env -u SESSION_MANAGER -u DBUS_SESSION_BUS_ADDRESS cinnamon-session~/.xsession
```



