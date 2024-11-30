# Jika linux gagal boot dengan sempurna karna error disk dan tampil saran gunakan fsck
- Masuk ke live cd linux sesuai distro
- masuk terminal
- cek disk yang akan direpair
  ```
  fdisk -l 
  ```
  atau
  ```
  lsblk
  ```
- jalankan repair disk. contoh disk yang tersimpan os yg akan di repair /dev/sda1
  ```
  sudo fsck -fy /dev/sda1
  ```
  fungsi
  ```
  fsck adalah singkatan dari file system check. Perintah ini berguna untuk memeriksa dan memperbaiki kesalahan pada sistem file.
  -f: Memaksa pemeriksaan.
  -y: Otomatis menjawab "ya" untuk semua pertanyaan.
  ```
- reboot linux. jika masih gagal boot gunakan package boot repair
- Install Boot Repair
  ```
  sudo add-apt-repository ppa:yannubuntu/boot-repair
  sudo apt update
  sudo apt install boot-repair
  ```
- jalankan boot repair dan ikuti pentunjuk dilayar
  ```
  sudo boot-repair
  ```
