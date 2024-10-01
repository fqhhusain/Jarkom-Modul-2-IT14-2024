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

Majapahit, Sriwijaya
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
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


