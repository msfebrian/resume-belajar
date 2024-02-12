# Logical Volume Manager LVM

# Tentang LVM
- LVM dapat menggabungkan beberapa fisik disk/penyimpanan menjadi satu partisi/volume.
- Partisi LVM lebih fleksibel dari partisi konvensional
- fitur lainnya pada LVM ada raid, LVM Thin (kapasitas volume bisa melebihi fisik disk)

## Konsep/Tahapan buat LVM
## 1.  buat partisi misalkan dengan fdisk/gdisk untuk Physical Volume LVM
```
  jika sudah ada partisinya type partisi bisa dirubah.
  pilih t di fdisk
    dan lihat list Type Partisi dengan pilih L.
  contoh untuk type LVM id nya 83
```
## 2. buat Physical Volume (PV) nya.
Perintah Physical Volme Create.
```
lihat daftar partisi
  lsblk

buat PV pvcreate /nama/partisi
  contoh
  pvcreate /dev/sdb1
```

lihat list Physical Volume LVM
```
pvs
```

## 3. buat Volume Group (VG)
```
vgcreate namaVG /PhysicalVolume/ygSudahDibuat
```

contoh
```
vgcreate vg-data /dev/sdb1
```

cara lihat list VG
```
vgs
```

melihat informasi Volume Group yang sudah dibuat
```
vgdisplay nama_volume_group
```

keterangan informasi VG Size dan Physical Extend (PE)
```
PE Size x Total PE = Nilainya Sama dengan VG Size
```

## 4. buat Logical Volume (LV) - (data dapat disimpan hanya di LV)
lv create dengan VG Size gunakan parameter -L ("L" Besar)
```
lvcreate -L 200MiB namaVG -n namaLV 
```

untuk lv create dengan PE Size gunakan parameter -l ("l" Kecil).
- contoh untuk buat size 200MB dengan informasi di vgdisplay PE Size 4MB. maka PE size kalikan 50
```
lvcreate -l 50 namaVG -n namaLV 
```

cara lihat list LV
```
lvs
```

cara lihat informasi LV seperti path dll
```
lvdisplay
```

## 5. Format Logical Volume (LV)
lihat dulu listnya
```
lsblk
```

- hati2 jangan sampai salah format physical groupnya
- Contoh struktur lvm
```
sdb                   <- disk
--sdb1                <- Partisi LVM
  |--namaVG-namaLV    <- namaVG dan namaLV (ini path yang utk diformat LVM)
```

contoh format partisi lvm
```
mkfs.ext4 /dev/namaVG/namaLV
```

bisa juga menggunakan mapper
```
mkfs.ext4 /dev/mapper/namVG-namaLV
```

## Mounting LVM di fstab
buka fstab
```
  nano /etc/fstab
```
tambah baris di fstab untuk automount lvm
```
  /dev/namaVG/namaLV /lokasi/mount ext4 defaults 0 0
```

mount dan verifikasi
```
  mount -av
```

# Extend LVM
## 1. Volume Group Extend (VG Extend)
VG Extend berfungsi menambah size VG dari disk lain. perintahnya
```
vgextend namaVG_yg_ingin_ditambah /dev/sumberDisk
```
contoh
```
lihat dulu list partisi PV
  pvs

kemudian pilih. contoh
  vgextend vg1 /dev/vdc1
```

## 2. Logical Volume Extend (PV Extend) Menambah size
## HATI-HATI JGN GUNAKAN mkfs (karna itu berfungsi format, data bisa hilang)
cara buatlv extend
```
lvextend -l +jumlahPE /namaDevice/nama_VG_Existing/nama_LV_Existing
```
contoh
```
lvextend -l +50 dev/vg1/lv-1
```
```
jika ingin satuan dalam Bit
"-l" gunakan L besar "-L"
```
contoh 
```
lvextend -L +200MiB dev/vg1/lv-2
```

## 3. set akumulasi jumlah file system khusus tipe ext
```
resize2fs /dev/namaVG/namaLV
```
cek 
```
df -h
```


# Perintah membuat raid mirror LVM

## Buat PV pada setiap partisi disk
```
pvcreate /dev/sda1
pvcreate /dev/sdb1
```

## Buat VG yang menggabungkan PV
```
vgcreate myvg /dev/sda1 /dev/sdb1
```

## Buat LV dengan RAID 1
```
lvcreate --type raid1 -m 1 -l 100%FREE -n mymirror myvg
```
perintah lvcreate untuk membuat Logical Volume (LV) dengan RAID 1 dan nama mymirror pada Volume Group (VG) yang disebut myvg.

1. lvcreate: Ini adalah perintah untuk membuat Logical Volume (LV) di dalam Volume Group (VG).

2. --type raid1: Opsi ini menentukan bahwa kita ingin membuat LV dengan konfigurasi RAID 1 (mirror). RAID 1 menggandakan data di antara dua atau lebih disk fisik sehingga jika salah satu disk mengalami kerusakan, data masih dapat diakses dari disk lainnya.

3. -m 1: Opsi ini menentukan tingkat mirroring. Angka 1 menunjukkan bahwa kita ingin menggandakan data (mirror) di antara dua segmen LV.

4. -l 100%FREE: Opsi ini menentukan ukuran LV. Dalam kasus ini, kita menggunakan seluruh ruang bebas yang tersedia di VG (myvg) untuk LV mymirror.

5. -n mymirror: Opsi ini menentukan nama LV yang akan dibuat, yaitu mymirror.

Dengan perintah di atas, kita membuat LV mymirror dengan konfigurasi RAID 1 pada VG myvg. LV ini akan memiliki dua segmen yang saling menggandakan data di antara dua disk fisik yang ada dalam VG.

## Konfirmasi segmen LV pada setiap disk
```
lvs -a -o name,copy_percent,devices myvg
```
perintah ini digunakan untuk melihat informasi rinci tentang Logical Volume (LV) dalam Volume Group (VG) yang disebut myvg. Mari kita jelaskan setiap bagian dari output perintah ini:

1. my_lv_rimage_0(0), my_lv_rimage_1(0), my_lv_rimage_2(0):

- Ini adalah segmen data dari LV yang menggunakan RAID 1 (mirror).
- Angka dalam tanda kurung menunjukkan persentase salinan data di antara segmen LV. Dalam hal ini, persentase salinan adalah 0%, yang berarti segmen data ini belum disalin ke segmen lainnya.

2. [my_lv_rimage_0] /dev/sde1(1), [my_lv_rimage_1] /dev/sdb1(1), [my_lv_rimage_2] /dev/sdd1(1):
- Ini menunjukkan perangkat fisik yang terlibat dalam segmen data LV.
- /dev/sde1, /dev/sdb1, dan /dev/sdd1 adalah partisi pada disk fisik yang digunakan untuk menyimpan data LV.

3. [my_lv_rmeta_0] /dev/sde1(0), [my_lv_rmeta_1] /dev/sdb1(0), [my_lv_rmeta_2] /dev/sdd1(0):
- Ini adalah segmen metadata dari LV RAID 1.
Segmen metadata menyimpan informasi konfigurasi RAID dan digunakan untuk mengelola LV.
- Angka dalam tanda kurung menunjukkan persentase salinan metadata di antara segmen LV. Dalam hal ini, persentase salinan adalah 0%, yang berarti segmen metadata ini belum disalin ke segmen lainnya.

Jadi, perintah tersebut memberikan informasi tentang segmen data dan metadata LV RAID 1 dalam VG myvg.


## Mount Logical Volume
mount Logical Volume (LV) yang telah Anda buat sebelumnya:

1. Periksa Nama Logical Volume (LV):
Jalankan perintah lvs untuk mendapatkan informasi tentang logical volume yang telah dibuat. Anda akan melihat daftar LV beserta nama dan path-nya.

2. Buat Titik Mount (Mount Point):
Gunakan perintah mkdir untuk membuat titik mount (direktori) di mana Anda ingin me-mount LV tersebut. Misalnya, Anda dapat membuat direktori /mnt/mymirror sebagai titik mount.

3. Mount Logical Volume:
Gunakan perintah mount untuk me-mount LV ke dalam titik mount yang telah Anda buat. Gantilah DEVICE dengan path LV yang sesuai.
Contoh: 
```
sudo mount /dev/mapper/myvg-mymirror /mnt/mymirror
```

4. Verifikasi Mounting:
Jalankan perintah mount tanpa argumen untuk memeriksa status mounting. Anda akan melihat LV dan titik mount yang terkait.
```
Contoh output: 
/dev/mapper/myvg-mymirror on /mnt/mymirror type ext4 (rw,relatime)
```

5. Jika ingin auto mount konfigurasi pada /etc/fstab

# mengganti disk rusak dan menambahkan disk baru pada Logical Volume Manager (LVM):

1. Mengganti Disk Rusak:
- Identifikasi disk yang rusak dengan menjalankan perintah lvs atau pvs. Perhatikan nama Logical Volume (LV) yang terkait dengan disk rusak.

2. Hapus disk rusak dari Volume Group (VG) dengan perintah 
```
vgreduce --removemissing VG_NAME.
```

3. Matikan sistem dengan perintah
```
shutdown -h now
```

4. Ganti disk lama yang rusak dengan disk baru.
Nyalakan sistem kembali.

5. Menambahkan Disk Baru:
- Inisialisasi disk baru dengan perintah 
```
pvcreate /dev/sdX.
```

- Tambahkan Physical Volume (PV) baru ke Volume Group (VG) dengan perintah 
```
vgextend VG_NAME /dev/sdX
```

- Buat Logical Volume (LV) baru atau perluas LV yang ada dengan perintah lvcreate atau lvextend.

- Buat sistem file pada LV dengan perintah 
``` 
mkfs.ext4 /dev/VG_NAME/LV_NAME
```

- Buat titik mount (direktori) untuk LV dengan perintah 
```
mkdir /mnt/MOUNT_POINT
```

- Mount LV ke titik mount dengan perintah 
```
mount /dev/VG_NAME/LV_NAME /mnt/MOUNT_POINT.
```

- Tambahkan entri ke /etc/fstab agar LV otomatis ter-mount saat sistem boot.

- Pastikan untuk mengganti VG_NAME, LV_NAME, /dev/sdX, dan MOUNT_POINT sesuai dengan konfigurasi Anda.
