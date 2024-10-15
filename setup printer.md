# SETUP SEBAGAI PRINT SERVER (SUPPORT ARMBIAN SERVER)
## pada server linux pastikan ip sudah jadi statik
## Update Repository
```
apt update
```  
## Install package CUPS
```
apt install cups
```
## cek status
```
systemctl status
```
## configurasi cups
```
nano /etc/cups/cupsd.conf
```
rubah only listen ....
```
rubah Listen localhost:631 menjadi port 631
```
tambah restric access to server `Allow @LOCAL`
```
<Location />
  Order allow.deny
  Allow @LOCAL
</location>
```
tambah restric access to the admin page `Allow @LOCAL`
```
<Location /admin>
  Order allow.deny
  Allow @LOCAL
</location>
```
kemudian save dan restart servie cups
```
systemctl restart cups
systemctl status cups
```

## Install Driver Printer HP
```
apt install hplip
```

## Install Driver Printer Universal
```
apt install printer-driver-gutenprint
```

## cek web server cups melalui browser
```
alamatip:631
```

## Add printer
```
masuk ke menu Administration kemudian Add Printer. masukkan password root
pilih printer kemudian ceklis
continue
```

```
ceklis Share This Printer
continue
```

```
Pilih Driver Printer Model
Add Printer
```

```
Setting Default Config Printer seperti ukuran kertas dll
```

## Install Driver untuk Network Printer agar dapat Memprint dari Client tanpa Install Driver
```
apt install avahi-daemon
```

```
systemctl enable avahi-daemon
systemctl status avahi-daemon
```

# Add Ukuran Kertas F4
```
masuk ke /etc/cups/ppd
```
1. pilih nama file printer dengan extensi ppd dan tambahkan konfiguasi kertas F4
2. Masukan Konfigurasi di deretan settingan kertas pada
```
*OPOptionHints PageSize: "dropdown"

dan

*OPOptionHints PageRegion: "dropdown"
```

```
*PageSize F4/Folio: "<</PageSize[612.000 935.000]/ImagingBBox null>>setpagedevice"
```

```
untuk kertas default sesuaikan
```

dan tambahkan juga di bagian ImageAbleArea
```
*DefaultImageableArea: F4
*StpDefaultImageableArea: Letter
```

```
*ImageAbleArea F4/Folio: "0.000 0.000 612.000 935.000"
```

dan tambahkan juga di bagian PaperDimension
```
*DefaultPaperDimension: F4
*StpDefaultPaperDimension: Letter
```

```
*PaperDimension F4/Folio: "612.000 935.000"
```

```
save
```









