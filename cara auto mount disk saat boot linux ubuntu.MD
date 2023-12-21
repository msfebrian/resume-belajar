# CARA AUTO MOUNT DISK SAAT BOOT DI LINUX UBUNTU SERVER

masuk terlebih dahulu ke super admin
```
sudo su
```

## Copy UUID Disk
Baca UUID Disk dengan perintah 
``` 
sudo lsblk -f
atau
sudo blkid
```
kemudian Copy UUID untuk dimasukkan ke fstab

pastikan folder tujuan mount sudah ada

misalkan disk akan di mount di /mnt/disk2

## Edit fstab
buka fstab dengan nano
```
sudo nano /etc/fstab
```
tambahkan baris di akhir seperti berikut
contoh 

```
# Auto Mount Disk2 ke /mnt/disk2
UUID=41267f53-8a46-4e67-971b-c02d55538ecb /mnt/disk2 ext4 defaults 0 2

```

jika error jangan gunakan/hapus default 0 2.


## Execute mount dan cek hasil konfigurasi mount sebelum reboot
execute mount fstab
```
sudo mount -a
```

pastikan disk2 sudah termount
```
sudo ls -l /mnt/disk
```

lakukan verifikasi dan pastikan tidak error sebelum reboot. jika error dan di reboot akan menyebabkan ke gagalan booting os

```
sudo findmnt --verify
```

jika tidak ada error maka rebbot os
```
sudo reboot now
```


## catatan dari open ai

Pada baris /etc/fstab, terdapat enam field yang mendefinisikan opsi dan parameter untuk mount point. Dua dari field tersebut adalah pass dan dump. Dalam konteks yang Anda tanyakan, yang dimaksud adalah:

1. pass (field ke-5): Angka ini menunjukkan apakah sistem akan melakukan pemeriksaan terhadap filesystem ketika booting. Jika nilainya 0, sistem tidak akan melakukan pemeriksaan. Jika nilainya 1 atau lebih, sistem akan memeriksa filesystem secara berurutan sesuai dengan angka tersebut. Misalnya, jika ada lebih dari satu filesystem yang diberi nilai 1, sistem akan memeriksa satu per satu dengan urutan angka.

2. dump (field ke-6): Field ini menunjukkan apakah sistem akan mencadangkan (dump) filesystem tersebut. Jika nilainya 0, filesystem tidak akan dicadangkan saat menjalankan perintah dump. Jika nilainya 1, sistem akan mencadangkan filesystem tersebut saat menjalankan perintah dump.

Dalam contoh yang Anda berikan, 0 dan 2 adalah nilai untuk field pass dan dump berturut-turut. Dalam kebanyakan kasus, nilai 0 pada field pass menunjukkan bahwa sistem tidak akan memeriksa filesystem tersebut saat boot, dan nilai 2 pada field dump menunjukkan bahwa filesystem akan dicadangkan jika diperintahkan untuk mencadangkan filesystem.

Field pass dan dump mungkin memiliki nilai yang berbeda-beda tergantung pada konfigurasi spesifik dari filesystem tersebut dan kebutuhan sistem Anda. Biasanya, untuk filesystem root (biasanya ditunjukkan dengan / dalam /etc/fstab), pass diberi nilai 1 agar sistem memeriksa secara urut saat booting. Sedangkan untuk filesystem lainnya, nilai 0 atau 2 adalah nilai yang umum diberikan.

Pastikan untuk merujuk pada dokumentasi sistem file dan kebutuhan spesifik dari sistem Anda untuk menentukan nilai yang tepat untuk field pass dan dump dalam konfigurasi /etc/fstab.
