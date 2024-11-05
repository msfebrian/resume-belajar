# Tutorial dari CHATGPT
# Membuat Serive dan Auto Start KASMVNC VNCSERVER
Untuk membuat service otomatis pada Debian yang menjalankan aplikasi `kasmvnc` dengan package `vncserver` dari path `bin`, ikuti langkah-langkah berikut:

### Langkah 1: Buat File Service
Buat file service di `/etc/systemd/system` dengan nama `kasmvnc.service`. Gunakan editor teks seperti `nano`:

```bash
sudo nano /etc/systemd/system/kasmvnc.service
```

### Langkah 2: Tulis Konfigurasi Service
Salin konfigurasi di bawah ini ke dalam file `kasmvnc.service`. Sesuaikan path dan user sesuai dengan kebutuhan Anda.

```ini
[Unit]
Description=KasmVNC Server
After=network.target

[Service]
Type=simple
User=<username>                   # Ganti dengan user yang benar
ExecStart=/path/to/bin/kasmvnc      # Ganti dengan path lengkap ke kasmvnc
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Keterangan:**
- `ExecStart`: Tentukan path lengkap ke binary `kasmvnc` yang ingin dijalankan.
- `User`: Tentukan user yang akan menjalankan service ini.

### Langkah 3: Reload systemd dan Aktifkan Service

Setelah file service selesai disimpan, reload systemd agar perubahan terbaca:

```bash
sudo systemctl daemon-reload
```

Kemudian, aktifkan service agar berjalan otomatis saat sistem boot:

```bash
sudo systemctl enable kasmvnc
```

### Langkah 4: Mulai dan Cek Status Service

Mulai service dengan perintah:

```bash
sudo systemctl start kasmvnc
```

Untuk memeriksa status service dan memastikan tidak ada error:

```bash
sudo systemctl status kasmvnc
```

### Langkah 5: Pengujian

Setelah service berjalan, Anda dapat menguji apakah `kasmvnc` sudah aktif dengan mengakses aplikasi VNC yang sudah diset sebelumnya.

Jika Anda memerlukan konfigurasi tambahan, seperti variabel lingkungan atau opsi lain untuk `kasmvnc`, tambahkan pada bagian `[Service]` dengan `Environment="VAR=VALUE"` atau sesuaikan `ExecStart`.

Selamat mencoba!

# Contoh menjalankan service KASMVNC dengan package yang dijalankan VNCSERVER
## Buat File Service
Buat file service di /etc/systemd/system dengan nama kasmvnc.service. Gunakan editor teks seperti nano:
```
[Unit]
Description=KasmVNC Server
After=network.target

[Service]
Type=simple
User=root
ExecStart=/bin/vncserver
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## Reload systemd dan Aktifkan Service
```
sudo systemctl daemon-reload
```

## Enable kan dan cek status
```
sudo systemctl enable kasmvnc
```
```
sudo systemctl start kasmvnc
```
```
sudo systemctl status kasmvnc
```

# Troubleshooting saat gagal auto run saat reboot
Untuk mengatasi masalah ini, kita perlu memodifikasi file service agar service tetap berjalan di background. Anda bisa mengubah service menjadi `Type=forking` atau menambahkan script yang memastikan bahwa service berjalan terus. Berikut adalah beberapa langkah yang bisa Anda coba.

### Solusi 1: Mengubah `Type` menjadi `forking`

Jika `kasmvnc` menghasilkan proses yang berjalan di background (misalnya, dengan mode daemon atau multi-threaded), kita bisa mengubah `Type` menjadi `forking`. Edit file `/etc/systemd/system/kasmvnc.service` sebagai berikut:

1. Buka file service:
    ```bash
    sudo nano /etc/systemd/system/kasmvnc.service
    ```

2. Ubah baris `Type=simple` menjadi `Type=forking`:

    ```ini
    [Service]
    Type=forking
    User=<username>                   
    ExecStart=/path/to/bin/kasmvnc      
    PIDFile=/run/kasmvnc.pid            # Optional, jika kasmvnc menghasilkan file PID
    Restart=on-failure
    ```

3. Reload konfigurasi dan restart service:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart kasmvnc
    ```

4. Periksa status service:

    ```bash
    systemctl status kasmvnc
    ```

### Solusi 2: Menambahkan Script `ExecStartPost`

Jika `kasmvnc` tetap terhenti setelah dijalankan, Anda bisa menambahkan perintah tambahan di bagian `ExecStartPost` untuk memastikan bahwa service terus berjalan, atau mencoba perintah di bawah ini:

1. Edit file service lagi:

    ```bash
    sudo nano /etc/systemd/system/kasmvnc.service
    ```

2. Tambahkan perintah untuk menjaga koneksi tetap aktif:

    ```ini
    [Service]
    Type=simple
    ExecStart=/path/to/bin/kasmvnc
    ExecStartPost=/bin/sleep infinity   # Menjaga agar service tetap aktif
    Restart=on-failure
    ```

3. Reload konfigurasi dan restart service:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart kasmvnc
    ```

### Solusi 3: Menambahkan Script Shell Sederhana

Jika `kasmvnc` memerlukan loop untuk menjaga proses tetap berjalan, Anda bisa membuat script shell yang menjalankan kasmvnc dalam loop dan membuat service untuk menjalankan script tersebut.

1. Buat file script di `/usr/local/bin/kasmvnc_loop.sh`:

    ```bash
    sudo nano /usr/local/bin/kasmvnc_loop.sh
    ```

2. Tambahkan perintah ini ke dalam file script:

    ```bash
    #!/bin/bash
    while true; do
        /path/to/bin/kasmvnc
        sleep 5     # Jeda agar tidak overloading jika service terus-menerus restart
    done
    ```

3. Simpan dan beri izin eksekusi:

    ```bash
    sudo chmod +x /usr/local/bin/kasmvnc_loop.sh
    ```

4. Ubah `kasmvnc.service` agar menggunakan script baru ini:

    ```ini
    [Service]
    ExecStart=/usr/local/bin/kasmvnc_loop.sh
    Restart=always
    ```

5. Reload konfigurasi dan restart service:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart kasmvnc
    ```

### Verifikasi

Setelah membuat perubahan, restart server untuk memastikan `kasmvnc` berjalan otomatis dan tidak berhenti setelah reboot:

```bash
sudo reboot
```

Periksa kembali status setelah reboot:

```bash
systemctl status kasmvnc
```

Dengan langkah-langkah di atas, service `kasmvnc` seharusnya bisa terus berjalan otomatis setelah reboot.
