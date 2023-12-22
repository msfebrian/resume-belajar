# config pve-firewall proxmox
dasar command
```
pve-firewall add rule <VM_ID> allow tcp --source <IP_ADDRESS> --dest <VM_IP_ADDRESS> --dest-port 22
```

Ganti <VM_ID> dengan ID VM yang ingin Anda akses, <IP_ADDRESS> dengan alamat IP Anda dari luar yang akan diizinkan, dan <VM_IP_ADDRESS> dengan alamat IP VM yang terkait.

# contoh mengijinkan port 22 diakses dari mana saja

```
pve-firewall add rule all allow tcp --dport 22
```

# contoh lainnya
```
pve-firewall add rule 100 allow tcp --source 192.168.88.0/24 --dest 0.0.0.0/0 --dest-port 22

```
Penjelasan dari perintah di atas:

--source 192.168.88.0/24 memungkinkan akses dari jaringan dengan alamat IP dalam rentang 192.168.88.1 hingga 192.168.88.254.
--dest 0.0.0.0/0 mengizinkan akses ke semua alamat IP VM yang ada pada Proxmox (VM dengan alamat IP apa pun).
--dest-port 22 mengizinkan akses hanya ke port 22 (port SSH).
Perintah tersebut akan menambahkan aturan firewall dengan nomor aturan 100 yang memungkinkan akses SSH dari jaringan 192.168.88.0/24 ke semua VM di Proxmox pada port 22.