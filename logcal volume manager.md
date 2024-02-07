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

## Konfirmasi segmen LV pada setiap disk
```
lvs -a -o name,copy_percent,devices myvg
```

