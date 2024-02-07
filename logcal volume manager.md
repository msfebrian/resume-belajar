# Logical Volume Manager LVM

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

lvcreate: Ini adalah perintah untuk membuat Logical Volume (LV) di dalam Volume Group (VG).
--type raid1: Opsi ini menentukan bahwa kita ingin membuat LV dengan konfigurasi RAID 1 (mirror). RAID 1 menggandakan data di antara dua atau lebih disk fisik sehingga jika salah satu disk mengalami kerusakan, data masih dapat diakses dari disk lainnya.
-m 1: Opsi ini menentukan tingkat mirroring. Angka 1 menunjukkan bahwa kita ingin menggandakan data (mirror) di antara dua segmen LV.
-l 100%FREE: Opsi ini menentukan ukuran LV. Dalam kasus ini, kita menggunakan seluruh ruang bebas yang tersedia di VG (myvg) untuk LV mymirror.
-n mymirror: Opsi ini menentukan nama LV yang akan dibuat, yaitu mymirror.

Dengan perintah di atas, kita membuat LV mymirror dengan konfigurasi RAID 1 pada VG myvg. LV ini akan memiliki dua segmen yang saling menggandakan data di antara dua disk fisik yang ada dalam VG.

## Konfirmasi segmen LV pada setiap disk
```
lvs -a -o name,copy_percent,devices myvg
```

