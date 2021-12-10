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
Teknik VLSM 

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

<img src="https://github.com/muthiaqrrta/Jarkom-Modul-5-D13-2021/blob/main/screenshot/VLSM.jpeg">

### C. Melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung
### D. Mmberikan IP pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian setting DHCP Relay pada router yang menghubungkannya.

### 1. Mengkonfigurasi Foosha menggunakan iptables, tanpa menggunakan MASQUERADE.
### 2. Drop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
### 3. Membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.
### 4. Membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dimana akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
### 5. Membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dimana akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya
### 6. Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

## Kendala
