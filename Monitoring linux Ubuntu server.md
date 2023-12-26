# Monitoring Hardware dan Proses pada Linux


## Lihat task manager
```
  sudo htop
```

## Melihat processor, ram, network dll dgn package glances
jika blm install
```
  sudo apt install glances
```
untuk melihat
```
  sudo glances
```

## Lihat info system operasi & hardware dgn package neofetch
jika blm install
```
  sudo apt install neofetch
```

Cara lihat
```
  sudo neofetch
```

## CPU X versi CPU Z linux
```
  sudo apt install cpu-x
```
Cara lihat
```
  sudo cpu-x --ncurses
```

## Aplikasi Lainnya yang lebih advance
Aplikasi monitoring webbase
```
   sudo apt install monitorix
```

Via the repository
```
  apt-get update
  apt-get install monitorix
```
Manually, downloading first the package and taking care for dependencies, and finally installing it.
```
  apt-get update
```
```
  apt-get install rrdtool perl libwww-perl libmailtools-perl libmime-lite-perl librrds-perl libdbi-perl libxml-simple-perl libhttp-server-simple-perl libconfig-general-perl libio-socket-ssl-perl
  dpkg -i monitorix*.deb
```

```
  apt-get -f install
```

To fine-tune your installation, take a look at the /etc/monitorix/monitorix.conf

## IMPORTANT NOTICE:
The Debian package also comes with an extra configuration file in /etc/monitorix/conf.d/00-debian.conf that includes some options specially adapted for Debian systems. This file will be loaded right after the main configuration file, hence some options in the main configuration will be overwritten by this extra file.

When you are done, restart Monitorix to let your changes take effect:
```
  service monitorix restart
```
View from browser 
http://localhost:8080/monitorix/
