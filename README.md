# Laporan Resmi Praktikum Jarkom Modul-3 2024

---

# Anggota Kelompok
| Nama  | NRP  |
|----------|----------|
| Nisrina Atiqah Dwiputri Ridzki| 5027231075 |
| Nicholas Arya Krisnugroho Rerangin| 5027231058 |

## Topologi
![image](https://github.com/user-attachments/assets/46562e8e-3f48-4175-bf2a-ef6b1991c4f5)

## Konfigurasi

### Paradis (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.65.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.65.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.65.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.65.4.0
	netmask 255.255.255.0
```
### Fritz (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.65.4.2
	netmask 255.255.255.0
	gateway 10.65.4.0
```
### Tybur (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.65.4.3
	netmask 255.255.255.0
	gateway 10.65.4.0
```
### Warhammer (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.65.3.4
	netmask 255.255.255.0
	gateway 10.65.3.0
```
### Beast (Load Balancer Laravel)
```
auto eth0
iface eth0 inet static
	address 10.65.3.2
	netmask 255.255.255.0
	gateway 10.65.3.0
```
### Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 10.65.3.3
	netmask 255.255.255.0
	gateway 10.65.3.0
```
### Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.1.2
	netmask 255.255.255.0
	gateway 10.65.1.0
```
### Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.1.3
	netmask 255.255.255.0
	gateway 10.65.1.0
```
### Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.1.4
	netmask 255.255.255.0
	gateway 10.65.1.0
```
### Armin (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.2.2
	netmask 255.255.255.0
	gateway 10.65.2.0
```

### Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.2.3
	netmask 255.255.255.0
	gateway 10.65.2.0
```

### Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.65.2.4
	netmask 255.255.255.0
	gateway 10.65.2.0
```

### Zeke (Client) 
```
auto eth0
iface eth0 inet dhcp
```

### Erwin (Client) 
```
auto eth0
iface eth0 inet dhcp
```


## Nomor 0
Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

### Membuat script di Fritz untuk memberikan domain marley.it03.com yang menyambung ke IP Annie, dan domain eldia.it03.com yang menyambung ke IP Armin
```
apt-get update
```
```
apt-get install bind9 -y
```

>no0.sh
```
apt-get update
apt-get install bind9 -y

forward="options {
directory \"/var/cache/bind\";
forwarders {
  	   192.168.122.1;
};

allow-query{any;};
listen-on-v6 { any; };
};
"
echo "$forward" > /etc/bind/named.conf.options

echo "zone \"marley.it03.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it03.com\";
};

zone \"eldia.it03.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it03.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

riegel="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it03.com. root.marley.it03.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it03.com.
@       IN    A    10.65.1.2
"
echo "$riegel" > /etc/bind/jarkom/marley.it03.com

granz="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it03.com. root.eldia.it03.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it03.com.
@       IN    A    10.65.2.2
"
echo "$granz" > /etc/bind/jarkom/eldia.it03.com

service bind9 restart
```
### Test di client Zeke (10.65.1.5) 
![WhatsApp Image 2024-10-27 at 00 44 45_32cf43b9](https://github.com/user-attachments/assets/58be0f1b-71a7-4829-816c-fdc788c704a0)

![WhatsApp Image 2024-10-27 at 00 44 45_2197cda9](https://github.com/user-attachments/assets/b0ed1400-7b92-419c-864a-c722c3819a8c)


### Test di client Erwin (10.65.2.5)
![WhatsApp Image 2024-10-27 at 00 44 45_bf9511bd](https://github.com/user-attachments/assets/5ff8c653-cf94-406a-a2dd-b28cdf43f728)

![WhatsApp Image 2024-10-27 at 00 44 45_643062d8](https://github.com/user-attachments/assets/aa9fcb17-5149-4241-8caa-297e69a8e136)


## Nomor 1-5
Jauh sebelum perang dimulai, ternyata para keluarga bangsawan, Tybur dan Fritz, telah membuat kesepakatan sebagai berikut:
1. Semua Client harus menggunakan konfigurasi ip address dari keluarga Tybur (dhcp).
2. Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)
3. Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)
4. Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut (4)
5. Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)

### Membuat script untuk menghubungkan Paradis (DHCP Relay) dengan Tybur (DHCP Server)
Instalasi di Paradis
```
apt-get update
```
```
apt-get install isc-dhcp-relay -y
```
>paradis1.sh
```
apt-get update
apt install isc-dhcp-relay -y

service isc-dhcp-relay start 

echo '# Defaults for isc-dhcp-relay initscript

# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts


# What servers should the DHCP relay forward requests to?
SERVERS="10.65.4.3" 

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay


echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```

Instalasi di Tybur
```
apt-get update
```
```
apt-get install isc-dhcp-server -y
```
>tybur1.sh
```
apt-get update
apt-get install isc-dhcp-server -y

echo 'INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

subnet="option domain-name \"example.org\";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style-none;

subnet 10.65.1.0 netmask 255.255.255.0 {
    range 10.65.1.5 10.65.1.25;
    range 10.65.1.50 10.65.1.100;
    option routers 10.65.1.1;
    option broadcast-address 10.65.1.255;
    option domain-name-servers 10.65.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.65.2.0 netmask 255.255.255.0 {
    range 10.65.2.9 10.65.2.27;
    range 10.65.2.81 10.65.2.243;
    option routers 10.65.2.1;
    option broadcast-address 10.65.2.255;
    option domain-name-servers 10.65.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.65.3.0 netmask 255.255.255.0 {
}

subnet 10.65.4.0 netmask 255.255.255.0 {
}

"
echo "$subnet" > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Test setelah client mendapat IP Dynamic dengan konfigurasi lease time yang sudah ditentukan
(Zeke)

![image](https://github.com/user-attachments/assets/cb94d6a1-d918-4004-82c0-a6f4950f2231)

(Erwin)

![image](https://github.com/user-attachments/assets/770a27fa-170d-4ce8-8219-77bd2a564276)

### Test ping marley.it03.com dan eldia.it03.com setelah mendapat ip dynamic (di nomer 0)
Sebelum tes ping instalasi di client (Zeke dan Erwin)
```
apt-get update
```
```
apt-get install lynx -y
```
```
apt-get install htop -y
```
```
apt-get install apache2-utils -y
```
```
apt-get install jq -y
```


## Nomor 6
Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 (6)
### Buat script di Armin, Eren, Mikasa
>armin6.sh/eren6.sh/mikasa6.sh
```
#!/bin/bash
 
# Update package list and install necessary packages
apt-get update
apt-get install lynx nginx wget unzip php7.3 php-fpm -y
 
# Start PHP-FPM and Nginx services
service php7.3-fpm start
service nginx start
 
# Create download directory
mkdir -p /var/www/html/download/
 
# Download the ZIP file from Google Drive
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1yliJkxu-3XmgJ6Xb37pGc2Jht5NTO9oj' -O /var/www/html/download/bangsa-eldia.zip
 
# Unzip the downloaded file
unzip /var/www/html/download/bangsa-eldia.zip -d /var/www/html/download
 
# Move extracted files to the web root
mv /var/www/html/download/bangsa-eldia/modul-3/* /var/www/html/
 
# Clean up by removing the download directory
rm -rf /var/www/html/download/
 
# Configure Nginx
cat <<EOL > /etc/nginx/sites-available/it03.conf
server {
	listen 80;
 
	root /var/www/html;
 
	index index.php index.html index.htm;
 
	server_name _;
 
	location / {
    	try_files \$uri \$uri/ /index.php?\$query_string;
	}
 
	location ~ \.php$ {
    	include snippets/fastcgi-php.conf;
    	fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
	}
 
	error_log /var/log/nginx/it03_error.log;
	access_log /var/log/nginx/it03_access.log;
}
EOL
 
# Enable the site configuration
ln -s /etc/nginx/sites-available/it03.conf /etc/nginx/sites-enabled/
 
# Remove the default Nginx site
rm /etc/nginx/sites-enabled/default
 
# Restart Nginx and PHP-FPM services
service nginx restart
service php7.3-fpm restart
```
