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
| **Tybur**        | DHCP Server         | eth0          | 10.80.4.2   | 10.80.4.1     |
| **Fritz**        | DNS Server          | eth0          | 10.80.4.3    | 10.80.4.1     |
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
    address 10.80.4.2
    netmask 255.255.255.0
    gateway 10.80.4.1
```

### **Fritz (DNS Server)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.4.3
    netmask 255.255.255.0
    gateway 10.80.4.1
```

### **Warhammer (Database Server)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.2
    netmask 255.255.255.0
    gateway 10.80.3.1
```

### **Beast (Load Balancer Laravel)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.3
    netmask 255.255.255.0
    gateway 10.80.3.1
```

### **Colossal (Load Balancer PHP)**
```bash
auto eth0
iface eth0 inet static
    address 10.80.3.4
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
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```

### Fritz (DNS Server)
```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y  
```

### Tybur (DHCP Server)
```sh
echo 'nameserver 10.80.4.3' > /etc/resolv.conf   # DNS Server 
apt-get update
apt install isc-dhcp-server -y
```

### Warhammer (Database Server)
```sh
echo 'nameserver 10.80.4.3' > /etc/resolv.conf   # DNS Server 
apt-get update
apt-get install mariadb-server -y
service mysql start
```

### Laravel Worker
```sh
echo 'nameserver 10.80.4.3' > /etc/resolv.conf   # DNS Server 
apt-get update
apt-get install mariadb-client -y
```
### PHP Worker
```sh
echo 'nameserver 10.80.4.3' > /etc/resolv.conf   # DNS Server 
apt-get update
apt-get install nginx -y
apt-get install wget unzip -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y
```

### Load Balancer PHP
```sh
echo 'nameserver 10.80.4.3' > /etc/resolv.conf   # DNS Server 
apt-get update
apt-get install nginx -y
```

### Client
```
apt-get update
```

## Soal 0

> Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name **marley.yyy.com** untuk worker Laravel mengarah pada **Annie**. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name **eldia.yyy.com** untuk worker PHP (0) mengarah pada **Armin**.

### Konfigurasi pada Fritz (DNS Server)

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
@       IN      NS      marley.it24.com.
@       IN      A       10.80.1.2    ; IP Annie
www     IN      CNAME   marley.it24.com.' > /etc/bind/sites/marley.it34.com

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
@       IN      A       10.80.2.2    ; IP Armin
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


