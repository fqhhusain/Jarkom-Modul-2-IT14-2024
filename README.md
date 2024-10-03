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
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Sriwijaya(DNS Master)
```
echo -e "nameserver 192.168.122.1\nnameserver 192.240.3.2" > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
chmod +x ./script-no-2.sh
chmod +x ./script-no-3.sh
chmod +x ./script-no-4.sh
chmod +x ./script-no-6.sh
./script-no-2.sh
./script-no-3.sh
./script-no-4.sh
./script-no-6.sh
```
Majapahit (DNS Slave)
```
echo -e "nameserver 192.168.122.1\nnameserver 192.240.3.2" > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

```

Client
```
echo -e "nameserver 192.168.122.1\nnameserver 192.240.3.2\nnameserver 192.240.1.2" > /etc/resolv.conf
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
script-no-2.sh
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
script-no-3.sh
```
echo 'zone "pasopati.it14.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it14.com";
};' >> /etc/bind/named.conf.local

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

script-no-4.sh
```
echo 'zone "rujapala.it14.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it14.com";
};' >> /etc/bind/named.conf.local

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

HayamWuruk, Srikandi, AlbertEinstein <br/>
Srikandi <br />
```
ping sudarsana.it14.com -c 3
ping pasopati.it14.com -c 3
ping rujapala.it14.com -c 3
```
![image](https://github.com/user-attachments/assets/c62ae2c0-cea7-4827-99c6-9d1449329900) <br/>
![image](https://github.com/user-attachments/assets/4ba9e445-ea35-4e50-aec4-6969ceb2b1a4) <br/>
![image](https://github.com/user-attachments/assets/5c532ddc-36f4-40c7-b09d-b5de4877bca4) <br/>
HayamWuruk <br />
![image](https://github.com/user-attachments/assets/504c56fe-b5cb-4b12-88a2-22913933a131) <br/>
![image](https://github.com/user-attachments/assets/2a104ad6-e30e-4668-8381-cb0d1f3f197f) <br/>
![image](https://github.com/user-attachments/assets/1af1c0f1-963e-4452-ad27-064dab339044) <br />

AlbertEinstein <br />
![image](https://github.com/user-attachments/assets/4f73f0be-02a1-48cf-8dec-f067921201cd) <br/>
![image](https://github.com/user-attachments/assets/f001861a-79fe-4a42-a091-e1b4c1c3326e) <br/>
![image](https://github.com/user-attachments/assets/e836bb5a-fc9f-4e54-bccf-19dcc66e3152) <br />

6. Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

192.240.2.3

in DNS Master
```
echo 'zone "2.240.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.240.192.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/2.240.192.in-addr.arpa

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it14.com. root.pasopati.it14.com. (
                        2024100301      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
2.240.192.in-addr.arpa.      IN      NS      pasopati.it14.com.
3                            IN      PTR     pasopati.it14.com.' > /etc/bind/jarkom/2.240.192.in-addr.arpa

service bind9 restart
```
in client
```
host -t PTR 192.240.2.3
```
![image](https://github.com/user-attachments/assets/11f427d6-c668-4b07-9289-81994e63a56b)


7. Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

DNS Server Utama (Sriwijaya)

```
echo '
zone "sudarsana.it14.com" {
    type master;
    notify yes;
    also-notify { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    allow-transfer { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    file "/etc/bind/jarkom/sudarsana.it14.com";
};
zone "pasopati.it14.com" {
    type master;
    notify yes;
    also-notify { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    allow-transfer { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    file "/etc/bind/jarkom/pasopati.it14.com";
};
zone "rujapala.it14.com" {
    type master;
    notify yes;
    also-notify { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    allow-transfer { 192.240.1.2; }; // Masukan IP Majapahit tanpa tanda petik
    file "/etc/bind/jarkom/rujapala.it14.com";
};
echo 'zone "2.240.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.240.192.in-addr.arpa";
};
' >> /etc/bind/named.conf.local
service bind9 restart
```
DNS Slave (Majapahit)
```
echo '
zone "sudarsana.it14.com" {
    type slave;
    masters { 192.240.3.2; };
    file "/var/lib/bind/sudarsana.it14.com";
};
zone "pasopati.it14.com" {
    type slave;
    masters { 192.240.3.2; };
    file "/var/lib/bind/pasopati.it14.com";
};
zone "rujapala.it14.com" {
    type slave;
    masters { 192.240.3.2; };
    file "/var/lib/bind/rujapala.it14.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
Testing <br/>
pada sriwijaya
```
service bind9 stop
```
![image](https://github.com/user-attachments/assets/a5631122-a682-4643-a603-c5caac2deea0) <br />
lalu ping <br />
![image](https://github.com/user-attachments/assets/ec3563f3-74fb-4dfb-bd53-cf811ec64c7a)

8. Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

DNS Master (Sriwijaya) <br/>

```
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
www     IN      CNAME   sudarsana.it14.com.
cakra   IN      A       192.240.2.2
@       IN      AAAA    ::1' > /etc/bind/jarkom/sudarsana.it14.com

service bind9 restart
```

```
ping cakra.sudarsana.it14.com
```
![image](https://github.com/user-attachments/assets/34431684-4cfd-44c7-a956-ab8f1e853734)

9. Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

DNS Master

```

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
www     IN      CNAME   pasopati.it14.com.
ns1     IN      A       192.240.1.2
panah   IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/jarkom/pasopati.it14.com

echo '
options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
}; ' > /etc/bind/named.conf.options
service bind9 restart
```

DNS Slave
```
# Update named.conf.options
echo '
options {
        directory "/var/cache/bind";

        allow-query { any; };

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

# Restart BIND9 service to apply the options
service bind9 restart

# Add a new zone configuration to named.conf.local
echo '
zone "panah.pasopati.it14.com" {
    type master;
    file "/etc/bind/panah/panah.pasopati.it14.com";
};' >> /etc/bind/named.conf.local

# Create directory for the zone files
mkdir -p /etc/bind/panah

# Copy the template file to create a new zone file
cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it14.com

# Create the zone file for the domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it14.com. root.panah.pasopati.it14.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it14.com.
@       IN      A       192.240.2.3     ; IP Kotalinga
@       IN      AAAA    ::1
www     IN      CNAME   panah.pasopati.it14.com.
' > /etc/bind/panah/panah.pasopati.it14.com
service bind9 restart
```
![image](https://github.com/user-attachments/assets/9c4df210-5a99-46bb-93b1-973035aaa693) <br />
![image](https://github.com/user-attachments/assets/f76a63fa-de09-4616-8c64-27c41546edea) <br />




10. Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.
Pada DNS Slave
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it14.com. root.panah.pasopati.it14.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it14.com.
@       IN      A       192.240.2.3     ; IP Kotalinga
@       IN      AAAA    ::1
www     IN      CNAME   panah.pasopati.it14.com.
log     IN      A       192.240.2.3
www.log IN      CNAME   panah.pasopati.it14.com.
' > /etc/bind/panah/panah.pasopati.it14.com
service bind9 restart
```

```
ping log.panah.pasopati.it14.com
```
![image](https://github.com/user-attachments/assets/30e8ec00-1cb3-490c-a873-0fc233c7e598)








