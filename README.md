# Jarkom-Modul-3-E08-2022
Laporan Resmi Praktikum III Jaringan Komputer oleh Kelompok E08
### Kelompok E08, PREFIX IP: 192.196
| **No** | **Nama** | **NRP** |
| - | - | - |
| 1. | Halyusa Ard Wahyudi | 5025201088 |
| 2. | Muhammad Ismail | 5025201223 |
| 3. | Immanuel Maruli Tua Pardede | 5025201166 |

### Topologi
<img src="https://github.com/halyusa16/Jarkom-Modul-3-E08-2022/blob/main/img/Topologi.png" width=50%>

## No. 1
### Soal
Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server (1).

### Jawaban
Kita buat topologi sesuai dengan ketentuan soal, lalu konfigurasi masing-masing node sesuai dengan ketentuan soal.

<ul>
  <li>Wise</li>
  
Node Wise sebagai DNS Server:

<img src="https://github.com/halyusa16/Jarkom-Modul-3-E08-2022/blob/main/img/No1_Wise.png" width=50%>

 <li>Westalis</li>
  
Node Westalis sebagai DHCP Server:

<img src="https://github.com/halyusa16/Jarkom-Modul-3-E08-2022/blob/main/img/No1_Westalis.png" width=50%>

 <li>Berlint</li> 
  
Node Berlint sebagai Proxy Server:

<img src="https://github.com/halyusa16/Jarkom-Modul-3-E08-2022/blob/main/img/No1_Berlint.png" width=50%>

## No. 2
### Soal
Ostania sebagai DHCP Relay (2).

### Jawaban
  
Konfigurasi Node Ostania sesuai dengan untuk DHCP Relay.
  
<img src="https://github.com/halyusa16/Jarkom-Modul-3-E08-2022/blob/main/img/No2_Ostania.png" width=50%>

## No. 3
### Soal
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

### Jawaban
  
Pada node Westalis (sebagai DHCP Server), kita tambahkan script berikut ke dalam file konfigurasi dhcp server dengan menggunakan command `nano /etc/dhcp/dhcpd.conf`. Pada script tersebut, kita menggunakan nid subnet 192.196.1.0 karena Switch1.

```
subnet  192.196.1.0 netmask 255.255.255.0 {
    range 192.196.1.50  192.196.1.88;
    range  192.196.1.120 192.196.1.155;
    option routers  192.196.1.1;
    option broadcast-address  192.196.1.255;
    option domain-name-servers 192.196.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}
```

Kemudian, restart dhcp server pada node Westalis dengan menggunakan command `service isc-dhcp-server restart`.

Pada klien (node SSS dan Garden), atur konfigurasi jaringannya dengan cara klik kanan pada masing-masing node, lalu pilih **configure** dan **edit network configuration**. Setelah masukkan script berikut.

```
  auto eth0
  iface eth0 inet dhcp
```

Lalu, restart klien (node SSS dan Garden). Untuk mengecek apakah kita sudah berhasil memberikan IP dinamis lewat DHCP, lihat IP masing-masing klien dengan menggunakan command `ip a` pada web console.
  
## No. 4
### Soal
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85

### Jawaban
  
Pada node Westalis (sebagai DHCP Server), kita tambahkan script berikut ke dalam file konfigurasi dhcp server dengan menggunakan command `nano /etc/dhcp/dhcpd.conf`. Pada script tersebut, kita menggunakan nid subnet 192.196.3.0 karena Switch3.

```
subnet  192.196.3.0 netmask 255.255.255.0 {
    range 192.196.3.10  192.196.3.30;
    range  192.196.3.60 192.196.3.85;
    option routers  192.196.3.1;
    option broadcast-address  192.196.3.255;
    option domain-name-servers 192.196.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}
```

Kemudian, restart dhcp server pada node Westalis dengan menggunakan command `service isc-dhcp-server restart`.

Pada klien (node Eden, NewstonCastle, dan KemonoPark), atur konfigurasi jaringannya dengan cara klik kanan pada masing-masing node, lalu pilih **configure** dan **edit network configuration**. Setelah masukkan script berikut.

```
  auto eth0
  iface eth0 inet dhcp
```

Lalu, restart klien (node Eden, NewstonCastle, dan KemonoPark). Untuk mengecek apakah kita sudah berhasil memberikan IP dinamis lewat DHCP, lihat IP masing-masing klien dengan menggunakan command `ip a` pada web console.

## No. 5

Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### Jawaban

Menambahkan forwarders pada `/etc/bind/named.conf.options` pada WISE

```text
options {
        directory "/var/cache/bind";

        forwarders {
                192.196.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

## No. 6

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

### Jawaban

Pada **Westalis** (DHCP Server) Buka`/etc/dhcp/dhcpd.conf`

Lalu pada network `192.196.1.0` tambahkan `default-lease-time 300;` untuk mengatur waktu minimumnya yaitu 5 menit atau 300 detik dan `max-lease-time 6900;` untuk mengatur waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit atau 6900 detik. 

```text
subnet  192.196.1.0 netmask 255.255.255.0 {
    range 192.196.1.50  192.196.1.88;
    range  192.196.1.120 192.196.1.155;
    option routers  192.196.1.1;
    option broadcast-address  192.196.1.255;
    option domain-name-servers 192.196.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

```

Lalu pada network `192.196.3.0` tambahkan `default-lease-time 600;` untuk mengatur waktu minimumnya yaitu 10 menit atau 300 detik dan `max-lease-time 6900;` untuk mengatur waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit atau 6900 detik.

```text
subnet  192.196.3.0 netmask 255.255.255.0 {
    range 192.196.3.10  192.196.3.30;
    range  192.196.3.60 192.196.3.85;
    option routers  192.196.3.1;
    option broadcast-address  192.196.3.255;
    option domain-name-servers 192.196.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}
```

## No. 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13

### Jawaban 

Pada **Eden** kita perlu menambahkan `hwaddress` dari **Eden** . Dapat dicari dengan perintah `ip a` sehingga diperoleh `22:f3:68:ec:27:30`. tambahkan address tersebut ke konfigurasi IP **Eden**. 

```text
auto eth0
iface eth0 inet dhcp
hwaddress ether 22:f3:68:ec:27:30
```

Setelah itu, kita menuju **Westalis** buka`/etc/dhcp/dhcpd.conf` dan tambahkan

```text
.....

host Eden {
    hardware ethernet 22:f3:68:ec:27:30;
    fixed-address 192.196.3.13;
}
