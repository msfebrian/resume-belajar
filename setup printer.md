# SETUP SEBAGAI PRINT SERVER (SUPPORT ARMBIAN SERVER)
1. pada server linux pastikan ip sudah jadi statik
2. Update Repository
```
apt update
```  
3. Install package CUPS
```
apt install cups
```
4. cek status
```
systemctl status
```
5. configurasi cups
```
nano /etc/cups/cupsd.conf
```
rubah only listen ....
```
rubah Listen localhost:631 menjadi port 631
```
tambah restric access to server `Allow @LOCAL`
```
<Location />
  Order allow.deny
  Allow @LOCAL
</location>
```
tambah restric access to the admin page `Allow @LOCAL`
```
<Location /admin>
  Order allow.deny
  Allow @LOCAL
</location>
```
kemudian save dan restart servie cups
```
systemctl restart cups
systemctl status cups
```


