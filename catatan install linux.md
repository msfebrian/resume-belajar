# Setting Penyimpanan Instalasi Ubuntu Server
## standar alokasi disk
ukuran swap disesuaikan dengan ram (tidak menggunakan jg tidak apa2)
```
  contoh swap 4G
```
boot disk
```
  boot disk 2G
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

## penanganan error saat pembuatan partisi disk
hapus semua partisi dlu menggunakan acronis partition atau software lainnya.

## penggunaan volume grup (vg) & lvm (list volume manager) .....

## install snap sdh di test di armbian
```
sudo apt update
sudo apt install
sudo reboot now
sudo snap install core
```

## install font Microsoft windows
```
sudo apt update
sudo apt install ttf-mscorefonts-installer
```