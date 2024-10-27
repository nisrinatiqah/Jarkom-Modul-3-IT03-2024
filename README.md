# Jarkom-Modul-3-IT03-2024

# Jarkom-Modul-3-IT25-2024

# Anggota Kelompok
| Nama  | NRP  |
|----------|----------|
| Nisrina Atikah | 5027231026 |
| Nicholas Arya  | 5027231054 |

## Pembuatan Topologi
![image](https://github.com/user-attachments/assets/46562e8e-3f48-4175-bf2a-ef6b1991c4f5)

## Konfigurasi



### Paradis (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.235.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.235.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.235.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 1192.235.4.0
	netmask 255.255.255.0

### Fritz (DNS Server)
auto eth0
iface eth0 inet static
	address 192.235.4.2
	netmask 255.255.255.0
	gateway 192.235.4.0

### Tybur (DHCP Server)
auto eth0
iface eth0 inet static
	address 192.235.4.3
	netmask 255.255.255.0
	gateway 192.235.4.0
```
### Warhammer (Database Server)
```
auto eth0
iface eth0 inet static
	address 192.235.3.4
	netmask 255.255.255.0
	gateway 192.235.3.0
```
### Beast (Load Balancer Laravel)
```
auto eth0
iface eth0 inet static
	address 192.235.3.2
	netmask 255.255.255.0
	gateway 192.235.3.0
```
### Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 192.235.3.3
	netmask 255.255.255.0
	gateway 192.235.3.0
```
### Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.1.2
	netmask 255.255.255.0
	gateway 192.235.1.0
```
### Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.1.3
	netmask 255.255.255.0
	gateway 192.235.1.0
```
### Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.1.4
	netmask 255.255.255.0
	gateway 192.235.1.0
```
### Armin (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.2.2
	netmask 255.255.255.0
	gateway 192.235.2.0
```

### Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.2.3
	netmask 255.255.255.0
	gateway 192.235.2.0
```

### Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.235.2.4
	netmask 255.255.255.0
	gateway 192.235.2.0
```

### Zeke, Erwin (Client) 
```
auto eth0
iface eth0 inet dhcp
```

## Instalasi Dependensi
### Paradis (DHCP Relay)
```
apt-get update
apt-get install isc-dhcp-relay -y
```
### Tybur (DHCP Server)
```
apt-get update
apt-get install isc-dhcp-server -y
```
### Fritz (DNS Server)
```
apt-get update
apt-get install bind9 -y
```
### Beast dan Colossal (Load Balancer)
```
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y
```
### Warhammer (Database Server)
```
apt-get update
apt-get install mariadb-server -y
```
### Annie, Bertholdt, Reiner (Laravel Worker)
```
apt-get update
apt-get install mariadb-server -y
```
### Armin, Eren, Mikasa (PHP Worker)
### Zeke dan Erwin (Client)
```
apt-get update
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install jq -y
```

## No.0
Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

### Membuat script di Fritz untuk memberikan domain marley.it25.com yang menyambung ke IP Annie, dan domain eldia.it25.com yang menyambung ke IP Armin
>Fritz/Script0.sh
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

echo "zone \"marley.it25.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it25.com\";
};

zone \"eldia.it25.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it25.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

riegel="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it25.com. root.marley.it25.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it25.com.
@       IN    A    10.76.1.2
"
echo "$riegel" > /etc/bind/jarkom/marley.it25.com

granz="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it25.com. root.eldia.it25.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it25.com.
@       IN    A    10.76.2.2
"
echo "$granz" > /etc/bind/jarkom/eldia.it25.com

service bind9 restart
```
### Test di client menggunakan IP Static Zeke (10.76.1.5) dan Erwin (10.76.2.5)
![image](https://github.com/user-attachments/assets/b856f626-e063-414e-ad47-e25b21ffb104)
![image](https://github.com/user-attachments/assets/253ca319-7736-4c1b-9690-28b8260e11a3)

## No.1-5
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Jauh sebelum perang dimulai, ternyata para keluarga bangsawan, Tybur dan Fritz, telah membuat kesepakatan sebagai berikut:
1. Semua Client harus menggunakan konfigurasi ip address dari keluarga Tybur (dhcp).
2. Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)
3. Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)
4. Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut (4)
5. Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)

### Membuat script untuk menghubungkan DHCP Relay (Paradis) dengan DHCP Server (Tybur)
>Paradis/Script1.sh
```
apt-get update
apt install isc-dhcp-relay -y

service isc-dhcp-relay start 

echo '# Defaults for isc-dhcp-relay initscript

# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.76.4.3" 

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay


echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```

>Tybur/Script1.sh
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

subnet 10.76.1.0 netmask 255.255.255.0 {
    range 10.76.1.5 10.76.1.25;
    range 10.76.1.50 10.76.1.100;
    option routers 10.76.1.1;
    option broadcast-address 10.76.1.255;
    option domain-name-servers 10.76.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.76.2.0 netmask 255.255.255.0 {
    range 10.76.2.9 10.76.2.27;
    range 10.76.2.81 10.76.2.243;
    option routers 10.76.2.1;
    option broadcast-address 10.76.2.255;
    option domain-name-servers 10.76.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.76.3.0 netmask 255.255.255.0 {
}

subnet 10.76.4.0 netmask 255.255.255.0 {
}

"
echo "$subnet" > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Test setelah client mendapat IP Dynamic dengan konfigurasi lease time yang sudah ditentukan
![image](https://github.com/user-attachments/assets/cb94d6a1-d918-4004-82c0-a6f4950f2231)
![image](https://github.com/user-attachments/assets/770a27fa-170d-4ce8-8219-77bd2a564276)

### Test ping marley.it25.com dan eldia.it25.com setelah mendapat ip dynamic
![image](https://github.com/user-attachments/assets/3dbf6b7c-1fc4-4aed-9f7b-154b44766ca4)
![image](https://github.com/user-attachments/assets/bb1a0ba2-00e5-44bb-9ac5-d74d257316d7)


