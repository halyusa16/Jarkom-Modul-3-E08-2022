# Jarkom-Modul-3-E08-2022
Laporan Resmi Praktikum III Jaringan Komputer oleh Kelompok E08
### Kelompok E08, PREFIX IP: 192.196
| **No** | **Nama** | **NRP** |
| - | - | - |
| 1. | Halyusa Ard Wahyudi | 5025201088 |
| 2. | Muhammad Ismail | 5025201223 |
| 3. | Immanuel Maruli Tua Pardede | 5025201166 |


## Soal Nomor 5

Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

## Jawaban Nomor 5

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

## Soal Nomor 6

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

## Jawaban Nomor 6

Pada **Westalis** (DHCP Server) Buka`/etc/dhcp/dhcpd.conf`

### Lalu pada network `192.196.1.0` tambahkan `default-lease-time 300;` untuk mengatur waktu minimumnya yaitu 5 menit atau 300 detik dan `max-lease-time 6900;` untuk mengatur waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit atau 6900 detik. 

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

### Lalu pada network `192.196.3.0` tambahkan `default-lease-time 600;` untuk mengatur waktu minimumnya yaitu 10 menit atau 300 detik dan `max-lease-time 6900;` untuk mengatur waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit atau 6900 detik.

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

## Soal Nomor 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13

## Jawaban Nomor 7

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
```
