# cloud-storage

NAMA : ZHEWA AL VARIZHY MANEVA
NIM : 23.83.1012

# Nextcloud Server Setup

## Layanan Server yang Dibutuhkan

Untuk mengimplementasikan Nextcloud dengan skala yang dapat mendukung performa tinggi dan pengelolaan yang baik, berikut adalah 5 layanan utama yang dibutuhkan:

1. **Server Database**
   - Fungsi: Menyimpan data aplikasi seperti pengguna, file metadata, dan pengaturan.
   - Rekomendasi: MariaDB atau PostgreSQL.

2. **Server Aplikasi (Application Server)**
   - Fungsi: Menjalankan Nextcloud sebagai aplikasi PHP.
   - Menggunakan PHP dan web server seperti Apache atau Nginx.

3. **Server File Storage**
   - Fungsi: Menyimpan data file yang diunggah oleh pengguna.
   - Dapat menggunakan NFS (Network File System), S3 Compatible Storage, atau penyimpanan lokal.

4. **Server Proxy/Load Balancer**
   - Fungsi: Mengatur lalu lintas antara pengguna dan backend aplikasi.
   - Menggunakan HAProxy atau Nginx sebagai load balancer.

5. **Server Redis (Optional)**
   - Fungsi: Untuk caching, mempercepat akses data metadata, dan menangani file locking.
   - Menggunakan Redis sebagai layanan caching.

---

## Spesifikasi Server

### a. Server Database
- **CPU**: 4 core (minimum)
- **RAM**: 8 GB
- **Storage**: SSD 100 GB (dengan RAID jika memungkinkan)
- **Network**: 1 Gbps
- **OS**: Ubuntu Server 20.04 atau Debian 11

### b. Server Aplikasi
- **CPU**: 4–8 core
- **RAM**: 16 GB
- **Storage**: SSD 50 GB untuk aplikasi dan log
- **Network**: 1 Gbps
- **OS**: Ubuntu Server 20.04 atau Debian 11
- **Software**: PHP 8.1, Apache/Nginx, Nextcloud

### c. Server File Storage
- **CPU**: 2 core
- **RAM**: 8 GB
- **Storage**: 1 TB (SSD atau HDD sesuai kebutuhan)
- **Network**: 1 Gbps
- **OS**: Ubuntu Server 20.04 atau CentOS 7/8
- **Software**: NFS, MinIO (S3-compatible), atau GlusterFS

### d. Server Proxy/Load Balancer
- **CPU**: 2 core
- **RAM**: 4 GB
- **Storage**: SSD 20 GB
- **Network**: 1 Gbps
- **OS**: Ubuntu Server 20.04 atau Debian 11
- **Software**: HAProxy atau Nginx

### e. Server Redis
- **CPU**: 2 core
- **RAM**: 4 GB
- **Storage**: SSD 10 GB
- **Network**: 1 Gbps
- **OS**: Ubuntu Server 20.04 atau Debian 11
- **Software**: Redis

---

## Langkah Instalasi

### 1. Siapkan Infrastruktur
Pastikan semua server sudah terinstal sistem operasi yang direkomendasikan.  
Update semua server:
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Instal Database di Server Database
Instal MariaDB atau PostgreSQL:

```bash
sudo apt install mariadb-server -y
```
Buat database dan user untuk Nextcloud:

```sql
CREATE DATABASE nextcloud;
CREATE USER 'zewa'@'varizi' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'%';
FLUSH PRIVILEGES;
```
### 3. Konfigurasi Server Aplikasi
Instal Apache, PHP, dan modulnya:

```bash
sudo apt install apache2 php libapache2-mod-php php-mysql php-redis php-xml php-curl php-zip php-mbstring unzip -y
```
Unduh dan ekstrak Nextcloud:

```bash
wget https://download.nextcloud.com/server/releases/latest.zip
unzip latest.zip -d /var/www/
sudo chown -R www-data:www-data /var/www/nextcloud
```
Konfigurasi Virtual Host untuk Nextcloud di Apache/Nginx.

### 4. Konfigurasi Penyimpanan File
Siapkan NFS atau penyimpanan S3 sesuai kebutuhan dan mount pada path yang diinginkan (/mnt/data):

```bash
sudo mount <storage-path> /mnt/data
sudo chown -R www-data:www-data /mnt/data
```

###5. Instal Redis
Instal Redis di server Redis:

```bash
sudo apt install redis-server -y
```
### 6. Konfigurasi Load Balancer
Instal Nginx di server load balancer:

```bash
sudo apt install nginx -y
```
Tambahkan konfigurasi berikut untuk meneruskan permintaan ke server aplikasi:

```nginx
upstream nextcloud_app {
    server <APPLICATION_SERVER_IP>:80;
}

server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://nextcloud_app;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Aktifkan konfigurasi dan restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/nextcloud /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```
### 7. Finalisasi Konfigurasi Nextcloud
Akses antarmuka web Nextcloud:

```arduino
http://<APPLICATION_SERVER_IP>
```
Masukkan database server, storage path, dan pengaturan Redis.





