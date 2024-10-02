# Jarkom-Modul-2-IT14-2024

### Anggota Kelompok
| Nama | NRP | 
|---|---|
| Muhammad Faqih Husain | 5027231023 | 
| Chelsea Vania Hariyono | 5027231003 | 

### Topologi
Topologi no.2
![image](https://github.com/user-attachments/assets/444e42a4-14a0-4526-b5ae-55f1b948a3e7)



### Inisialisasi
Prefix ip `192.240`
#### Script bashrc
Nusantara
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.240.0.0/16
echo nameserver 192.240.3.2 > /etc/resolv.conf
```

Sriwijaya, Majapahit (DNS)
```
echo nameserver 192.240.3.2 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```

Client
```
echo -e "nameserver 192.168.122.1\nnameserver 192.240.3.2" > /etc/resolv.conf
```

Other
```
echo nameserver 192.240.3.2 > /etc/resolv.conf
```


Sebuah kerajaan besar di Indonesia sedang mengalami pertempuran dengan penjajah. Kerajaan tersebut adalah Sriwijaya. Karena merasa terdesak Sriwijaya meminta bantuan pada Majapahit untuk mempertahankan wilayahnya. Pertempuran besar tersebut berada di Nusantara

#### 1. Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

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
- Majapahit (DNS Slave)
```
auto eth0
iface eth0 inet static
	address 192.240.1.2
	netmask 255.255.255.0
	gateway 192.240.1.1
```
- HayamWuruk (client)
```
auto eth0
iface eth0 inet static
	address 192.240.1.3
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
- Srikandi (client)
```
auto eth0
iface eth0 inet static
	address 192.240.1.5
	netmask 255.255.255.0
	gateway 192.240.1.1
```
- Sriwijaya (DNS Master)
```
auto eth0
iface eth0 inet static
	address 192.240.3.2
	netmask 255.255.255.0
	gateway 192.240.3.1
```
- AlbertEinstein (client)
```
auto eth0
iface eth0 inet static
	address 192.240.2.5
	netmask 255.255.255.0
	gateway 192.240.2.1
```
- Tanjungkulai (web server)
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
- Kotalingga
```
auto eth0
iface eth0 inet static
	address 192.240.2.3
	netmask 255.255.255.0
	gateway 192.240.2.1
```

#### 2. Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

#### Setup DNS Sriwijaya
Update package lists
```
apt-get update -y
```
Install bind9
```
apt-get install bind9 -y
```
script.sh
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
@       IN      A       192.240.1.4    ;
@       IN      AAAA    ::1' > /etc/bind/jarkom/sudarsana.it14.com

service bind9 restart
```
3. Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

```
echo 'zone "pasopati.it14.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it14.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it14.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it14.com. root.pasopati.it14.com. (
                        2024100101      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it14.com.
@       IN      A       192.240.2.3    ;
@       IN      AAAA    ::1' > /etc/bind/jarkom/pasopati.it14.com

service bind9 restart
```

4. Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

```
echo 'zone "rujapala.it14.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it14.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it14.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it14.com. root.rujapala.it14.com. (
                        2024100101      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it14.com.
@       IN      A       192.240.2.4    ;
@       IN      AAAA    ::1' > /etc/bind/jarkom/rujapala.it14.com

service bind9 restart
```
5. Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

