# Jarkom-Modul-2-IT14-2024

### Anggota Kelompok
| Nama | NRP | 
|---|---|
| Muhammad Faqih Husain | 5027231023 | 
| Chelsea Vania Hariyono | 5027231003 | 

### Topologi
Topologi 2
![image](https://raw.githubusercontent.com/fqhhusain/Jarkom-Modul-2-IT14-2024/refs/heads/main/Topologi.png)

### inisialisasi
Prefix ip `192.240`
#### Script bashrc
Nusantara
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.240.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Majapahit, Sriwijaya, Solok
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```

Sebuah kerajaan besar di Indonesia sedang mengalami pertempuran dengan penjajah. Kerajaan tersebut adalah Sriwijaya. Karena merasa terdesak Sriwijaya meminta bantuan pada Majapahit untuk mempertahankan wilayahnya. Pertempuran besar tersebut berada di Nusantara

1. Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

- Nusantara (router)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.240.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.240.2.1
	netmask 255.255.255.0
	
auto eth3
iface eth3 inet static
	address 192.240.3.1
	netmask 255.255.255.0
```
- Tanjungkulai(web server)
```
auto eth0
iface eth0 inet static
	address 192.240.2.4
	netmask 255.255.255.0
	gateway 192.240.2.1
```
- Bedahulu (web server)
```
auto eth0
iface eth0 inet static
	address 192.240.2.2
	netmask 255.255.255.0
	gateway 192.240.2.1
```
- sriwijaya (DNS Master)
```
auto eth0
iface eth0 inet static
	address 192.240.3.2
	netmask 255.255.255.0
	gateway 192.240.3.1
```
- Majapahit (DNS Slave)
```
auto eth0
iface eth0 inet static
	address 192.240.1.2
	netmask 255.255.255.0
	gateway 192.240.1.1
```
- Solok
```
auto eth0
iface eth0 inet static
	address 192.240.1.4
	netmask 255.255.255.0
	gateway 192.240.1.1
```
2. Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

#### Setup DNS Sriwijaya
Update package lists
```
apt-get update -y
```
Install bind9
```
apt-get install bind9 -y
```
```
echo 'zone "sudarsana.it14.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/sudarsana.it14.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it14.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it14.com. root.sudarsana.it14.com. (
                        2024100101      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it14.com.
@       IN      A       192.240.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it14.com.' > /etc/bind/jarkom/sudarsana.it14.com

service bind9 restart
```



