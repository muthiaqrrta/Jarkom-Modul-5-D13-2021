# Jarkom-Modul-5-D13-2021

### Anggota kelompok:
Anggota | NRP
------------- | -------------
Muthia Qurrota Akyun | 05111940000019
Fayha Syifa Qalbi | 05111940000185
Raihan Alifianto | 05111940000213

## Soal dan Jawaban
### A. Membuat topologi jaringan sesuai dengan rancangan yang diberikan.

![image](https://user-images.githubusercontent.com/68548653/145531503-26496f59-1d9f-4e86-aa15-3f5f460f3889.png)

   Keterangan : 	
   - Doriki adalah DNS Server
   - Jipangu adalah DHCP Server
   - Maingate dan Jorge adalah Web Server
   - Jumlah Host pada Blueno adalah 100 host
   - Jumlah Host pada Cipher adalah 700 host
   - Jumlah Host pada Elena adalah 300 host
   - Jumlah Host pada Fukurou adalah 200 host

### B. Membuat topologi tersebut menggunakan teknik CIDR atau VLSM.

Nama Subnet | Jumlah Host | Netmask | Subnetmask | IP
------------- | ------------- | ------------- | ------------- | -------------
A1 | 2 | /30 | 255.255.255.252 | 192.198.0.0
A2 | 2 | /30 | 255.255.255.252 | 192.198.0.4
A3 | 201 | /24 | 255.255.255.0 | 192.198.1.0
A4 | 301 | /23 | 255.255.254.0 | 192.198.2.0
A5 | 101 | /25 | 255.255.255.128 | 192.198.0.128
A6 | 701 | /22 | 255.255.252.0 | 192.198.4.0
A7 | 4 | /29 | 255.255.255.248 | 192.198.0.16
A8 | 4 | /29 | 255.255.255.248 | 192.198.0.24
Total | 1316 | /21 | 255.255.248.0 | -

<img src="https://github.com/muthiaqrrta/Jarkom-Modul-5-D13-2021/blob/main/screenshot/CIDR.jpeg">

### C. Melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung
Buat file script.sh kemudian isikan perintah berikut. 
```
route add -net 192.198.1.0 netmask 255.255.255.0 gw 192.198.0.6
route add -net 192.198.2.0 netmask 255.255.254.0 gw 192.198.0.6
route add -net 192.198.0.24 netmask 255.255.255.248 gw 192.198.0.6
route add -net 192.198.4.0 netmask 255.255.252.0 gw 192.198.0.2
route add -net 192.198.0.128 netmask 255.255.255.128 gw 192.198.0.2
route add -net 192.198.0.16 netmask 255.255.255.248 gw 192.198.0.2

echo '
net.ipv4.ip_forward=1
net.ipv4.conf.all.accept_source_route = 1' > /etc/sysctl.conf

echo '
# What servers should the DHCP relay forward requests to?
SERVERS="192.198.0.19"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES=""

# Additional options that are passed to the DHCP relay daemon?
OPTIONS="" '> /etc/default/isc-dhcp-relay

service isc-dhcp-relay restart

#1
iptables -t nat -A POSTROUTING -s 192.198.0.0/16 -o eth0 -j SNAT --to-s 192.168.122.99
```

### D. Mmberikan IP pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian setting DHCP Relay pada router yang menghubungkannya.
Install DHCP menggunakan perintah `apt-get install isc-dhcp-server -y`

Buka file /etc/default/isc-dhcp-server menggunakan perintah `vi /etc/default/isc-dhcp-server`

Kemudian tambahkan `INTERFACES="eth0"`

Buka file /etc/dhcp/dhcpd.conf dengan perintah `vi /etc/dhcp/dhcpd.conf`

Kemudian tambahkan 
```
subnet 192.198.1.0 netmask 255.255.255.0 {
	range 192.198.1.2 192.198.1.254;
	option routers 192.198.1.1;
	option broadcast-address 192.198.1.255;
	option domain-name-servers 192.198.0.18;
	default-lease-time 600;
	max-lease-time 7200;
}
subnet 192.198.0.128 netmask 255.255.255.128 {
	range 192.198.0.130 192.198.0.254;
	option routers 192.198.0.129;
	option broadcast-address 192.198.0.255;
	option domain-name-servers 192.198.0.18;
	default-lease-time 600;
	max-lease-time 7200;
}
subnet 192.198.4.0 netmask 255.255.252.0 {
	range 192.198.4.2 192.198.4.254;
	option routers 192.198.4.1;
	option broadcast-address 192.198.4.255;
	option domain-name-servers 192.198.0.18;
	default-lease-time 600;
	max-lease-time 7200;
}
subnet 192.198.2.0 netmask 255.255.254.0 {
	range 192.198.2.2 192.198.2.254;
	option routers 192.198.2.1;
	option broadcast-address 192.198.2.255;
	option domain-name-servers 192.198.0.18;
	default-lease-time 600;
	max-lease-time 7200;
}
subnet 192.198.0.16 netmask 255.255.255.248{
}

```

### 1. Mengkonfigurasi Foosha menggunakan iptables, tanpa menggunakan MASQUERADE.
**Pada Foosha**

Command yang digunakan `iptables -t nat -A POSTROUTING -s 192.198.0.0/16 -o eth0 -j SNAT --to-s (ip eth0)` yang menyesuaikan dari eth0 tersebut.

Kemudian, pada semua node yang terkait dilakukan `echo nameserver 192.168.122.1 > /etc/resolv.conf`

### 2. Drop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
Pada router Foosha tambahkan role berikut. 
```
iptables -A FORWARD -p tcp --dport 80 -d 192.198.0.16/29 -i eth0 -j DROP
```
Maka paket yang menuju port 80 (HTTP) akan didrop.

### 3. Membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.
Pada Jipangu dan Doriki, berikan komentar berikut
```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

### 4. Membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dimana akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
Pada DNS Server (Doriki) ditambahkan rule berikut.
Untuk paket yang berasal dari Blueno menggunakan perintah
```
iptables -A INPUT -s 192.198.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.198.0.128/25 -j REJECT
```
Untuk paket yang berasal dari Chiper menggunakan perintah
```
iptables -A INPUT -s 192.198.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.198.4.0/22 -j REJECT
```
Jadi akses dari subnet Blueno dan Cipher dibatasi dari 07.00 sampai 15.00 di hari Senin-Kamis, dan selain waktu tersebut ditolak.

### 5. Membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dimana akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya
Pada DNS Server (Doriki) ditambahkan rule berikut
Untuk paket yang berasal dari ELENA menggunakan perintah
```
iptables -A INPUT -s 192.198.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```
Untuk paket yang berasal dari FUKUROU menggunakan perintah
```
iptables -A INPUT -s 192.198.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```
Jadi akses dari subnet Elena dan Fukuro dibatasi dari 00.00 sampai 06.59 dan dari 15.01 sampai 23.59, dan selain waktu tersebut ditolak

### 6. Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
**Pada Doriki**

Membuat domain (DNS) yang mengarah ke IP random (dalam hal ini 192.198.8.1) pada file `/etc/bind/named.conf`


## Kendala
