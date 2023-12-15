# Installasi dan Konfigurasi Proxmox
## Konfigurasi Disk Saat Install
saat install awal proxmox pada saat menu disk pilih option
kemudian pilih disk ukuran 40GB sisanya akan dipartisi setelah instalasi proxmox selesai
```
  lihat list disk
    lsblk

  pilih induk disk utk di create partition misalkan
    fdisk /dev/sda

  pilih m utk lihat list perintah
  lihat partisi yang tersedia kemudian buat partisi
    pilih n
  pilih angka default saja untuk mempartisi semua sisa partisi yg available

```

Format Disk ke ZFS

```
  dari menu node kemudian pilih Disks
  pilih partisi yg td telah dibuat misalkan /dev/sda4
    kemudian wipe
```
```
  Pilih menu ZFS
    Create: ZFS

  Contoh Pengisian konfigurasi
    Name : local-disk-pool
    Add Storage : Unceklist
```
