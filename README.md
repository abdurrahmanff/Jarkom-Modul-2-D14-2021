# Jarkom-Modul-2-D14-2021

Lapres Modul 2 Jaringan Komputer

##  Nama anggota kelompok
1.  A. Zidan Abdillah Majid (05111940000070)
2.  liffton Delias Perangin Angin (05111940000181)
3.  Abdurrahman Fauzan Firmansyah (05111940000231)

### Soal
**1.  EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet**

Jawaban: 

1.  Buat topologi seperti dibawah ini

    ![image](/img/1_topologi.png)

2.  Ubah `network configuration` dengan config dibawah ini

    - Foosha
    ```
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
    address 10.28.1.1
    netmask 255.255.255.0

    auto eth2
    iface eth2 inet static
    address 10.28.2.1
    netmask 255.255.255.0
    ```

    - Loguetown
    ```
    auto eth0
    iface eth0 inet static
    address 10.28.1.2
    netmask 255.255.255.0
    gateway 10.28.1.1
    ```

    - Alabasta
    ```
    auto eth0
    iface eth0 inet static
    address 10.28.1.3
    netmask 255.255.255.0
    gateway 10.28.1.1
    ```

    - EniesLobby
    ```
    auto eth0
    iface eth0 inet static
    address 10.28.2.2
    netmask 255.255.255.0
    gateway 10.28.2.1
    ```

    - Water7
    ```
    auto eth0
    iface eth0 inet static
    address 10.28.2.3
    netmask 255.255.255.0
    gateway 10.28.2.1
    ```

    - Skypie
    ```
    auto eth0
    iface eth0 inet static
    address 10.28.2.4
    netmask 255.255.255.0
    gateway 10.28.2.1
    ```

2.  Jalankan command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.28.0.0/16` pada router `Foosha`

3.  Jalankan command `cat /etc/resolv.conf` pada router `Foosha` untuk mengecek IP DNS

4.  Jalankan command `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada node selain `Foosha`

5.  Cek apakah semua node bisa connect ke internet dengan mengeping google.com
    - Foosha

      ![image](/img/1_cek_ping_Foosha.png)

    - Longuetown

      ![image](/img/1_cek_ping_Loguetown.png)

    - Alabasta

      ![image](/img/1_cek_ping_Alabasta.png)

    - EniesLobby

      ![image](/img/1_cek_ping_EniesLobby.png)

    - Water7

      ![image](/img/1_cek_ping_Water7.png)

    - Skypie

      ![image](/img/1_cek_ping_Skypie.png) 
  
    Bisa dilihat semua node telah terhubung ke internet.
  
**2. Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku**

Jawaban:

1.  Buka `EniesLobby` dan install **bind9** menggunakan command
    ```
    apt-get update
    apt-get intall bind9
    ```

2.  Edit file `/etc/bind/named.conf.local` dan tambahkan code dibawah
    ```
    zone "franky.d14.com" {
        type master;
        file "/etc/bind/kaizoku/franky.d14.com";
    };
    ```

3.  Buat folder `kaizoku` di `/etc/bind/` dan copykan file `db.local` di `etc/bind/` ke folder `kaizoku` yang telah dibuat dengan nama `franky.d14.com`
    ```
    mkdir /etc/bind/kaizoku
    cp /etc/bind/db.local /etc/bind/kaizoku/franky.d14.com
    ```

4.  Edit file `franky.d14.com` menjadi seperti dibawah ini

    ![image](/img/2_franky.d14.com.png)
    
    Keterangan:
    
    TBD

5.  Buka `Longuetown` dan `Alabasta` dan install **dnsutlis** menggunakan command
    ```
    apt-get update
    apt-get install dnsutils
    ```

6.  Ganti nameserver pada `Longuetown` dan `Alabasta` sehingga menuju ke IP `EniesLobby`, `Water7` dan `Skypie`

    ```
    nameserver 10.28.2.2
    nameserver 10.28.2.2
    nameserver 10.28.2.2
    ```

7.  Cek apakah **franky.d14.com** dan **www.franky.d14.com** sudah bisa diakses menggunakan `ping franky.d14.com`, `host franky.d14.com`, `ping www.franky.d14.com`, dan`host www.franky.d14.com`

    - Loguetown

      cek **franky.d14.com**



      cek **www.franky.d14.com**

    - Alabasta

      cek **franky.d14.com**

      ![image](/img/2_cek_ping_franky_alabasta.png)

      ![image](/img/2_cek_host_franky_alabasta.png)

      cek **www.franky.d14.com**

      ![image](/img/2_cek_ping_www_alabasta.png)

      ![image](/img/2_cek_host_www_alabasta.png)

**3. Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie**

**4. Buat juga reverse domain untuk domain utama**

**5. Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama**

**6. Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo**

**7. Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie**

**8. Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com**

**9. Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home**

**10. Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com**

**11. Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja**

**12. Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache**

**13. Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js**

**14. Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500**

**15. dengan authentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy**

**16. Dan setiap kali mengakses IP Skypie akan diahlikan secara otomatis ke www.franky.yyy.com**

**17. Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png**