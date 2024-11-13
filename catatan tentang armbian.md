## Disable Kernel Update
```
masuk ke armbian-config
cari freeze kernel update
```

## jika menggunakan desktop environment matikan power sleep
matikan power sleep terutama pada menu setting

## Resolusi layar max 728p

Resolusi layar desktop yang direkomendasikan maximal 728p. jika lebih dr itu berat, lag

## Cara fix sound tdk befungsi di armbian
```
apt install alsamixer
```
kalau tidak bisa
```
apt-get install alsamixer
```
atau
```
sudo apt-get install -y gnome-alsamixer
```

## pilih opsi
```
opsi sebelah kiri pilih 
I2S 
sebelah kanan pilih 
AIU SPDIF SRC SEL [SPDIF]
```

# Install synaptic package manager
synaptic package manager adalah aplikasi gui untuk manajemen package di linux debian/ubuntu

update linux jika diperlukan 
```
sudo apt update
sudo apt upgrade
```
install synaptic package manager 
```
sudo apt-get install synaptic
```

# Install Driver 3d Lima Linux Armbian STB HG680P
```
sudo apt install inxi mesa-utils neofetch
```
aplikasi benchmark
```
sudo apt install glmark2 glmark2-es2
```
## cek mennggunakan LLVM atau 3d lima driver
``` 
glxinfo | grep "OpenGL"
```
## cek kemampuan LLVM
```
glmark2    
```

## mengaktifkan lima driver
```
sudo nano /etc/X11/xorg.conf.d/01-armbian-defaults.conf
sudo apt upgrade
```
update mesa driver
```
sudo add-apt-repository ppa:oibaf/graphics-drivers
sudo apt upgrade
```
cara cek menggunakan LLVM atau 3g lima driver
```
glxinfo | grep "OpenGL"
```

## menonaktifkan display compositing di xfce
masuk ke menu -> setting -> windows manager tweaks. nonaktifkan enable display compos glmark2
