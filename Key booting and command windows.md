Android
- Recovery : Hold Volume+ & Power
- Fastboot : Hold Volume- & Power
- Download Mode Redmi 3s : 
   Hold Volume+ Volume- & Power
   
- Hardware Test : *#*#6484#*#*
   Atau : *#*#64663#*#*
   Vivo V21 5g : *#558#
- Operator Mod : *#*#4636#*#*

Windows
Safe mode F8 / dr msconfig cntang boot safe mode

Laptop Ecs
- Bios : del
- Boot Selection : F11
- Boot from Lan : F12

Laptop Toshiba Papah
- Bios : F2 klo susah fn+f2
- Boot Selection : F12
- reset bios password dll. cabut hdd & cmos kerik Jumper g3 jika ada lapisan. jumper g3 trus dinyalakan sampe nyala ada bacaan, ulangi sampai 5x

PC HP  Pavilion P6000 Series
- Bios : F10
- Boot Selection : Esc
- Boot From LAN : F12

PC SERVER IBM
- Bios : F1
- Boot Selection : F12
- Hold Boot : F10

PC SERVER HP
-Bios : F10

Laptop Asus A44
- Bios : F2 (hold f2 & Power) 
- Boot Selection : 
- Recovery :

Laptop Asus a407ub
- Bios & Boot Selection : esc (tahan esc lalu tekan power. trus tekan2 terus esc sampe masuk opsi)

Laptop Lenovo All in One
- Bios : F1
- Boot Selection : F12
- Recovery :


PC Acer Aspire Buildup 
- Bios : F2 
- Boot Selection : F11
- Recovery

PC Acer Veriton Buildup 
- Bios : Del
- Boot Selection : F12
- Recovery


Laptop Acer Aspire
- Bios : F2 
- Boot Selection : F12
- Recovery

PC OptiPlex 390 Buildup 
- Bios : F2 
- Boot Selection : F12
- Recovery

PC All in one HP Pavilion 20 (pa rahmat)
- Boot menu F9
- Bios F10
setting bios ide, legacy enable, secure boot disable, boot pilih legacy, dan pilih boot hdd sata 0. dan hdd pke mbr. utk gpt gagal mulu blm ktmu.



God Mod Full Control Panel Windows Shortcut
make folder with name 
{ED7BA470-8E54-465E-825C-99712043E01C}

Check Licensi Windows via cmd
slmgr -dlv

Check versi windows
winver


lokasi path update hapus jika error
C:\Windows\SoftwareDistribution\Download


repair login screen / fix system yg error via cmd
sfc /scannow

cmd shortcut app config
msconfig
gpedit.msc
regedit
services.msc


cek running port via cmd
netstat -aon

apache port 80 ga bisa running matikan service
World Wide Web Publishing


utk buat / gabung partisi hdd dr windows
shrink : utk pecah partisi
extend : utk gabung partisi


Tweaking Windows 10
Matikan service
- Background Inteligent Transfer Service
- SysMain
- Windows Update

- Setting visual effect hanya show thumbnail instead of icon & smooth edge of screen font
- Matikan Semua Background App
- restart pc


layar berkedip
- cek driver vga
- matiin service dari system configuration
- windows error reporting
- problem reports control panel
- cmd administrator sfc /scannow
- restart pc

fix / repair startup
bootrec /rebuildbcd
bootrec /fixmbr
bootrec /fixboot
bootrec /scanos
Turn off
Dan  chkdsk /f /r c:
Exit restart
Cd \windows\system32\config
md backup
copy *.* backup
cd regback
dir
copy *.* .. 
pilih all 
Matikan recovery
bcdedit /set {default} recoveryenable No
exit
sfc /scannow
Exit continue masuk ke windows
Jika blm bisa proses lg start up repair dr cd windows


cek psu komputer
- jumper kabel hijau & hitam/atau kabel ground
- posisi tonjolan & lubang colok diatas, warna hijau urutan ke empat dr kanan
- posisi tonjolan & lubang colok diatas, warna hitam urutan ke lima dr kanan

atau jumper dengan
- posisi tonjolan & lubang colok dibawah, warna hitam/merah urutan ke lima dr kiri

- cek voltase psu kabel kuning&hitam/ground di 12v
- cek voltase psu kabel merah&hitam/ground di 5v
- cek voltase psu kabel orange di 20pin&hitam/ground di 3.3v


membuat startup vmware,virtualbox dll
"path aplikasi.exe" start "path file vmwarenya misalkan mikrotik.wmx"

save jd cmd dan taro di startup dgn cara panggil run shell:startup

scan & repair disk termasuk bad sector
chkdsk /x /f /v /r drive:
contoh chkdsk /x /f /v /r d:

setting long path pada nama file windows 10
via regedit
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem
LongPathsEnabled Value data 1 Hexadecimal. ok & restart

via group policy gpedit.msc
Computer Configuration > Administrative Templates > System > Filesystem
Enable win32 long paths

Cara cek Versi TPM Lewat RUN
tpm.msc


Bypass secureboot & tpm saat install windows 11
saat tdk dpt install close dlu sampai ada tombol install kemudian :
tekan shift+f10 dan ketik regedit
tambah key di HKEY_LOCAL_MACHINE\System\Setup 
klik Kanan disetup new key LabConfig -> 
New DWord 32bit value -> BypassSecureBootCheck -> value data 00000001
New DWord 32bit value -> 
BypassTPMCheck -> value data 00000001
Close dan install lagi


Win 10 virus remover tool
Search di menu MRT.exe
Atau via cmd mrt /? Utk lihat command
mrt / full utk scan total

Hapus folder yg tidak bisa dihapus
via cmd 
rd /s "\\?\isikanPathFoldernya


Enable Disable Onedrive folder
Via regedit find
System.IsPinnedToNameSpaceTree = change value 0/1

Bypass login online jd offline win11
Saat tampilan lets connect
Tekan shift + F10
cmd ketik OOBE\BYPASSNRO
restart dan pilih i don't have internet 

Atau asal masukkan email dan password nnti bisa offline login
Ex. Email a@a.com password asal

Mengatasi stuck install windows di just a moment
Tahan tombol windows+x dan power. Sampai mati kemudian nyalakan lagi

Cara Cek Ram CMD
systeminfo | findstr /C:"Total Physical Memory"
