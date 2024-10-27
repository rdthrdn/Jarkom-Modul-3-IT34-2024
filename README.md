# Jarkom-Modul-3-IT34-2024
| Nama | NRP |
|---|---|
|Raditya Hardian Santoso|5027231033|
|Rafi Afnaan Fathurrahman|5027231040|

## Pendahuluan

Pulau Paradis dan Marley, sama-sama menghadapi ancaman besar dari satu sama lain. Keduanya membangun infrastruktur pertahanan yang kuat untuk melindungi sistem vital mereka. Dengan strategi yang matang, mereka bersiap menghadapi serangan dan mempertahankan wilayah masing-masing.

**Bangsa Marley,** dipimpin oleh **Zeke,** telah mempersiapkan **Annie, Bertholdt, dan Reiner** untuk menyerang menggunakan **Laravel Worker.** Di sisi lain, **Klan Eldia** dari **Paradis** telah mempersiapkan **Armin, Eren, dan Mikasa** sebagai **PHP Worker** untuk mempertahankan pulau tersebut. **Warhammer** bertindak sebagai **Database Server**, sementara **Beast** dan **Colossal** sebagai **Load Balancer.** 

## Topologi

![image](https://github.com/user-attachments/assets/eb1ea1b9-f45e-4bef-99cb-8ebb933d8033)


## Tabel IP

| **Node**         | **Node Type**       | **Interface** | **IP Address** | **Gateway**     |
|------------------|---------------------|---------------|----------------|-----------------|
| **Paradis**      | Router/DHCP Relay   | eth0          | DHCP           | -               |
|                  |                     | eth1          | 10.80.1.1    | -               |
|                  |                     | eth2          | 10.80.2.1	   | -               |
|                  |                     | eth3          |10.80.3.1  | -               |
|                  |                     | eth4          | 10.80.4.1    | -               |
| **Tybur**        | DHCP Server         | eth0          | 10.80.4.3   | 10.80.4.1     |
| **Fritz**        | DNS Server          | eth0          | 10.80.4.2    | 10.80.4.1     |
| **Warhammer**    | Database Server     | eth0          | 10.80.3.2    |10.80.3.1    |
| **Beast**        | Load Balancer Laravel | eth0        | 	10.80.3.3   | 10.80.3.1     |
| **Colossal**     | Load Balancer PHP   | eth0          | 10.80.3.4   |10.80.3.1     |
| **Annie**        | Laravel Worker      | eth0          | 	10.80.1.2  | 10.80.1.1     |
| **Bertholdt**    | Laravel Worker      | eth0          | 	10.80.1.3    | 10.80.1.1     |
| **Reiner**       | Laravel Worker      | eth0          | 10.80.1.4    | 10.80.1.1     |
| **Armin**        | PHP Worker          | eth0          | 10.80.2.2    | 10.80.2.1     |
| **Eren**         | PHP Worker          | eth0          | 10.80.2.3   | 10.80.2.1     |
| **Mikasa**       | PHP Worker          | eth0          | 10.80.2.4   | 10.80.2.1     |
| **Zeke**         | Client              | eth0          | DHCP           | -               |
| **Erwin**        | Client              | eth0          | DHCP           | -               |\

## Konfigurasi IP

### **Paradis (Router/DHCP Relay)**
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 10.80.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 10.80.2.1
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 10.80.3.1
    netmask 255.255.255.0

auto eth4
iface eth4 inet static
    address 10.80.4.1
    netmask 255.255.255.0
```

### **Tybur (DHCP Server)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.4.3
    netmask 255.255.255.0
    gateway 10.80.4.1
```

### **Fritz (DNS Server)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.4.2
    netmask 255.255.255.0
    gateway 10.80.4.1
```

### **Warhammer (Database Server)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.4
    netmask 255.255.255.0
    gateway 10.80.3.1
```

### **Beast (Load Balancer Laravel)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.2
    netmask 255.255.255.0
    gateway 10.80.3.1
```

### **Colossal (Load Balancer PHP)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.3
    netmask 255.255.255.0
    gateway 10.80.3.1
```

### **Annie (Laravel Worker)**
```bash
auto eth0
iface eth0 inet static
	address 10.80.1.2
	netmask 255.255.255.0
	gateway 10.80.1.1
```

### **Bertholdt (Laravel Worker)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.1.3
    netmask 255.255.255.0
    gateway 10.80.1.1
```

### **Reiner (Laravel Worker)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.1.4
    netmask 255.255.255.0
    gateway 10.80.1.1
```

### **Armin (PHP Worker)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.2.2
    netmask 255.255.255.0
    gateway 10.80.2.1
```

### **Eren (PHP Worker)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.2.3
    netmask 255.255.255.0
    gateway 10.80.2.1
```

### **Mikasa (PHP Worker)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.2.4
    netmask 255.255.255.0
    gateway 10.80.2.1
```

### **Zeke (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hwaddress ether 7a:47:21:fc:07:a4
```

### **Erwin (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hwaddress ether ba:89:d6:0f:57:f8
```

## Script Awal

### Paradis (DHCP Relay)
```sh
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.80.0.0/16
apt-get update
apt install isc-dhcp-relay -y
```

### Fritz (DNS Server)
```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```

### Tybur (DHCP Server)
```sh
echo 'nameserver 10.80.4.2' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```

### Warhammer (Database Server)
```sh
echo 'nameserver 10.80.4.2' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y

service mysql start
```

### Laravel Worker
```sh
echo 'nameserver 10.80.4.2' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y
```
### PHP Worker
```sh
echo 'nameserver 10.80.4.2' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install nginx -y
apt install software-properties-common -y
apt install php7.3 -y
apt install php7.3-fpm -y
```

### Load Balancer PHP
```sh
echo 'nameserver 10.80.4.2' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start
```

### Client
```
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

## Soal 0

Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name **marley.yyy.com** untuk worker Laravel mengarah pada **Annie**. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name **eldia.yyy.com** untuk worker PHP (0) mengarah pada **Armin**.

### Konfigurasi pada Fritz (DNS Server)

```sh
echo 'zone "marley.it34.com" { 
        type master; 
        file "/etc/bind/marley/marley.it34.com";
};

zone "eldia.it34.com" {
        type master;
        file "/etc/bind/eldia/eldia.it34.com";
}; ' >> /etc/bind/named.conf.local

mkdir /etc/bind/marley
mkdir /etc/bind/eldia

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it34.com. root.marley.it34.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it34.com.
@       IN      A       10.80.1.2     ; IP Annie' > /etc/bind/marley/marley.it34.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it34.com. root.eldia.it34.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.16.com.
@       IN      A       10.80.2.2     ; IP Armin' > /etc/bind/eldia/eldia.it34.com

service bind9 restart
```

## Soal 1
Yang diaplikasikan di konfigurasi pada bagian Topologi, Tabel IP, dan Konfigurasi IP.

## Soal 2
Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100
### Konfigurasi pada Tybur (DHCP Server)
```sh
echo '
subnet 10.80.1.0 netmask 255.255.255.0 {
range 10.80.1.5 10.80.1.25;
range 10.80.1.50 10.80.1.100;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
## Soal 3

Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243

### Konfigurasi pada Tybur (DHCP Server)

```sh
echo '
subnet 10.80.1.0 netmask 255.255.255.0 {
	range 10.80.1.5 10.80.1.25;
	range 10.80.1.50 10.80.1.100;
}

subnet 10.80.2.0 netmask 255.255.255.0 {
	range 10.80.2.09 10.80.2.27;
	range 10.80.2.81 10.80.2.243;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

## Soal 4

Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut 

### Konfigurasi pada Paradis (DHCP Relay)
```sh
echo '
SERVERS="10.80.4.2"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

### Konfigurasi pada Tybur (DHCP Server)
```sh
echo '
SERVERS="10.80.4.2"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

### Konfigurasi pada Tybur (DHCP Server)
```sh
echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
subnet 10.80.1.0 netmask 255.255.255.0 {
	range 10.80.1.05 10.80.1.25;
	range 10.80.1.50 10.80.1.100;
	option routers 10.80.1.1;
	option broadcast-address 10.80.1.255;
	option domain-name-servers 10.80.4.3;
}

subnet 10.80.2.0 netmask 255.255.255.0 {
	range 10.80.2.09 10.80.2.27;
	range 10.80.2.81 10.80.2.243;
	option routers 10.80.2.1;
	option broadcast-address 10.80.1.255;
	option domain-name-servers 10.80.4.3;
}

subnet 10.80.3.0 netmask 255.255.255.0 {
	option routers 10.80.3.1;
}

subnet 10.80.4.0 netmask 255.255.255.0 {
	option routers 10.80.4.1;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
## Soal 5

Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit.

### Konfigurasi pada Tybur (DHCP Server)
```sh
echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
subnet 10.80.1.0 netmask 255.255.255.0 {
	range 10.80.1.5 10.80.1.25;
	range 10.80.1.50 10.80.1.100;
	option routers 10.80.1.1;
	option broadcast-address 10.80.1.255;
	option domain-name-servers 10.80.4.3;
	default-lease-time 360;
	max-lease-time 5220;
}

subnet 10.80.2.0 netmask 255.255.255.0 {
	range 10.80.2.9 10.80.2.27;
	range 10.80.2.81 10.80.2.243;
	option routers 10.80.2.1;
	option broadcast-address 10.80.1.255;
	option domain-name-servers 10.80.4.3;
	default-lease-time 1800;
	max-lease-time 5220;
}

subnet 10.80.3.0 netmask 255.255.255.0 {
	option routers 10.80.3.1;
}

subnet 10.80.4.0 netmask 255.255.255.0 {
	option routers 10.80.4.1;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

## Soal 6

Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 

### Konfigurasi pada PHP Worker

```sh
service nginx start
service php7.3-fpm start

mkdir -p /var/www/eldia.it34.com

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1TvebIeMQjRjFURKVtA32lO9aL7U2msd6' -O /root/bangsaEldia.zip
unzip -o /root/bangsaEldia.zip -d /var/www/eldia.it34.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/eldia.it34.com
ln -s /etc/nginx/sites-available/eldia.it34.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo '
server {
  listen 80;
  listen [::]:80;

  root /var/www/eldia.it34.com;
  index index.php index.html index.htm;

  server_name eldia.it34.com;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
  }

  location ~ /\.ht {
    deny all;
  }
}' > /etc/nginx/sites-available/eldia.it34.com

service nginx restart
```
## Soal 7

Dikarenakan Armin sudah mendapatkan kekuatan titan colossal, maka bantulah kaum eldia menggunakan colossal agar dapat bekerja sama dengan baik. Kemudian lakukan testing dengan 6000 request dan 200 request/second. 

### Konfigurasi pada Colossal (Load Balancer PHP)
```sh
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
    server 10.80.2.2;
    server 10.80.2.3;
    server 10.80.2.4;
}

server {
    listen 80;
    server_name eldia.it34.com www.eldia.it34.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

### Konfigurasi pada Fritz (DNS Server)

Ubah untuk mengarahkan eldia.it34.com ke IP Colossal (Load Balancer PHP)

```sh
echo 'zone "marley.it34.com" {
    type master;
    file "/etc/bind/sites/marley.it34.com";
};
zone "eldia.it34.com" {
    type master;
    file "/etc/bind/sites/eldia.it34.com";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/marley.it34.com
cp /etc/bind/db.local /etc/bind/sites/eldia.it34.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it34.com. root.marley.it34.com. (
                        2024102301      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it34.com.
@       IN      A       10.80.1.2    ; IP Annie
www     IN      CNAME   marley.it34.com.' > /etc/bind/sites/marley.it34.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it34.com. root.eldia.it34.com. (
                            2024102301         ; Serial
                            604800              ; Refresh
                            86400              ; Retry
                            2419200              ; Expire
                            604800 )            ; Negative Cache TTL
;
@       IN      NS      eldia.it34.com.
@       IN      A       10.80.3.4    ; IP Colossal
www     IN      CNAME   eldia.it34.com.' > /etc/bind/sites/eldia.it34.com

echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

## Soal 10
Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di Colossal dengan dengan kombinasi username: “arminannie” dan password: “jrkmyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/
### Konfigurasi pada Colossal (Load Balancer PHP)
```sh
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit34

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
    server 10.80.2.2;
    server 10.80.2.3;
    server 10.80.2.4;
}

server {
    listen 80;
    server_name eldia.it34.com www.eldia.it34.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

## Soal 11
Lalu buat untuk setiap request yang mengandung /titan akan di proxy passing menuju halaman https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki (11) 
hint: (proxy_pass)
### Konfigurasi pada Colossal (Load Balancer PHP)
```sh
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit34

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
    server 10.80.2.2;
    server 10.80.2.3;
    server 10.80.2.4;
}

server {
    listen 80;
    server_name eldia.it34.com www.eldia.it34.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

## Soal 12
Selanjutnya Colossal ini hanya boleh diakses oleh client dengan IP [Prefix IP].1.77, [Prefix IP].1.88, [Prefix IP].2.144, dan [Prefix IP].2.156. 
hint: (fixed in dulu clientnya)
### Konfigurasi pada Colossal (Load Balancer PHP)
```sh
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit34

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #least_conn;
        #ip_hash;
    server 10.80.2.2;
    server 10.80.2.3;
    server 10.80.2.4;
}

server {
    listen 80;
    server_name eldia.it34.com www.eldia.it34.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        allow 10.80.1.77;
        allow 10.80.1.88;
        allow 10.80.2.144;
        allow 10.80.2.156;
        deny all;
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

## Soal 13
Karena mengetahui bahwa ada keturunan marley yang mewarisi kekuatan titan, Zeke pun berinisiatif untuk menyimpan data data penting di Warhammer, dan semua data tersebut harus dapat diakses oleh anak buah kesayangannya, Annie, Reiner, dan Berthold.
### Konfigurasi pada Warhammer (Database)
```sh
apt-get update
apt-get install mariadb-server -y
service mysql start

echo '# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf 

sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf

service mysql restart
```
### Jalankan di Database
```sh
mysql -u root -p
Enter password: (kosong agar mudah)

CREATE USER 'it34'@'%' IDENTIFIED BY 'it34';
CREATE USER 'it34'@'localhost' IDENTIFIED BY 'it34';
CREATE DATABASE db_it34;
GRANT ALL PRIVILEGES ON *.* TO 'it34'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'it34'@'localhost';
FLUSH PRIVILEGES;
```
