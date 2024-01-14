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
sudo
```
install synaptic package manager 
```
sudo apt-get install synaptic
```
