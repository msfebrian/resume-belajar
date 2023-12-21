# Installasi dan Konfigurasi Proxmox
## Konfigurasi Disk Saat Install
saat install awal proxmox pada saat menu disk pilih option
```
  pilih disk ukuran
    20GB
  sisanya akan dipartisi setelah instalasi proxmox selesai
```

## Login Proxmox
Login default menggunakan username : root

## Setting Repositories
pilih node yang ada dibawah menu datacenter kemudian cari menu repositories
```
    disable component enterprise
      ceph.list
    dan 
      pve-enterprise
```

add repositories 
```
    pilih
      Repository : No-Subscription
```

melihat list repositories dari shell
```
    nano /etc/apt/source.list
```

update & upgrate proxmox
```
    masuk ke shell
    kemudian
        apt update
    dan
        apt upgrade
    atau
        apt update && apt upgrade -y
```

dibawah node keterangan penyimpanan default sebagai berikut
```
    local : adalah  tempat penyimpanan OS proxmox
    local-lvm : adalah default menyimpanan virtual machine
```

## Konfigurasi Disk Setelah Install
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
    Compression : lz4 (disarankan dari forum2 karna banyak kelebihan)
```

## Membuat tempat penyimpanan directory zfs
Masuk ke shell
```
 lihat list zfs
        zfs list
    buat pool utk penyimpanan Virtual Machine dan Container pada server1-disk-pool
        zfs create  server1-disk-pool/vmct
    buat pool utk penyimpanan ISO dan Image Container pada disk1-pool
        zfs create  server1-disk-pool/isoimg
    buat pool utk penyimpanan Backup pada disk1-pool
        zfs create  server1-disk-pool/backup
```

## Buat Directory utk penyimpanan
```
  dari menu Datacenter
    pilih Storage
    pilih Add Directory
    contoh pengisian
      ID : backup-pool
      Directory : /server1-disk-pool/isoimg (isi sesuai folder zfs yang sudah dibuat)
      Content : sesuaikan dengan peruntukan
    
```

## Cara Hapus ZFS
jika sudah ada list ZFS sdh ada hardisk tidak dapat di wipe, destroy zfs disk.
```
    Pilih Menu
        Disk -> ZFS
    Pilih ZFS yg akan dihapus
    di pojok kanan atas pilih
        More -> Desroy
```

## Cara Hapus Directory ZFS Pool
Hapus di Datacenter
```
    pilih datacenter
    storage 
    remove
```
cara via shell
```
    lihat list pool ZFS
        zfs list
    Kemudian hapus pool ZFS yang diinginkan
        zpool destroy -f Disk2
```


## Auto mount disk
sebelum auto mount pastikan lokasi mount sudah ada dgn mkdir dan disk sudah terformat
```
   lihat uuid disk
   lsblk -f

   nano /etc/fstab

   tambahkan 
   UUID= nomor-uuid /lokasi/mount ext4

   vefikasi mount apakah berhasil
   mount -a
   findmnt --verify
```

cara buat direktori pada disk ext4 sama dengan ZFS pastikan terlebih dahulu disk sudah terformat dan sdh dimount. di menu storage masukan lokasi mount dan pilih peruntukan content yang dinginkan 

## Install & Konfigurasi Zerotier
```
  curl -s https://install.zerotier.com | bash
```
  contoh perintah join
```
  zerotier-cli join d3ecf572698u91cd
```

