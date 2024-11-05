# Membuat Serive dan Auto Start KASMVNC VNCSERVER
## Membuat Layanan Otomatis untuk kasmvnc pada Debian

### Memahami Perintah dan Konsep
Sebelum kita masuk ke detail perintah, mari kita pahami konsep dasar yang terlibat.

* **Layanan (Service):** Ini adalah program yang berjalan di latar belakang sistem operasi dan menyediakan fungsi tertentu. Dalam kasus ini, kita ingin membuat layanan yang akan menjalankan aplikasi kasmvnc secara otomatis saat sistem boot.
* **kasmvnc:** Ini adalah aplikasi yang digunakan untuk mengakses desktop jarak jauh.
* **vncserver:** Ini adalah daemon yang menyediakan layanan VNC.
* **path bin:** Ini adalah direktori di mana executable program (seperti kasmvnc) biasanya berada.

### Langkah-langkah Membuat Layanan

#### 1. **Buat File Unit Service**
File unit service adalah file konfigurasi yang berisi instruksi tentang bagaimana layanan harus dimulai, dihentikan, dan dijalankan.

```bash
sudo nano /etc/systemd/system/kasmvnc.service
```

Pastekan konfigurasi berikut ke dalam file, sesuaikan dengan kebutuhan Anda:

```
[Unit]
Description=kasmvnc VNC Server
After=network.target

[Service]
Type=simple
User=your_username  # Ganti dengan username Anda
WorkingDirectory=/home/your_username  # Ganti dengan direktori home Anda
ExecStart=/usr/bin/kasmvnc :1  # Ganti :1 dengan nomor display yang diinginkan
ExecStop=/usr/bin/killall -TERM kasmvnc

[Install]
WantedBy=multi-user.target
```

* **Penjelasan Konfigurasi:**
  * `[Unit]`: Bagian ini berisi informasi umum tentang layanan.
  * `Description`: Deskripsi singkat tentang layanan.
  * `After`: Menentukan layanan lain yang harus sudah berjalan sebelum layanan ini dimulai.
  * `[Service]`: Bagian ini berisi instruksi untuk menjalankan layanan.
  * `Type`: Jenis layanan (simple dalam kasus ini).
  * `User`: User yang akan menjalankan layanan.
  * `WorkingDirectory`: Direktori kerja saat layanan dijalankan.
  * `ExecStart`: Perintah untuk memulai layanan.
  * `ExecStop`: Perintah untuk menghentikan layanan.
  * `[Install]`: Bagian ini menentukan kapan layanan harus diaktifkan.

#### 2. **Simpan dan Muat Ulang Daemon Systemd**
Simpan file konfigurasi dan muat ulang daemon systemd:

```bash
sudo systemctl daemon-reload
```

#### 3. **Aktifkan Layanan**
Untuk membuat layanan berjalan saat sistem boot:

```bash
sudo systemctl enable kasmvnc
```

#### 4. **Mulai Layanan**
Untuk memulai layanan sekarang:

```bash
sudo systemctl start kasmvnc
```

#### 5. **Verifikasi**
Untuk memeriksa status layanan:

```bash
sudo systemctl status kasmvnc
```

### Penjelasan Lebih Lanjut
* **Nomor Display:** Angka setelah `:` dalam `ExecStart` menentukan nomor display VNC. Anda bisa menggunakan nomor lain jika ingin menjalankan beberapa instance kasmvnc.
* **User dan Working Directory:** Pastikan user dan direktori kerja yang Anda tentukan memiliki izin yang tepat untuk menjalankan kasmvnc.
* **Killall:** Perintah `killall` digunakan untuk menghentikan semua proses kasmvnc yang berjalan.
* **Systemd:** Systemd adalah init system yang digunakan pada kebanyakan distribusi Linux modern. Ini mengelola layanan sistem.

### Tips Tambahan
* **Konfigurasi kasmvnc:** Setelah layanan berjalan, Anda bisa mengkonfigurasi kasmvnc lebih lanjut menggunakan opsi baris perintah atau file konfigurasi.
* **Log:** Periksa log sistem untuk melihat pesan error atau informasi lainnya terkait layanan.
* **Firewall:** Pastikan firewall Anda mengizinkan koneksi VNC ke sistem Anda.

### Contoh Konfigurasi Lengkap (dengan password):
```
ExecStart=/usr/bin/kasmvnc -geometry 1280x1024 -password /home/your_username/.vnc/password :1
```
Perintah di atas akan memulai kasmvnc dengan resolusi 1280x1024 dan menggunakan password yang disimpan di file `/home/your_username/.vnc/password`.

**Catatan:**
* Ganti `your_username` dengan nama pengguna Anda.
* Sesuaikan jalur ke executable kasmvnc, nomor display, dan opsi lainnya sesuai dengan kebutuhan Anda.
* Untuk keamanan yang lebih baik, pertimbangkan untuk menggunakan SSH tunneling atau VPN untuk mengakses VNC.
