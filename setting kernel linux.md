# Cara install kernel linux
```
apt install linux-image-VERSI_KERNELNYA
```
# Cara restore ke kernel default
```
menghapus kernel baru dengan synaptic manager filter linux-app
```
# Tahapan kembali ke kernel lama dengan grub config
## lihat daftar kernel terinstall
```
dpkg -l | grep linux-image
```
atau 
```
dpkg -l | grep linux-image | grep -v hwe 
```

## lihat kernel yang digunakan
- lihat kernel terinstall
```
uname -srn
```
utk lebih detil arsitektur
```
uname -srnm
```
atau
```
hostnamectl
```
atau
```
cat /proc/version
```
atau
```
lsb_release -a
```

- lihat array kernel pada grub.cfg dengan filter berdasarkan versi distro contoh utk LMDE 6. (list versi lengkap bisa di lihat dari `nano /boot/grub/grub.cfg` pada bagian menu entry) 
```
cat /boot/grub/grub.cfg | grep -iE "menuentry 'LMDE 6 Faye, with Linux" | awk '{print i++ " : "$1, $2, $3, $4, $5, $6, $7}'
```
contoh tampilnya setelah filter
```
0 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-25-amd64'
1 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-25-amd64
2 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-23-amd64'
3 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-23-amd64
4 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-12-amd64'
5 : menuentry 'LMDE 6 Faye, with Linux 6.1.0-12-amd64
```

## modify GRUB_DEFAULT=0 dengan memilih kernel yang akan diload
- jika `GRUB_DEFAULT=0` maka hanya kernel yg terakhir saja yg diload
- jika `GRUB_DEFAULT="1>2"` maka hanya kernel array ke 1 sampai ke 2 yang diload
```
sudo nano /etc/default/grub
```
- lihat konfigurasi grub yg sudah di edit 
```
grep GRUB_DEFAULT /etc/default/grub
```

## Regenerate GRUB config file with midified settings
```
sudo update-grub
```

## Reboot and validate booted kernel
```
sudo systemctl reboot
```

# Mendisable atau menahan update kernel agar tidak terupdate
Pada Debian 12 (dan distro Debian-based lainnya), Anda dapat menghentikan update kernel menggunakan metode berikut:

1. **Gunakan APT Hold untuk Kernel Package:**

   Debian memiliki perintah untuk "menahan" paket agar tidak diperbarui. Anda bisa menggunakan perintah berikut untuk mencegah kernel dari pembaruan otomatis:

   ```bash
   sudo apt-mark hold linux-image-$(uname -r)
   ```

   Perintah ini akan "memegang" (hold) versi kernel yang saat ini sedang Anda gunakan, sehingga tidak akan diperbarui saat Anda melakukan `apt upgrade`.

   Untuk melepas (unhold) kernel dan mengizinkan update kembali, gunakan:

   ```bash
   sudo apt-mark unhold linux-image-$(uname -r)
   ```

2. **Blokir Kernel Update melalui File Preferences (Optional):**

   Anda juga bisa menggunakan file `preferences` di APT untuk memastikan kernel tidak diupdate. Langkah-langkahnya:

   - Buat atau edit file di `/etc/apt/preferences.d/` (misalnya, `no-kernel-upgrade`):

     ```bash
     sudo nano /etc/apt/preferences.d/no-kernel-upgrade
     ```

   - Tambahkan baris berikut untuk menahan paket kernel:

     ```plaintext
     Package: linux-image*
     Pin: release a=stable
     Pin-Priority: -1
     ```

     Dengan pengaturan di atas, APT tidak akan memperbarui paket kernel (apapun yang diawali dengan `linux-image`). Jika Anda ingin menyesuaikan agar hanya memblokir versi tertentu, Anda dapat mengganti `*` dengan versi kernel yang ingin Anda tahan.

3. **Cek dan Verifikasi:**

   Setelah melakukan perubahan, Anda bisa memverifikasi apakah kernel sudah "ditahan" dari update dengan menjalankan:

   ```bash
   apt-mark showhold
   ```

   Ini akan menunjukkan daftar paket yang ditandai sebagai "hold."

Dengan langkah-langkah di atas, kernel pada Debian Anda tidak akan diperbarui secara otomatis.
