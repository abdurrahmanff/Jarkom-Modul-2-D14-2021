# Jarkom-Modul-2-D14-2021

Lapres Modul 2 Jaringan Komputer

##  Nama anggota kelompok
1.  A. Zidan Abdillah Majid (05111940000070)
2.  Cliffton Delias Perangin Angin (05111940000181)
3.  Abdurrahman Fauzan Firmansyah (05111940000231)

Prefix IP kelompok kami adalah `10.28.x.x`

### Soal
**1.  EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet**

Jawaban: 

1.  Buat topologi seperti dibawah ini

    ![image](/img/1/topologi.png)

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

      ![image](/img/1/cek_ping_Foosha.png)

    - Loguetown

      ![image](/img/1/cek_ping_Loguetown.png)

    - Alabasta

      ![image](/img/1/cek_ping_Alabasta.png)

    - EniesLobby

      ![image](/img/1/cek_ping_EniesLobby.png)

    - Water7

      ![image](/img/1/cek_ping_Water7.png)

    - Skypie

      ![image](/img/1/cek_ping_Skypie.png) 
  
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

    ![image](/img/2/franky.d14.com.png)
    
    Keterangan:
    
    TBD

5.  Restart `bind9` menggunakan command

    ```
    service bind9 restart
    ```

6.  Buka `Loguetown` dan `Alabasta` dan install **dnsutlis** menggunakan command
    ```
    apt-get update
    apt-get install dnsutils
    ```

7.  Ganti nameserver pada `Loguetown` dan `Alabasta` sehingga menuju ke IP `EniesLobby`, `Water7` dan `Skypie`

    ```
    nameserver 10.28.2.2
    nameserver 10.28.2.2
    nameserver 10.28.2.2
    ```

8.  Cek apakah **franky.d14.com** dan **www.franky.d14.com** sudah bisa diakses menggunakan `ping franky.d14.com`, `host franky.d14.com`, `ping www.franky.d14.com`, dan`host www.franky.d14.com`

    - Loguetown

      cek **franky.d14.com**

      ![image](/img/2/cek_ping_franky_loguetown.png)

      ![image](/img/2/cek_host_franky_loguetown.png)

      cek **www.franky.d14.com**

      ![image](/img/2/cek_ping_www_loguetown.png)

      ![image](/img/2/cek_host_www_loguetown.png)

    - Alabasta

      cek **franky.d14.com**

      ![image](/img/2/cek_ping_franky_alabasta.png)

      ![image](/img/2/cek_host_franky_alabasta.png)

      cek **www.franky.d14.com**

      ![image](/img/2/cek_ping_www_alabasta.png)

      ![image](/img/2/cek_host_www_alabasta.png)

**3. Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie**

1.  Buka `EniesLobby` dan tambahkan file config `franky.d14.com` dengan code dibawah

    ```
    super           IN      A       10.28.2.4
    www.super       IN      CNAME   super
    ```

    ![image](/img/3/franky.d14.com.png)

    Keterangan:

    TBD

2.  Restart `bind9` menggunakan command yang sama seperti sebelumnya

3.  Cek apakah **super.franky.d14.com** dan **www.super.franky.d14.com** sudah bisa diakses menggunakan `ping super.franky.d14.com`, `host super.franky.d14.com`, `ping www.super.franky.d14.com`, dan `host www.super.franky.d14.com`

    - Loguetown

      cek **super.franky.d14.com**

      ![image](/img/3/cek_ping_super_loguetown.png)

      ![image](/img/3/cek_host_super_loguetown.png)

      cek **www.super.franky.d14.com**

      ![image](/img/3/cek_ping_www_loguetown.png)

      ![image](/img/3/cek_host_www_loguetown.png)

    - Alabasta

      cek **super.franky.d14.com**

      ![image](/img/3/cek_ping_super_alabasta.png)

      ![image](/img/3/cek_host_super_alabasta.png)

      cek **www.super.franky.d14.com**

      ![image](/img/3/cek_ping_super_alabasta.png)

      ![image](/img/3/cek_host_super_alabasta.png)

**4. Buat juga reverse domain untuk domain utama**

1.  Buka `EniesLobby` dan edit file `named.conf.local` dengan menambahkan code dibawah

    ```
    zone "2.28.10.in-addr.arpa" {
      type master;
      file "/etc/bind/kaizoku/2.28.10.in-addr.arpa";
    };
    ```
2.  Copykan file `db.local` di `etc/bind/` ke folder `kaizoku` dengan nama `2.28.10.in-addr.arpa`

    ```
    cp /etc/bind/db.local /etc/bind/kaizoku/2.28.10.in-addr.arpa
    ```

3.  Edit file `2.28.10.in-addr.arpa` menjadi seperti gambar dibawah

    ![image](/img/4/in-addr.png)

4.  Restart `bind9` menggunakan command yang sama seperti sebelumnya

5.  Cek pada client `Loguetown` dan `Alabasta` menggunakan command `host 10.28.2.2`

    - Loguetown

      ![image](/img/4/cek_loguetown.png)

    - Alabasta

      ![image](/img/4/cek_alabasta.png)

**5. Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama**

1.  Buka `EniesLobby` dan edit file `named.conf.local` dan ubah menjadi seperti dibawah

    ```
    zone "franky.d14.com" {
        type master;
        notify yes;
        also-notify { 10.28.2.3;  };
        allow-transfer { 10.28.2.3; };
        file "/etc/bind/kaizoku/franky.d14.com";
    };
    ```

    ![image](/img/5/enies_namedlocal.png)

2.  Restart bind9

3.  Buka `Water7` dan install bind9 dengan menggunakan command

    ```
    apt-get update
    apt-get install bind9
    ```

4.  Edit file `named.local.conf` menjadi seperti dibawah

    ![image](/img/5/water7_namedlocal.png)

5.  Restart bind9

6.  Untuk mengecek, matikan terlebih dahulu bind9 pada `EniesLobby` kemudian coba ping **franky.d14.com** melalui `Loguetown` dan/atau `Alabasta`

    ```
    root@EniesLobby:~# service bind9 stop
    ```

    - Loguetown

      ![image](/img/5/cek_ping_loguetown.png)

    - Alabasta

      ![image](/img/5/cek_ping_alabasta.png)


**6. Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo**

1.  Buka `EniesLobby` dan edit file `franky.d14.com` dengan menambahkan code dibawah

    ```
    ns1     IN      A       10.28.2.3
    mecha   IN      NS      ns1
    ```

    ![image](/img/6/enies_franky.d14.com.png)

2.  Edit file `named.conf.local` menjadi seperti dibawah

    ```
    zone "franky.d14.com" {
        type master;
        allow-transfer { 10.28.2.3; };
        file "/etc/bind/kaizoku/franky.d14.com";
    };
    ```

    ![image](/img/6/enies_named.conf.local.png)

3.  Buka file `named.conf.options` di folder `/etc/bind/`. Comment bagian `dnssec-validation auto` dan tambahkan code `allow-query{any;};`

    ```
    // dnssec-validation auto;

    allow-query{any;};
    ```

    ![image](/img/6/enies_named.conf.options.png)

4.  Restart bind9

5.  Buka `Water7`. Edit file `named.conf.local` dengan menambahkan code dibawah

    ```
    zone "mecha.franky.d14.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.d14.com";
    };
    ```

    ![image](/img/6/water7_named.conf.local.png)

6.  Buka file `named.conf.options`. Comment bagian `dnssec-validation auto` dan tambahkan code `allow-query{any;};`

    ```
    // dnssec-validation auto;

    allow-query{any;};
    ```

    ![image](/img/6/water7_named.conf.options.png)

7.  Restart bind9

8.  Buat folder dengan nama `sunnygo` pada `/etc/bind/` dan copykan file `db.local` ke folder yang baru dibuat dengan nama `mecha.franky.d14.com`

    ```
    mkdir /etc/bind/sunnygo
    cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.d14.com
    ```

9.  Buka file `mecha.franky.d14.com` dan ubah menjadi seperti dibawah

    ![image](/img/6/water7_mecha.franky.d14.com.png)

10. Restart bind9

11. Cek apakah `mecha.franky.d14.com` dan `www.mecha.franky.d14.com` bisa pada `Loguetown` dan `Alabasta`

    - Loguetown

      cek **mecha.franky.d14.com**

      ![image](/img/6/cek_ping_mecha_loguetown.png)

      ![image](/img/6/cek_host_mecha_loguetown.png)

      cek **www.mecha.franky.d14.com**

      ![image](/img/6/cek_ping_www_loguetown.png)

      ![image](/img/6/cek_host_www_loguetown.png)

    - Alabasta

      cek **mecha.franky.d14.com**

      ![image](/img/6/cek_ping_mecha_alabasta.png)

      ![image](/img/6/cek_host_mecha_alabasta.png)

      cek **www.mecha.franky.d14.com**

      ![image](/img/6/cek_ping_www_alabasta.png)

      ![image](/img/6/cek_host_www_alabasta.png)

**7. Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie**

1.  Buka `Water7` dan edit file `mecha.franky.d14.com` dengan menambahkan code dibawah

    ```
    general IN      A       10.28.2.4
    www.general     IN      CNAME   general
    ```

    ![image](/img/7/water7_mecha.franky.d14.com.png)

2.  Restart bind9

3.  Buka `EniesLobby` dan restart bind9

4.  Cek apakah `general.mecha.franky.d14.com` dan `www.general.mecha.franky.d14.com` bisa pada `Loguetown` dan `Alabasta`

    - Loguetown

      cek **general.mecha.franky.d14.com**

      ![image](/img/7/cek_ping_general_loguetown.png)

      ![image](/img/7/cek_host_general_loguetown.png)

      cek **www.general.mecha.franky.d14.com**

      ![image](/img/7/cek_ping_www_loguetown.png)

      ![image](/img/7/cek_host_www_loguetown.png)

    - Alabasta

      cek **general.mecha.franky.d14.com**

      ![image](/img/7/cek_ping_general_alabasta.png)

      ![image](/img/7/cek_host_general_alabasta.png)

      cek **www.general.mecha.franky.d14.com**

      ![image](/img/7/cek_ping_www_alabasta.png)

      ![image](/img/7/cek_host_www_alabasta.png)

**8. Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com**

1.  Buka `EniesLobby` dan edit file `franky.d14.com` menjadi seperti dibawah

    ![image](/img/8/enies_franky.d14.com.png)

2.  Restart bind9

3.  Buka `Skypie` dan install `apache2`, `php`, dan `libapache2-mod-php7.0` dengan menggunakan command

    ```
    apt-get update
    apt-get install apache2 php libapache2-mod-php7.0 -y -y -y
    ```

4.  Copykan file `000-default.conf` pada folder `/etc/apache2/sites-available/` ke folder yang sama dengan nama `franky.d14.com.conf`

    ```
    cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/franky.d14.com.conf
    ```

5.  Ubah `DocumentRoot` pada file `franky.d14.com.conf` sehingga mengarah ke `/var/www/franky.d14.com` dan tambahkan server name dan server alias

    ```
    DocumentRoot /var/www/franky.d14.com

    ServerName franky.d14.com
    ServerAlias www.franky.d14.com
    ```

    ![image](/img/8/skypie_franky.d14.com.conf.png)

6.  Aktifkan config yang baru dibuat kemudian restart apache2 menggunakan command

    ```
    a2ensite franky.d14.com.conf
    service apache2 restart
    ```

7.  Karena pada praktikum ini telah disediakan file requirement untuk webnya, jadi bisa langsung didownload menggunakan `curl` dan isinya diextract menggunakan `unzip` ke directory `var/www/` dan rename menjadi `franky.d14.com`

    ```
    curl -o franky.zip https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/franky.zip
    apt-get unzip
    unzip franky.zip -d /var/www/
    mv /var/www/franky /var/www/franky.d14.com
    ```

8.  Buka `Loguetown` dan `Alabasta` kemudian install `lynx`

    ```
    apt-get install lynx
    ```

9.  Buka **http://www.franky.d14.com** menggunakan `lynx`

    - Loguetown

      ![image](/img/8/cek_lynx_loguetown.png)

    - Alabasta

      ![image](/img/8/cek_lynx_alabasta.png)

**9. Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home**

1.  Buka `Skypie` dan edit file `franky.d14.com` dengan menambahkan code dibawah

    ```
    Alias "/home" "/var/www/franky.d14.com/index.php/home"
    ```

    ![image](/img/9/skypie_franky.d14.com.conf.png)

2.  Restart apache2

3.  Buka **http://www.franky.d14.com/home** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`

    - Loguetown

      ![image](/img/9/cek_lynx_loguetown.png)

    - Alabasta

      ![image](/img/9/cek_lynx_alabasta.png)

**10. Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com**

1.  Buka ```Skypie``` dan copykan file `000-default.conf` pada folder `/etc/apache2/sites-available/` ke folder yang sama dengan nama `super.franky.d14.com.conf`

    ```
    cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/super.franky.d14.com.conf
    ```

2.  Ubah `DocumentRoot` pada file `super.franky.d14.com.conf` sehingga mengarah ke `/var/www/super.franky.d14.com` dan tambahkan server name dan server alias

    ```
    DocumentRoot /var/www/franky.d14.com

    ServerName super.franky.d14.com
    ServerAlias www.super.franky.d14.com
    ```

    ![image](/img/10/super.franky.d14.com.conf.png)

3.  Aktifkan config dan restart apache2

    ```
    a2ensite super.franky.d14.com.conf
    service apache2 restart
    ```

4.  Download file requirement dan copykan ke folder `/var/www/` dengan nama super.franky.d14.com

    ```
    curl -o franky.zip https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip
    apt-get unzip
    unzip super.franky.zip -d /var/www/
    mv /var/www/super.franky /var/www/super.franky.d14.com
    ```

5.  Buka **http://www.suoer.franky.d14.com** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`

    ![image](/img/10/lynx_super.png)

**11. Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja**

1.  Buka `Skypie` dan edit file `super.franky.com.conf` dengan menambahkan code dibawah

    ```
    <Directory /var/www/super.franky.d14.com/error>
                Options -Indexes
    </Directory>
    ```

    ![image](/img/11/super.franky.d14.com.conf.png)

    Ket: Karena yang diinginkan untuk directory listing hanya pada folder **public**, maka folder **error** tidak diberi akses.

2.  Restart apache2

3.  Buka **http://www.suoer.franky.d14.com** dan coba akses folder **error** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`. Maka yang keluar adalah error **403 Forbidden**.

    ![image](/img/11/lynx_super-public.png)

    ![image](/img/11/lynx_super-error.png)


**12. Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache**

1.  Buka `Skypie` dan edit file `super.franky.com.conf` dengan menambahkan code dibawah

    ```
    ErrorDocument 404 /error/404.html
    ```

    ![image](/img/12/super.conf.png)

2.  Restart apache2

3.  Buka **http://www.suoer.franky.d14.com/gaada** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`. Maka yang keluar adalah error **404** dengan page `/error/404.html`.

    ![image](/img/12/lynx_super.png)

**13. Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js**

1.  Buka `Skypie` dan edit file `super.franky.com.conf` dengan menambahkan code dibawah

    ```
    Alias "/js" "/var/www/super.franky.d14.com/public/js"
    ```

    ![image](/img/13/super.conf.png)

2.  Restart apache2

3.  Buka **http://www.suoer.franky.d14.com/js** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`. Maka yang keluar adalah directory listing folder `/public/js`

    ![image](/img/13/lynx_super.png)

**14. Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500**

1.  Buka `Skypie` dan edit file `ports.conf` pada folder `/etc/apache2/` dengan menambahkan code dibawah

    ```
    Listen 15000
    Listen 15500
    ```

    ![image](/img/14/ports.conf.png)

2.  copykan file `000-default.conf` pada folder `/etc/apache2/sites-available/` ke folder yang sama dengan nama `super.franky.d14.com.conf`

    ```
    cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/general.mecha.franky.d14.com.conf
    ```

3.  Ubah `DocumentRoot` pada file `general.mecha.franky.d14.com.conf` sehingga mengarah ke `/var/www/general.mecha.franky.d14` dan tambahkan server name dan server alias, serta ubah port menjadi `VirtualHost *:15000 *:15500`

    ```
    <VirtualHost *:15000 *:15500>
        ...
        DocumentRoot /var/www/general.mecha.franky.d14

        ServerName general.mecha.franky.d14.com
        ServerAlias www.general.mecha.franky.d14.com
        ...
    ```

    ![image](/img/14/general.conf.png)

4.  Aktifkan config dan restart apache2

    ```
    a2ensite general.mecha.franky.d14.com.conf
    service apache2 restart
    ```

5.  Download file requirement dan copykan ke folder `/var/www/` dengan nama general.mecha.franky.d14.com

    ```
    curl -o franky.zip https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/general.mecha.franky.zip
    apt-get unzip
    unzip general.mecha.franky.zip -d /var/www/
    mv /var/www/general.mecha.franky /var/www/general.mecha.franky.d14
    ```

6.  Apabila dicoba buka **http://www.general.mecha.franky.d14.com** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`. Maka akan mengeluarkan default apache page

    ![image](/img/14/lynx_tanpaport.png)

7.  Sekarang coba buka **http://www.general.mecha.franky.d14.com:15000** atau **http://www.general.mecha.franky.d14.com:15500** maka akan menampilkan page yang benar

    ![image](/img/14/lynx_denganport.png)

**15. dengan authentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy**

1.  Buka `Skypie` dan edit file `general.mecha.franky.d14.com.conf` dan tambahkan code dibawah

    ```
    <Directory /var/www/general.mecha.franky.d14>
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /var/www/general.mecha.franky.d14/.htpasswd
        Require valid-user
    </Directory>
    ```

    ![image](/img/15/general.conf.png)

2.  Buat file untuk menyimpan username dan password pada directory `var/www/general.mecha.franky.d14/` dengan nama `.htpasswd` dengan menggunakan command dibawah

    ```
    htpasswd -b -c /var/www/general.mecha.franky.d14/.htpasswd luffy onepiece
    ```

    Isi file `.htpasswd`

    ![image](/img/15/isihtpasswd.png)

3.  Restart apache2

4.  Buka **http://www.general.mecha.franky.d14.com:15000** atau **http://www.general.mecha.franky.d14.com:15500** mengguanakn `lynx` pada `Loguetown` atau `Alabasta` maka sekarang kita akan disuruh memasukkan username dan password untuk mengakses halaman tersebut

    ![image](/img/15/lynx_1.png)

    ![image](/img/15/lynx_2.png)

    ![image](/img/15/lynx_3.png)

    ![image](/img/15/lynx_4.png)

**16. Dan setiap kali mengakses IP Skypie akan diahlikan secara otomatis ke www.franky.yyy.com**

1.  Buka `Skypie` dan edit file `000-default.conf` dengna menambahkan code dibawah

    ```
    <Directory /var/www/>
        RewriteEngine On
        RewriteBase "/"
        RewriteCond "%{HTTP_HOST}" "10.28.2.4"
        RewriteRule "/(.*)" "http://www.franky.d14.com" [L,R=301]
    </Directory>
    ```

    ![image](/img/16/000.conf.png)

    Ket: Apabila hostnya merupakan **10.28.2.4** maka akan otomatis diredirect ke http://www.franky.d14.com 

2.  Aktifkan module rewrite dan restart apache

    ```
    a2enmod rewrite
    service apache2 restart
    ```

3.  Buka **10.28.2.4** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta`. Maka akan otomatis keredirect ke http://www.franky.d14.com

    ![image](/img/16/lynx_1.png)

    ![image](/img/16/lynx_2.png)

**17. Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png**

1.  Buka `Skypie` dan edit file `super.franky.d14.com.conf` dengan menambahkan code dibawah

    ```
    <Directory /var/www/super.franky.d14.com/public/images>
        RewriteEngine On
        RewriteBase "/"
        RewriteCond "%{REQUEST_URI}" "(?=.*franky.*)(^((?!franky\.png).)*$)"
        RewriteRule "." "http://www.super.franky.d14.com/public/images/franky.png" [NC,L]
    </Directory>
    ```

    ![image](/img/17/super.conf.png)

    Keterangam: Apabila mengakses file pada folder `/public/images/` akan mengecek nama file yang diakses. Jika nama filenya terdapat substring `franky` dan tidak bernama `franky.png` saja, maka akan diredirect ke http://www.super.franky.d14.com/public/images/franky.png

2.  Restart apache2

3.  Buka **http://www.super.franky.d14.com** dengan menggunakan `lynx` pada `Loguetown` dan `Alabasta` dan coba akses file dalam folder **/public/images/**

    -   Akses **bukanfrankytapirandom.99689**

        ![image](/img/17/lynx_adafranky.png)

    -   Akses **car.jpg**

        ![image](/img/17/lynx_gaadafranky.png)
