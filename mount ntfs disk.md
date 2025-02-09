# Mount NTFS
Disk NTFS dapat dimount di Armbian. Berikut adalah beberapa langkah untuk melakukannya:

## Instal paket ntfs-3g (jika belum terpasang):
Pada Debian/Ubuntu, jalankan perintah berikut:
```
sudo apt install ntfs-3g
```

Pada Fedora/CentOS/AlmaLinux, gunakan perintah ini:
```
sudo dnf install ntfs-3g
```

Pada Arch Linux/Manjaro, gunakan perintah berikut:
```
sudo pacman -S ntfs-3g
```

## Identifikasi path dari partisi NTFS yang ingin Anda mount. 
Anda dapat menggunakan perintah 
```
parted -l 
```
untuk mengetahui pathnya. 

Misalnya, jika partisinya adalah 
```
/dev/sdb1, maka pathnya adalah /mnt/ntfs.
```

Buat direktori mount (misalnya /mnt/ntfs):
```
sudo mkdir -p /mnt/ntfs
```

## Mount partisi NTFS dengan izin baca-tulis:
```
sudo mount -t ntfs-3g /dev/sdb1 /mnt/ntfs
```

## Verifikasi izin mount dengan menjalankan perintah berikut:
```
mount | grep ntfs
```

Jika Anda melihat rw dalam output, itu berarti izin baca-tulis berhasil.

## Opsional: Agar partisi NTFS otomatis ter-mount saat sistem boot, 
tambahkan entri berikut ke file /etc/fstab:
```
/dev/sdb1 /mnt/ntfs ntfs defaults 0 0
```

Simpan dan tutup file, lalu jalankan perintah
```
 mount -a 
```
untuk memasang semua partisi yang dikonfigurasi dalam /etc/fstab.

# Mount fat32 disk
Verifikasi izin mount dengan menjalankan perintah berikut:
```
mount | grep fat32
```

Jika Anda melihat rw dalam output, itu berarti izin baca-tulis berhasil.

## Opsional: Agar partisi FAT32 otomatis ter-mount saat sistem boot, 
tambahkan entri berikut ke file /etc/fstab:
```
/dev/sda1 /mnt/fat32 vfat rw,user,exec,umask=000 0 0
```

Simpan dan tutup file, lalu jalankan perintah ```
mount -a 
```

untuk memasang semua partisi yang dikonfigurasi dalam /etc/fstab.

# trouble shooting mount ntfs
Berikut adalah langkah-langkah untuk memperbaiki masalah partisi NTFS di Debian:

### 1. **Pastikan NTFS-3G Terinstal**
NTFS-3G adalah driver NTFS yang stabil untuk Linux:
```bash
sudo apt update
sudo apt install ntfs-3g
```

### 2. **Nonaktifkan "Fast Startup" di Windows**
Ini mencegah Windows mengunci partisi NTFS:
1. Buka **Control Panel** > **Power Options**
2. Klik **Choose what the power buttons do**
3. Klik **Change settings that are currently unavailable**
4. Hilangkan centang **Turn on fast startup**
5. Simpan perubahan.

### 3. **Konfigurasi Mount Otomatis di `/etc/fstab`**
Edit file fstab:
```bash
sudo nano /etc/fstab
```
Tambahkan entri seperti ini (sesuaikan dengan partisi Anda):
```bash
UUID=XXXX-XXXX  /mnt/ntfs  ntfs-3g  defaults,windows_names,uid=1000,gid=1000,dmask=022,fmask=133,locale=en_US.utf8  0 0
```
- Ganti `UUID=XXXX-XXXX` dengan UUID partisi NTFS Anda (cek dengan `sudo blkid`).
- `windows_names`: Mencegah pembuatan nama file ilegal untuk Windows.
- `uid`/`gid`: Sesuaikan dengan ID user Debian Anda (biasanya 1000).
- `dmask`/`fmask`: Atur permission folder/file.

### 4. **Perbaiki Partisi NTFS (Jika Diperlukan)**
Jika partisi terkunci karena tidak dimatikan dengan benar:
```bash
sudo ntfsfix /dev/sdXn  # Ganti sdXn dengan partisi NTFS (e.g., sda1)
```

### 5. **Pastikan Mount Point Bersih**
```bash
sudo umount /mnt/ntfs  # Unmount partisi
sudo mount -a  # Mount ulang semua partisi di fstab
```

### 6. **Verifikasi Hasil**
- Coba buat file/folder di partisi NTFS dari Debian.
- Restart Debian dan cek apakah file tetap ada dan bisa diakses.
- Boot ke Windows dan verifikasi file bisa diakses.

### Jika Masih Ada Masalah:
1. **Cek Log Sistem:**
   ```bash
   journalctl -p 3 -xb  # Cari error terkait NTFS
   ```
2. **Coba Mount Manual dengan Debug:**
   ```bash
   sudo mount -t ntfs-3g -o debug /dev/sdXn /mnt/ntfs
   ```

### Catatan Penting:
- Hindari penggunaan karakter `* ? < > : " |` dalam nama file.
- Pastikan tidak ada kerusakan fisik pada disk (cek dengan `sudo smartctl -a /dev/sdX`).
- Jika menggunakan dual-boot, selalu shutdown Windows sepenuhnya (jangan hibernasi).

Dengan langkah ini, partisi NTFS seharusnya bisa ditulis secara permanen dan kompatibel dengan Windows.


## Berikut penjelasan rinci tentang `dmask` dan `fmask` serta angka `022` dan `133`:

---

### **Apa Itu `dmask` dan `fmask`?**
- **`dmask` (Directory Mask):**  
  Menentukan **permission default untuk direktori** pada partisi NTFS saat di-mount di Linux.
- **`fmask` (File Mask):**  
  Menentukan **permission default untuk file** pada partisi NTFS saat di-mount di Linux.

Kedua opsi ini menggunakan **nilai octal** (angka berbasis 8) untuk mengatur izin akses.

---

### **Apa Arti Angka `022` dan `133`?**
#### **1. Format Octal Permission**
Dalam Linux, permission diwakili oleh 3 digit octal:
- **Digit 1:** Permission untuk **user/pemilik** (owner).  
- **Digit 2:** Permission untuk **group**.  
- **Digit 3:** Permission untuk **others** (pengguna lain).  

Setiap digit adalah penjumlahan dari:
- **4** = Read (baca)  
- **2** = Write (tulis)  
- **1** = Execute (eksekusi)  

Contoh:
- **7** = 4+2+1 (read, write, execute)  
- **6** = 4+2 (read, write)  
- **5** = 4+1 (read, execute)  

---

#### **2. `dmask=022`**
- **Artinya:**  
  Direktori akan memiliki permission default: **`755`** (direktori bisa diakses oleh semua pengguna, tetapi hanya pemilik yang bisa menulis).

- **Penjelasan:**  
  - **`dmask=022`** = Mask untuk **menghilangkan izin write (tulis)** pada group dan others.  
  - Permission dasar untuk direktori di Linux adalah **`777`** (full access), tetapi mask `022` akan mengurangi:  
    ```
    777 (default)  
    - 022 (mask)  
    = 755 (hasil akhir)
    ```
  - **Hasil:**  
    - **Owner:** Read + Write + Execute (`7`)  
    - **Group:** Read + Execute (`5`)  
    - **Others:** Read + Execute (`5`)  

#### **3. `fmask=133`**
- **Artinya:**  
  File akan memiliki permission default: **`644`** (file bisa dibaca oleh semua pengguna, tetapi hanya pemilik yang bisa menulis).

- **Penjelasan:**  
  - **`fmask=133`** = Mask untuk **menghilangkan izin execute (eksekusi)** dan **write (tulis)** pada group/others.  
  - Permission dasar untuk file di Linux adalah **`666`** (read + write untuk semua), tetapi mask `133` akan mengurangi:  
    ```
    666 (default)  
    - 133 (mask)  
    = 644 (hasil akhir)
    ```
  - **Hasil:**  
    - **Owner:** Read + Write (`6`)  
    - **Group:** Read (`4`)  
    - **Others:** Read (`4`)  

---

### **Kenapa Menggunakan `dmask=022` dan `fmask=133`?**
1. **Kompatibilitas dengan Windows:**  
   NTFS tidak mendukung permission ala Linux, tetapi `dmask` dan `fmask` memastikan file/direktori bisa diakses oleh semua pengguna (mirip perilaku Windows).

2. **Kontrol Akses:**  
   - **Owner (pemilik):** Bisa membaca, menulis, dan mengakses direktori/file.  
   - **Others (pengguna lain):** Hanya bisa membaca (tidak bisa memodifikasi atau menghapus).

3. **Hindari Masalah "Permission Denied":**  
   Jika mask tidak diatur, Linux mungkin akan memberikan permission yang terlalu ketat (misalnya `700`), sehingga file tidak bisa diakses dari Windows atau oleh pengguna lain.

---

### **Contoh Kasus**
- Jika Anda membuat file di Debian dengan `fmask=133`:  
  - Di Linux: Permission file = `644` (pemilik bisa edit, lainnya hanya baca).  
  - Di Windows: File tetap bisa diakses karena Windows tidak peduli dengan permission Linux.  

- Jika Anda membuat direktori di Debian dengan `dmask=022`:  
  - Di Linux: Permission direktori = `755` (pemilik bisa edit, lainnya hanya lihat isi direktori).  
  - Di Windows: Direktori bisa diakses tanpa masalah.

---

### **Jika Ingin Lebih Ketat atau Longgar**
- **Agar semua pengguna bisa menulis:**  
  ```bash
  dmask=000,fmask=000  # Permission 777 untuk direktori dan 666 untuk file
  ```
- **Agar lebih ketat (hanya pemilik yang bisa akses):**  
  ```bash
  dmask=077,fmask=077  # Permission 700 untuk direktori dan 600 untuk file
  ```

---

### **Penting!**
- Pastikan **tidak menggunakan `umask`** karena cara kerjanya berbeda. Gunakan **`dmask`/`fmask`** khusus untuk NTFS.  
- Nilai ini hanya berlaku saat partisi di-mount di Linux. Saat diakses di Windows, permission Linux tidak berlaku.