# Cara Install dan Konfigurasi MySQL di Linux Ubuntu
Installasi
```
  sudo apt install mysql-server
```
Cek Status MySQL
```
  sudo systemctl status mysql
```

Konfigurasi password
```
  sudo mysql_secure_installation
```

Enable disable service mysql
```
  sudo systemctl enable mysql
  sudo systemctl disable mysql

  sudo systemctl stop mysql
  sudo systemctl start mysql
  sudo systemctl restart mysql
```

Cek Service dengan tampil status
```
  sudo  /etc/init.d/mysql stop
  sudo  /etc/init.d/mysql start
```

Masuk tanpa password
```
  sudo mysql -u root mysql
```
atau masuk dgn password
```
  sudo mysql -u root -p
```

## Konfigurasi MySQL agar bisa diakses dari luar
```
  sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Default hanya dapat diakses dr local 
bind-address = 127.0.0.1 atau localhost 
```
  Rubah Bind Address jadi 0.0.0.0 (agar dapat diakses dari luar).
```
Utk rubah port jg dsni
```
  sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Untuk mematikan casesensitive 
```
  sudo nano /etc/mysql/my.cnf
  Dibawah [mysql] tambah baris ini
    lower_case_table_names=1
  Cek variabel
    show variables where Variable_name='lower_case_table_names';
```

kemudian restart mysql
```
  sudo systemctl restart mysql 
  atau
  sudo service mysql restart
```

## Membuat User
```
  CREATE USER 'nama_user'@'%' IDENTIFIED BY 'password';

  GRANT ALL PRIVILEGES ON *.* TO 'nama_user'@'%' WITH GRANT OPTION;
  FLUSH PRIVILEGES; (utk menerapkan / apply)
```
atau 
```
  GRANT ALL PRIVILEGES ON *.* TO 'nama_user'@'%';
  FLUSH PRIVILEGES;
```
```
gunakan '%' jika ingin dapat diakses dari manapun, jika localhost hanya dilocal
```

## Access Permission
Contoh Opsi Ijin Akses User
```
  GRANT SELECT, INSERT, UPDATE ON database_name.table_name TO 'username'@'host';
```

## Melihat Daftar User
```
  SELECT user, host FROM mysql.user;
```

## Menghapus User
```
  DROP USER 'nama_user'@'host';
  FLUSH PRIVILEGES;
```

## Ganti password user
```
   ALTER USER 'nama_user'@'host' IDENTIFIED BY 'password_baru';
   FLUSH PRIVILEGES;
```

## Contoh DDL (Data Definition Language)
```
   CREATE DATABASE new_database;
   DROP DATABASE old_database;
   ALTER TABLE table_name ADD COLUMN column_name INT;
```

## Setting firewall Ubuntu 
```
  sudo ufw allow 3306/tcp
```
