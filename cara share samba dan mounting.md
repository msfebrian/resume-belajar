# Cara share dengan samba dan mounting share smb
# Cara buat share folder dengan samba

## Instalasi samba
```
sudo apt update
sudo apt install samba
```

## backup default config samba

```
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

## edit konfigurasi samba
```
sudo nano /etc/samba/smb.conf
```

## tambahkan di bagian akhir file smb.conf:
```
[nama_share]
   path = /lokasi/folder/anda
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no
   create mask = 0777
   directory mask = 0777
```
1. Ganti [nama_share] dengan nama yang ingin Anda gunakan untuk share folder.
2. path adalah lokasi dari folder yang ingin Anda bagikan.
3. Sesuaikan opsi lain sesuai dengan kebutuhan akses yang Anda inginkan.

## untuk menambahkan user pada konfi samba
contoh user tertentu. tambahkan konfig setelah read only 
```
[usb_storage]
comment = CasaOS share usb_storage
public = Yes
path = /DATA/usb_storage
browseable = Yes
read only = No
guest ok = Yes
writeable = Yes
valid users = nama_user
create mask = 0777
directory mask = 0777
```

## fungsi force user = root dan penjelasan lengkap dari gemini
```
force user = root
```

## Memahami `force user = root` dalam Konfigurasi Samba

**Fungsi `force user = root`** dalam konfigurasi Samba adalah untuk memaksa semua permintaan akses ke share Samba tertentu agar dianggap berasal dari pengguna `root`. Ini berarti, tidak peduli siapa yang mencoba mengakses share tersebut, sistem akan memperlakukannya seolah-olah adalah pengguna `root` dengan semua hak akses yang dimiliki oleh pengguna `root`.

**Mengapa menggunakan `force user = root`?**

* **Sederhana:** Dalam beberapa kasus, memberikan akses penuh ke semua pengguna mungkin lebih mudah daripada mengelola izin pengguna secara individual.
* **Keamanan:** Meskipun memberikan akses penuh, penggunaan `force user = root` seringkali dikombinasikan dengan pengaturan izin direktori yang ketat untuk membatasi tindakan yang dapat dilakukan oleh pengguna.
* **Skrip dan Automasi:** Dalam skrip atau automasi, memaksa semua pengguna menjadi `root` dapat menyederhanakan logika dan mengurangi kompleksitas.

**Kapan harus berhati-hati:**

* **Risiko Keamanan:** Memberikan akses penuh sebagai `root` dapat menjadi celah keamanan jika tidak dikelola dengan benar.
* **Konflik Izin:** Jika ada konflik antara izin yang ditetapkan pada share Samba dan izin sistem file, perilaku yang tidak terduga dapat terjadi.

## Menambahkan Password untuk Pengguna `febrian` dalam Konfigurasi Samba

**Cara umum untuk menambahkan pengguna dan mengatur password dalam konfigurasi Samba:**

1. **Buat pengguna sistem:**
   ```bash
   useradd febrian
   passwd febrian
   ```
2. **Edit file konfigurasi Samba:**
   Biasanya, file konfigurasi Samba adalah `/etc/samba/smb.conf`. Tambahkan sebuah section baru untuk pengguna `febrian`. Misalnya:

   ```
   [febrianshare]
   path = /home/febrian/shared
   browseable = yes
   writable = yes
   valid users = febrian
   ```

   * **`path`:** Direktori yang akan di-share.
   * **`browseable`:** Menentukan apakah share akan muncul dalam browser jaringan.
   * **`writable`:** Menentukan apakah pengguna dapat menulis ke share.
   * **`valid users`:** Menentukan pengguna yang diperbolehkan mengakses share.

3. **Restart layanan Samba:**
   ```bash
   sudo systemctl restart smbd nmbd
   ```

**Penting:**

* **Keamanan:** Jangan memberikan akses tulis penuh ke direktori yang sensitif seperti `/etc`.
* **Izin Direktori:** Pastikan izin direktori yang di-share sudah diatur dengan benar agar pengguna `febrian` memiliki akses yang sesuai.
* **Pengguna Lokal:** Pengguna yang Anda tambahkan di Samba harus ada sebagai pengguna lokal pada sistem operasi.
* **Grup:** Anda juga bisa menggunakan grup untuk mengatur izin. Misalnya:

   ```
   valid users = @febriangroup
   ```

**Catatan:**

* Konfigurasi Samba bisa sangat kompleks dan memiliki banyak opsi. Dokumentasi resmi Samba adalah referensi terbaik untuk memahami semua fitur yang tersedia.
* **Jangan gunakan `force user = root`** jika Anda ingin mengelola izin pengguna secara individual dan aman.
* **Pertimbangkan keamanan:** Selalu prioritaskan keamanan ketika mengkonfigurasi Samba.

**Contoh Konfigurasi Lengkap (dengan `force user = root` untuk share tertentu):**

```
[global]
   workgroup = WORKGROUP
   server string = Samba Server %v
   security = user

[public]
   path = /srv/samba/public
   browseable = yes
   public = yes
   writable = yes
   force user = root

[febrianshare]
   path = /home/febrian/shared
   browseable = yes
   writable = yes
   valid users = febrian
```

Dalam contoh di atas, semua pengguna yang mengakses `[public]` akan dianggap sebagai `root`, sedangkan akses ke `[febrianshare]` dibatasi hanya untuk pengguna `febrian`.

**Dengan mengikuti langkah-langkah di atas dan mempertimbangkan aspek keamanan, Anda dapat dengan sukses menambahkan pengguna dan mengatur izin dalam konfigurasi Samba Anda.**

**Apakah Anda memiliki pertanyaan lebih lanjut tentang konfigurasi Samba?** 


## buat user utk smb

```
sudo smbpasswd -a nama_pengguna
```

## restart samba
```
sudo systemctl restart smbd
```

## ijinkan firewall 
```
sudo ufw allow samba
```

## cara akses difile explorer
```
smb://alamat_IP_server/nama_share
```

## cara buka smb dr terminal
```
smbclient //alamat_IP_server/nama_share -U nama_pengguna
```

## Cara Mounting Share smb
Buat folder utk lokasi mount contoh
```
 mkdir /home/samba
```
install aplikasi cifs-utils dengan perintah:
```
sudo apt-get install cifs-utils
```
contoh cara mounting smb
```
sudo mount -t cifs -o user=nama_user //192.168.2.1/shared /home/samba/
```

atau
```
sudo mount -t cifs //<alamat_IP_server>/<nama_berbagi> /mnt/smb -o username=<nama_pengguna>,password=<kata_sandi>
```

lihat apakah sdh ada share smb yg sudah dimount 
```
df -h
```
untuk unmounting caranya
```
sudo umount /home/samba
```

# cara automount dari fstab
## buat directory jika blm ada
```
sudo mkdir /mnt/nama_mount_point
```

## edit fstab
```
sudo nano /etc/fstab
```

## tambahkan pada fstab
```
//alamat_IP_server/nama_share /mnt/nama_mount_point cifs credentials=/path/to/credentials_file,uid=nama_pengguna_lokal,gid=nama_grup_lokal 0 0
```

1. alamat_IP_server: Alamat IP server.
2. nama_share: Nama folder yang dibagikan.
3. /mnt/nama_mount_point: Lokasi di mana folder tersebut akan di-mount.
4. credentials=/path/to/credentials_file: Path menuju file yang berisi kredensial pengguna (biasanya berisi username=nama_pengguna\npassword=sandi_pengguna).
5. nama_pengguna_lokal: Nama pengguna lokal di Ubuntu.
6. nama_grup_lokal: Nama grup pengguna lokal di Ubuntu.

lakukan verifikasi & mount 
```
sudo findmnt --verify
```

```
sudo mount -a
```

## Buat file untuk menyimpan kredensial pengguna (opsional tapi direkomendasikan).
Untuk keamanan, disarankan untuk menyimpan kredensial pengguna dalam file terpisah. Buat file teks yang berisi kredensial pengguna di lokasi yang aman. Misalnya:
```
sudo nano /path/to/credentials_file
```

isi file seperti ini
```
username=nama_pengguna
password=sandi_pengguna
```

atur permission agar hanya dpt diakses oleh root
```
sudo chmod 600 /path/to/credentials_file
```

lakukan verifikasi & mount 
```
sudo findmnt --verify
```

```
sudo mount -a
```

# Contoh Implementasi set SMB untuk dua folder dengan dua user berbeda
contoh share folder yang akan dibuat yaitu folder sigit dan dadan
## Buat Folder
```
sudo mkdir /smb/sigit
sudo mkdir /smb/dadan
```

## Tambahkan konfigurasi di file configurasi smb
untuk default debian dan ubuntu konfigurasi file gunakan smb.conf 
```
sudo nano /etc/samba/smb.conf
```
untuk casa os konfigurasi file gunakan smb.casa.conf 
```
sudo nano /etc/samba/smb.casa.conf
```
Tambahkan konfigurasi berikut di akhir file:
```
[sigit]
path = /smb/sigit
valid users = sigit
read only = no
browseable = yes

[dadan]
path = /smb/dadan
valid users = dadan
read only = no
browseable = yes
```

## Buat user Samba dan set password:
```
sudo adduser sigit
sudo smbpasswd -a sigit
# Masukkan password: sigitAuth

sudo adduser dadan
sudo smbpasswd -a dadan
# Masukkan password: dadanAuth
```

## Restart layanan Samba:
```
sudo systemctl restart smbd
```



