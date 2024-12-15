# CLOUD SERVER-FINAL-PROJECT-OS-SERVER-SYSTEM-ADMIN-23.83.1012

NAMA : ZHEWA AL VARIZHY MANEVA

NIM : 23.83.1012

# Nextcloud Server Setup

Untuk spesifikasi server yang saya gunakan, saya memberikan alokasi RAM sebesar 4GB dengan storage 25GB, kenapa saya memakai 25GB karna cloud server ini masih berbasis local, serta prosesor yang diberikan sebesar 3 core. Dalam Repository ini, saya akan menggunakan beberapa layanan server yang terdiri dari :

## Layanan Server
1.SSH

2.web server (apache)

3.database server (mariadb,) 

## 1. Instalasi dan Konfigurasi SSH Server

### 1.1 Instalasi SSH
**Langkah 1: Lakukan Instalasi Paket SSH Server**

```
apt-get install openssh-server
```
### 1.2 Konfigurasi SSH
**Langkah 1: Buka Direktori konfigurasi ssh dengan text editor(disini saya menggunakan nano)**
```
nano /etc/ssh/sshd_config
```
**Langkah 2: Edit Konfigurasi seperti dibawah ini**
```
Include /etc/ssh/sshd_config.d/*.conf

#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
#PermitRootLogin prohibit-password
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

#PubkeyAuthentication yes

# Expect .ssh/authorized_keys2 to be disregarded by default in future.
#AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody
```

**Langkah 3: Restart layanan SSH Server**
```
systemctl restart sshd
```
## 2. Instalasi dan Konfigurasi Web Server
Saya memutuskan untuk menggunakan Apache2 sebagai server web utama dalam proyek saya, meskipun tidak ada alasan khusus untuk memilihnya dibandingkan Nginx. Anda bebas memilih Nginx jika lebih sesuai dengan kebutuhan Anda. Selain itu karena Apache2 menawarkan dukungan yang baik untuk PHP.

### 2.1 Instalasi Apache2

**Langkah 1: Instalasi Paket Apache2**
```
apt-get install apache2
```
**Langkah 2: enable Paket Apache2**
```
sudo systemctl enable apache2
```
**Lengkah 3: Restart Layanan Apache2**
```
systemctl restart apache2
```
**Langkah 4: Cek Apache2**

Jika Konfigurasi Berhasil seharusnya muncul layanan web default seperti gambar dibawah ini:
![Screenshot 2024-12-15 193449](https://github.com/user-attachments/assets/3a03ba14-1152-41f4-a852-6595ce0a34e9)

## 3. Instalasi dan Konfigurasi Database Server
Nextcloud memerlukan database untuk menjalankan programnya, maka dari itu kita akan melakukan installasi Mariadb database.
### 3.1 Instalasi MariaDB

**Langkah 1: Instalasi Paket MariaDB**
```
sudo apt-get install mariadb-server
```
**Langkah 2: enable Paket MariaDB**
```
sudo systemctl enable mariadb
```
### 3.2 Konfigurasi MariaDB
**Langkah 1: Jalankan Perintah ini**
```
mysql_secure_installation
```
**Langkah 2: Ikuti Konfigurasi dibawah ini**

Anda Bisa Mengikuti Langkah-Langkah ini:
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

### 3.3 Instalasi dan Konfigurasi Phpmyadmin

**Langkah 1: Lakukan instalasi paket**
```
sudo apt install php php8.3-common php8.3-cli php8.3-mbstring php8.3-imap php8.3-redis php8.3-snmp php8.3-xml php8.3-zip php8.3-curl php8.3-mysql php8.3-gd
```
**Langkah 2: Konfigurasi Phpmyadmin**

1. lalu masukan code :
```
cd /var/www/html/
```
nanti akan muncul seperti ini:

![Screenshot 2024-12-16 002234](https://github.com/user-attachments/assets/8228d53a-4744-4e27-8e6a-7b0744373557)

2. buat 1 file
```
sudo vi info.php
```

3. lalu Isi file tersebut dengan:
```
<?php
phpinfo();
?>
```

 ### 3.4 Menguji Konfigurasi 

berhasil dibuka melalui Webserver Apache2 contohnya :
![Screenshot 2024-12-15 194900](https://github.com/user-attachments/assets/c7a1c092-8bac-4c38-9c73-6c585eefd81b)













