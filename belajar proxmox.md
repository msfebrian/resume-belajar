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

# Konfigurasi Swapp
Swappiness pada sistem Ubuntu mengontrol seberapa sering file swap digunakan. Nilai swappiness mengatur kecenderungan kernel untuk memindahkan proses dari memori fisik ke disk swap.
## 1. Swappiness pada sistem Ubuntu mengontrol seberapa sering file swap digunakan. Nilai swappiness mengatur kecenderungan kernel untuk memindahkan proses dari memori fisik ke disk swap. 
```
nano /etc/sysctl.conf
```
Tambahkan atau ubah baris berikut:
```
vm.swappiness = 10
```
- Simpan perubahan dan tutup berkas.
- Nilai ini menunjukkan bahwa swap hanya akan digunakan ketika penggunaan RAM mencapai sekitar 80 hingga 90 persen.
- Terapkan perubahan swapp
```
sudo sysctl -p
```

## 2. Mengubah Nilai Swappiness Secara Langsung (terkadang cara ini suka ke reset)
Jika Anda ingin mengubah nilai swappiness tanpa harus me-reboot sistem, jalankan perintah berikut:
```
sysctl vm.swappiness=10
```

## 3. Verifikasi Nilai Swappiness
Anda dapat memeriksa nilai swappiness saat ini dengan menjalankan:
```
cat /proc/sys/vm/swappiness
```
Nilai default di Ubuntu adalah 60, tetapi menguranginya menjadi 10 akan meningkatkan performa secara keseluruhan untuk instalasi desktop Ubuntu yang umum.

## 4. Sangat dianjurkan untuk reboot utk alokasi ulang RAM


# Konfigurasi Disk Setelah Install
## Create Partition
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

## Menggunakan LVM (Rekomended)
dengan LVM partition dan size disk lebih fleksibel dan dapat dibuat raid.

1. dari Menu Disk pilih partition yg belum digunakan kemudian pilih Wipe Disk
2. Pilih Menu LVM buat Volume Gruop.
   - Create:Volume Group & Pilih disk yang belum digunakan
   - Isi Nama VG Contoh Name : pve-data
   - Jangan Ceklis Add Storage (agar dapat disetting untuk penyimpan semua data)
4. Masuk ke terminal buat LVM
   ```
   lsblk
   untuk lihat list VG atau dengan
   vgdisplay
   ```
   buat LVM
   ```
   lvcreate -L ukuran namaVG -n namaLV
   contoh
   lvcreate -L 200G pve-data -n pve-storage  
   ```

   cek hasil
   ```
   lvs
   atau
   lvdisplay
   ```

5. Format LVM
   untuk path lihat di lvdisplay
   ```
   mkfs.ext4 /dev/namaVG/namaLV
   ```
   
7. buat auto mount di /etc/fstab
   lihat path LV
   ```
   lvdisplay
   ```

   mount ke fstab
   ```
   nano /etc/fstab
   ```

   contoh penambahan kode di fstab
   ```
   # Mount LVM dari partisi 2
   /dev/pve-data/pve-storage /mnt/pve/second-partition ext4 defaults 0 0
   ```

   cek dan pastikan berhasil termount
   ```
   findmnt --verify
   mount -a
   reboot now
   ```

8. Buat Directory utk penyimpanan
```
  dari menu Datacenter
    pilih Storage
    pilih Add Directory
    contoh pengisian
      ID : pve-storage
      Directory : /mnt/pve/second-partition (isi sesuai mount lvm yang sudah di set)
      Content : sesuaikan dengan peruntukan 
```
   

## Menggunakan ext4
1. dari posisi node proxmox pilih menu Disks -> pilih disk partition yang belum digunakan kemudian Wipe Disk
2. ke menu Disk -> Directory -> Create:Directory
3. Isian Disk : pilih disk partition yang belum digunakan
4. Filesystem : ext4
5. Name : Isi_Nama(misalkan second-partition)
6. ceklis Add Storage 

# Menggunakan ZFS (RAM minimal diatas 8gb)
ZFS menggunakan RAM untuk cache dilihat dari dokumentasi per 1 TB memakan 1 GB RAM. dan diharuskan menggunakan UPS untuk menghindari kegagalan penyimpanan data dari RAM ke Disk
## Format Disk ke ZFS

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

# Qemu Guest Agent
QEMU Guest Agent adalah sebuah agen perangkat lunak yang digunakan untuk berkomunikasi antara host (mesin fisik) dan tamu (mesin virtual) dalam lingkungan virtualisasi yang menggunakan QEMU sebagai hypervisor1. Berikut adalah beberapa fungsi utama dari QEMU Guest Agent:

1. Shutdown yang Tepat: Agen ini memungkinkan penghentian yang benar pada mesin tamu, bukan hanya mengandalkan perintah ACPI atau kebijakan Windows.
2. Pembekuan Sistem File Tamu: Saat membuat salinan cadangan atau snapshot, agen ini membekukan sistem file tamu (pada Windows, menggunakan layanan Volume Shadow Copy Service (VSS)). Ini membantu memastikan konsistensi data.
3. Sinkronisasi Waktu: Setelah mesin tamu dijeda (misalnya setelah snapshot), agen ini langsung menyelaraskan waktu mesin tamu dengan hypervisor menggunakan QEMU Guest Agent sebagai langkah pertama

## Aktifkan QEMU Guest Agent di Proxmox:
1. Buka antarmuka web Proxmox.
2. Pilih VM Anda, lalu pergi ke tab Options.
3. Temukan opsi QEMU Guest Agent dan aktifkan.

## Install Package Qemu Guest Agent Linux
Ubuntu / Debian
```
apt-get install qemu-guest-agent
```

Redhat / RPM / RHEL
```
yum install qemu-guest-agent
```

## Aktifkan Service
Aktifkan layanan agar berjalan secara otomatis saat boot:
```
sudo systemctl enable qemu-guest-agent
```

Mulai QEMU Guest Agent:
```
sudo systemctl start qemu-guest-agent
```

Verifikasi bahwa QEMU Guest Agent berjalan dengan baik:
Anda dapat memeriksa status layanan dengan perintah berikut:
systemctl status qemu-guest-agent

Jika layanan belum berjalan, Anda dapat menggunakan Services control panel untuk memulainya dan memastikan agar berjalan otomatis pada boot berikutnya

## Install Package Qemu Guest Agent Windows
1. unduh driver virtio-win (lihat Windows VirtIO Drivers). Kemudian, instal driver virtio-serial:
2. Pasang ISO ke VM Windows Anda (virtio-*.iso).
3. Buka Device Manager di Windows.
- Cari “PCI Simple Communications Controller”.
- Klik kanan -> Update Driver dan pilih direktori yang terpasang di DRIVE:\vioserial\<OSVERSION>\ (di mana <OSVERSION> adalah versi Windows, misalnya 2k12R2 untuk Windows 2012 R2).
- Setelah itu, instal QEMU Guest Agent:
- Buka ISO yang terpasang di explorer.
- Installer agen tamu ada di direktori guest-agent.
- Jalankan installer dengan mengklik dua kali (gunakan qemu-ga-x86_64.msi untuk 64-bit atau qemu-ga-i386.msi untuk 32-bit).
- Pastikan QEMU Guest Agent berjalan dengan memeriksa daftar Windows Services atau melalui PowerShell:
```
Get-Service QEMU-GA
```
- Jika belum berjalan, gunakan Services control panel untuk memulai dan mengatur agar berjalan otomatis saat boot berikutnya

# Import VMDK dan OVA ke proxmox
## export dan extract
1. kirim file ke root proxmox bisa dengan filezilla
2. jika extensi ova extract dahulu agar dahulu. karna vmdk biasanya dikemas dalam ova dengan perintah
```
tar -xvf nama-imagenya.ova
```
3. buat virtual machine kemudian remove disk bawaan dengan cara klik tombol `detach` dan `remove` dari menu hardware. dan `ingatkan ID virtual machinenya`
4. import namafile dgn extensi `vmdk` di root folder hasil extract ke ID Virtual machine dengan perintah
```
qm importdisk no_id nama_file_vmdknya lokasi_storage_pve --format format_yg_diinginkan
```
contoh
```
qm importdisk 104 ubuntu-vm-disk001.vmdk pve-storage -format vmdk 
```
5. edit disk hasil import di menu hardware
```
pilih disk hasil import
klik edit
ceklis discard (jika hardware menggunakan ssd) fungsi untuk tidak menggunakan blok yg tidak digunakan agar memperjang umur ssd dan optimasi performa
ceklis ssd emulation (jika hardware menggunakan ssd)
klik Add
```
6. pilih boot order
```
naikkan posisi disk ke urutan pertama setelah cd/iso
dan ceklis disk boot
```

# merubah disk.qcow2 dari preallocation on menjadi off agar ukuran mengikuti dapat menyesuaikan dengan size real
1. jika preallocation `on` file disk akan mereservasi ukuran disk yang ditentukan
2. jika preallocation `off` file disk akan bertambah bertahap sesuai dengan size yg digunakan
3. esuaikan permission dengan menggunakan chown namauser:namagroup agar dapat di edit
disk qcow2 asumsi dari pc diluar server proxmox

4. edit preallocation dengan qemu-img convert
contoh
```
sudo qemu-img convert -f qcow2 -0 qcow2 -o preallocation=off manjaro.qcow2 manjaro_new.qcow2
```
5. import seperti cara sebelumnya ke dalam proxmox


