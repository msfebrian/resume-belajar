# cara restore dari timeshift via cli untuk recovery system linux yang error
**Menentukan Partisi Tujuan:**

* **Partisi yang sama:** Jika Anda ingin mengembalikan sistem ke kondisi sebelumnya pada partisi yang sama, maka perintah yang sudah saya berikan sebelumnya dapat langsung digunakan.
* **Partisi berbeda:** Jika Anda ingin mengembalikan sistem ke partisi yang berbeda (misalnya, ke partisi yang baru dibuat), maka Anda perlu mengganti titik mount pada perintah `mount` dan menyesuaikan perintah restore.

**Contoh Perintah dengan Partisi Tujuan yang Berbeda:**

Misalkan Anda ingin mengembalikan sistem ke partisi `/dev/sdb1`:

1. **Unmount partisi tujuan (jika sudah ter-mount):**
   ```bash
   sudo umount /dev/sdb1
   ```
2. **Mount partisi sumber (partisi yang berisi snapshot Timeshift):**
   ```bash
   sudo mount /dev/sda3 /mnt
   ```
   Ganti `/dev/sda3` dengan partisi yang benar.
3. **Jalankan Timeshift:**
   ```bash
   /mnt/var/lib/timeshift/timeshift --restore <nomor_snapshot> --target /dev/sdb1
   ```
   Perhatikan penambahan `--target /dev/sdb1` untuk menentukan partisi tujuan.

**Penjelasan Tambahan:**

* **`--target`:** Opsi ini digunakan untuk menentukan partisi tujuan restore.
* **Peringatan:** Pastikan Anda telah membuat partisi tujuan dengan ukuran yang cukup dan memformatnya dengan sistem file yang sama dengan partisi sumber.

**Contoh Lengkap:**

```bash
# Unmount partisi tujuan
sudo umount /dev/sdb1

# Mount partisi sumber
sudo mount /dev/sda3 /mnt

# Lihat daftar snapshot
/mnt/var/lib/timeshift/timeshift --list

# Misal, ingin restore snapshot nomor 3 ke /dev/sdb1
/mnt/var/lib/timeshift/timeshift --restore 3 --target /dev/sdb1

# Reboot sistem
reboot
```

**Pertimbangan Lain:**

* **Sistem File:** Pastikan sistem file pada partisi sumber dan tujuan kompatibel.
* **Ukuran Partisi:** Pastikan partisi tujuan memiliki ruang yang cukup untuk menampung data yang akan dipulihkan.
* **Bootloader:** Setelah restore, Anda mungkin perlu mengkonfigurasi bootloader agar sistem dapat boot dari partisi yang baru.

**Penting:** Sebelum melakukan restore, sangat disarankan untuk membuat backup data penting Anda terlebih dahulu. Proses restore dapat menimpa data yang ada di partisi tujuan.

**Apakah Anda memiliki pertanyaan lebih lanjut atau ingin contoh yang lebih spesifik?**

**Tips Tambahan:**

* **Gunakan tab completion:** Untuk mempercepat proses pengetikan perintah, Anda bisa menggunakan tab completion pada terminal. Ketikkan beberapa huruf pertama dari nama perintah atau direktori, lalu tekan tombol Tab untuk melengkapi secara otomatis.
* **Manfaatkan man pages:** Jika Anda ingin mengetahui detail tentang suatu perintah, gunakan `man <nama_perintah>`. Misalnya, untuk melihat manual halaman perintah `timeshift`, ketikkan `man timeshift`.

Dengan mengikuti langkah-langkah di atas dan mempertimbangkan faktor-faktor yang disebutkan, Anda dapat melakukan restore sistem Linux Anda dengan aman dan efektif menggunakan Timeshift.
