<h1 align="center"><img src="https://i.ibb.co/hK251gp/Socialhome-dark-600.png" alt="Socialhome-dark-600" border="0"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ kembali ke atas ^`](#)

**Socialhome** adalah kumpulan profil personal dengan fungsi jejaring sosial. Pengguna dapat membuat konten menggunakan *Markdown* dan HTML/CSS/JS jika sudah terdaftar sebagai *trusted user*. Saat ini Socialhome menggunakan <a href = "https://diaspora.github.io/diaspora_federation/">protokol Diaspora</a>, sehingga pengguna dapat berinteraksi satu sama lain.


# Instalasi
[`^ kembali ke atas ^`](#)

#### Kebutuhan Sistem :
- Unix, Linux atau Windows.
- uWSGI (~200mb)
- Channels worker (~85mb)
- Daphne (~60mb)
- Circus (~25mb)
- 5x RQ workers (~75mb each == 375mb)
- RQ scheduler (~75mb)
- Database sesuai dengan kebutuhan pengguna (100K content -> aprox. 230MB)

#### Proses Instalasi :
1. Login kedalam server menggunakan SSH. Untuk pengguna windows bisa menggunakan aplikasi [PuTTY](http://www.putty.org/).
    ```
    $ ssh adam@172.18.88.88 -p 22
    ```

2. Pastikan seluruh paket sistem kita *up-to-date*, dan install seluruh kebutuhan sisrem seperti `Apache`, `PHP`, dan `MySQL`.
    ```
    $ sudo apt-get update
    $ sudo apt-get install apache2
    $ sudo apt-get install mysql-server
    $ sudo apt-get install php
    $ sudo apt-get install libapache2-mod-php
    $ sudo apt-get install php-mysql
    $ sudo apt-get install php-gd php-mcrypt php-mbstring php-xml php-ssh2 php-curl php-zip php-intl
    $ sudo apt-get install unzip
    ```

3. Unduh **Prestashop** ke dalam direktori kita. 
    ```
    $ wget https://download.prestashop.com/download/releases/prestashop_1.7.0.5.zip
    ```

4. Ekstrak file yang telah diunduh ke dalam direktori yang kita inginkan.
    ```
    $ sudo unzip prestashop_1.7.0.5.zip -d /var/www/html/prestashop
    ```

5. Ubah otorisasi kepemilikan ke user www-data (webserver)
    ```
    $ sudo chown -R www-data:www-data /var/www/html/prestashop
    ```

6. Buat database dan user untuk **Prestashop**.
    ```
    $ mysql -u root -p -v -e "
        CREATE DATABASE prestashop;
        CREATE USER 'prestashopuser'@'localhost' IDENTIFIED BY 'prestashoppassword';
        GRANT ALL PRIVILEGES ON `prestashop`.* TO 'prestashopuser'@'localhost';
        FLUSH PRIVILEGES;"
    ```

7. Konfigurasi Apache web server.
    ```
    $ sudo a2enmod rewrite
    $ sudo touch /etc/apache2/sites-available/prestashop.conf
    $ sudo ln -s /etc/apache2/sites-available/prestashop.conf /etc/apache2/sites-enabled/prestashop.conf
    $ sudo nano /etc/apache2/sites-available/prestashop.conf

    <VirtualHost *:80>
    ServerAdmin admin@your-domain.com
    DocumentRoot /var/www/html/prestashop/
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    <Directory /var/www/html/prestashop/>
    Options FollowSymLinks
    AllowOverride All
    Order allow,deny
    allow from all
    </Directory>
    ErrorLog /var/log/apache2/your-domain.com-error_log
    CustomLog /var/log/apache2/your-domain.com-access_log common
    </VirtualHost>
    ```

8. Edit file `etc/php/7.0/apache2/php.ini` dan tambahkan baris berikut :
    ```
    memory_limit = 128M
    upload_max_filesize = 16M
    max_execution_time = 60
    file_uploads = On
    allow_url_fopen = On
    magic_quotes_gpc = Off
    register_globals = Off
    ```

9. Restart kembali Apache web server.
    ```
    $ sudo service apache2 restart
    ```

10. Kunjungi alamat IP web server kita untuk meneruskan instalasi.
    - Pilih Bahasa yang akan digunakan

      ![1](https://4.bp.blogspot.com/-4Bd2ScecDIs/WNfZ0H8j3UI/AAAAAAAAGjE/9f7Knlqzgw0a0Lgd2AVQ7Qt53bI-Of8bACLcB/s1600/36.PNG)

    - Setujui persyaratan yang berlaku

      ![2](https://4.bp.blogspot.com/-mglU1XDt-T0/WNfZ0OJ7n8I/AAAAAAAAGjI/bG23YpPUkyEOCiozy1_Qc4TnA29bJw0lACLcB/s1600/37.PNG)

    - Cek kecocokan sistem

      ![3](https://3.bp.blogspot.com/-ewzlTX1qtmw/WNfZ0HTeFuI/AAAAAAAAGjM/edNiBt1f24Qt4x4sWCoCHfyo7JXWWmoZwCLcB/s1600/38.PNG)

    - Isi informasi tentang toko yang kita buat

      ![4](https://2.bp.blogspot.com/-Q5cCz5hyubQ/WNfZ1FZod9I/AAAAAAAAGjU/H_uUfxtZLUE11VPDafwK8jR3-aealPKcgCLcB/s1600/39.png)

    - Konfigurasi database

      ![5](https://1.bp.blogspot.com/-rh08nNV2Leg/WNfZ1DAaDOI/AAAAAAAAGjY/R5oIKIMI4rYjg7gO71MgR26JSMahxtpxgCLcB/s1600/40.PNG)

    - Lanjutkan proses instalasi

      ![6](https://3.bp.blogspot.com/-t2MrsQBYXBU/WNfZ0x4YoWI/AAAAAAAAGjQ/zOqZVNSFIpQkQjY0awofbetdEowQLdGAwCLcB/s1600/41.PNG)

11. Setelah proses instalasi selesai hapus direktori install untuk alasan keamanan.
    ```
    $ sudo rm -rf /var/www/html/prestashop/install
    ```



# Konfigurasi
[`^ kembali ke atas ^`](#)

- Untuk menentukan konfigurasi umum, kuota upload, dan pemberitahuan, kita dapat membuka submenu **Administration** pada menu **Advanced Parameters** dan mengisi field sesuai kebutuhan. 
    
    ![adv](https://1.bp.blogspot.com/-FVf16Vgl39w/WNgF9uD_R1I/AAAAAAAAGkI/SMY8oR4ZpDwNJAP4te0Ml0xCghuEYwQfQCLcB/s1600/Screenshot_6.jpg)

    ![setting](https://1.bp.blogspot.com/-pPGnvOtpH6k/WNgF-bV6TcI/AAAAAAAAGkQ/i4X-qAe2ohcLT18UDAaA5tYDZGrri0nvQCLcB/s1600/ss.png)

- Untuk melengkapi aplikasi, kita dapat menambahkan fitur atau modul-modul tertentu pada menu `Modules`.

    ![modul](https://4.bp.blogspot.com/-6dRdIL2WQGw/WNfs8Ul0KnI/AAAAAAAAGjw/_TmOk2h3mIgRc7Z0Uw1kYLx7bIDaZ-Z4wCLcB/s1600/Screenshot_2.jpg)

- Untuk memperindah aplikasi, kita dapat mengganti tema aplikasi pada menu `Design`.

    ![design](https://4.bp.blogspot.com/-HSXimyvqUVc/WNfs9sGUKnI/AAAAAAAAGj0/l3ZyZX2biuUa05VhnVdwrdFcCxxpGWv0gCLcB/s1600/Screenshot_3.jpg)



# Maintenance
[`^ kembali ke atas ^`](#)

Ketika kita ingin memodifikasi toko yang sudah terinstall, kita mungkin tidak ingin ada orang lain yang membuka aplikasi kita. Pada saat seperti itu, kita dapat mengkonfigurasi aplikasi kita untuk masuk ke dalam *maintenance mode*. Berikut ini adalah langkah-langkah yang harus kita lakukan :
1. Login ke dalam admin toko kita.
2. Klik submenu **General** pada menu **Shop Parameters**.

    ![shop](https://2.bp.blogspot.com/-jD8tqsXFEZU/WNgF9oM9htI/AAAAAAAAGkE/y5imPsRHlC8WE4FWW_4Ypt7B5qldQwGOACLcB/s1600/Screenshot_4.jpg)

3. pilih tab **Maintenance**.

    ![maintenance](https://2.bp.blogspot.com/-nP-fEgmv0Nk/WNgF9liISII/AAAAAAAAGkM/79LNJAoksb0J5dhVSqpo2Q4mZf3G4z-YwCLcB/s1600/Screenshot_5.jpg)

4. Klik tombol `on` atau `off` untuk menjalankan atau mematikan *maintenance mode*.
5. Jika kita ingin agar teman kita dapat membuka aplikasi saat sedang dalam *maintenance mode*, masukkan **IP Adress** miliknya ke dalam field **Maintenance IP**.
6. Tuliskan pesan yang ingin kita sampaikan ketika ada orang yang membuka aplikasi kita saat sedang maintenance ke dalam field **Custom Maintenance Text**
7. Klik tombol **Save** untuk menyimpan perubahan.



# Otomatisasi
[`^ kembali ke atas ^`](#)

Jika kalian masih merasa kesulitan dalam meng-install **Prestashop**, terdapat dua cara alternatif yang lebih mudah. Cara pertama adalah dengan menggunakan `script shell` yang otomatis akan menjalankan semua perintah instalasi pada terminal. Contoh `script shell` yang dapat kita gunakan adalah [setup.sh](../master/setup.sh)

Cara kedua adalah dengan menggunakan layanan yang tersedia pada *web-hosting provider*. Dengan layanan tersebut kita hanya perlu satu kali klik untuk meng-install **Prestashop**. Berikut langkah-lankah untuk melakukannya :
1. kita perlu mengunjungi *web-hosting provider* yang menyediakan *script* instalasi **prestashop** otomatis, seperti [SimpleScripts](http://www.simplescripts.com/script_details/install:PrestaShop), [Installatron](http://installatron.com/prestashop), atau [Softaculous](http://www.softaculous.com/apps/ecommerce/PrestaShop).
2. Sebagai contoh, kita akan menggunakan layanan dari [Installatron](http://installatron.com/prestashop). Kunjungi link tersebut lalu klik tombol **Install this Application**.

    ![Installatron](https://4.bp.blogspot.com/-PGjmovGOoOc/WNgQDHbE1RI/AAAAAAAAGk0/90dTTmH15cY6WSWqr8UU8BPETQs4KyxnACLcB/s1600/Screenshot_8.jpg)

3. Isi semua informasi yang dibutuhkan, lalu klik tombol **Install**.

    ![form](https://4.bp.blogspot.com/-5UwbsHAaBe0/WNgQDDjFdhI/AAAAAAAAGk4/coOLiqqP2DcVxq-hHwFa9cVW3P_t6p1tQCLcB/s1600/ss2.png)

4. Tunggu hingga proses instalasi selesai.



# Cara Pemakaian
[`^ kembali ke atas ^`](#)

Cara pemakaian **CMS Prestashop** ini sangat mudah, karena aplikasi ini telah menyediakan *interface* yang mudah dimengerti. Berikut untuk lebih jelasnya :
1. Ini merupakan tampilan awal saat kita membuka Socialhome.

    <h1 align="center"><img src="https://i.ibb.co/kBbBf83/halamanawal.png" alt="halamanawal" border="0"></h1>


2. Pertama-tama kita Sign in terlebih dahulu, 

   <h1 align="center"><img src="https://i.ibb.co/sVScKpf/signin.png" alt="signin" border="0"></h1>

3. Setelah Sign in, ada banyak bisa kita lakukan, tentunya  kita bisa meng-create suatu post
    <h1 align="center"><img src="https://i.ibb.co/zhHhTnp/Create.png" alt="Create" border="0"></h1>
    
4. Kita juga dapat melihat profil kita sendiri, dan pada profil ini, akan ditampilkan juga postingan tadi,
    <h1 align="center"><img src="https://i.ibb.co/kxqLG4R/profilsetelahcreate.png" alt="profilsetelahcreate" border="0"></h1>

5. Juga terdapat fitur search, dimana kita dapan mencari akun-akun milik orang lain yang mungkin akan kita ikuti, atau kita tambahkan sebagai orang yang diikuti

     <h1 align="center"><img src="https://i.ibb.co/8mFLTH2/search.png" alt="search" border="0"></h1>

6. Kemudian, kita juga dapat mengklik menu setting, dan salah satunya kita bisa membuka Account. pada Account ini salah satu yang dapat kita lakukan adalah mengatur halaman apa yang kita inginkan muncul pada saat pertama kali masuk.

<h1 align="center"><img src="https://i.ibb.co/vYhn5sQ/account.png" alt="account" border="0"></h1>
        
7. Terdapat juga tab bernama Streams, disini terdapat pilihan content apa yang ingin kita lihat, mislnya content public atau local, seperti dibawah ini,

    <h1 align="center"><img src="https://i.ibb.co/k8Svdhr/streams-local.png" alt="streams-local" border="0"></h1>
    <h1 align="center"><img src="https://i.ibb.co/G5TBBVB/streams-public.png" alt="streams-public" border="0"></h1>

8. Selain menu-menu yang berhubungan dengan penjualan, **Prestashop** juga menyediakan menu untuk meningkatkan performa website kita seperti menu untuk menginstal modul atau plugin, menu untuk memperindah tampilan website, menu untuk mengatur pengiriman dan pembayaran barang, bahkan menu lokalisasi untuk meningkatkan layanan pada region tertentu.

    ![improve](https://3.bp.blogspot.com/-d_DufFiAblk/WOGbls1KYxI/AAAAAAAAGl0/AsyvLt1zp1IP1BG1PbDaG91PJKV-xDkhACLcB/s1600/presta8.png)

9. Selain itu, terdapat juga menu untuk mempermudah konfigurasi website kita baik itu konfigurasi umum maupun konfigurasi lanjut.

    ![configure](https://4.bp.blogspot.com/-58ke7QyDUwQ/WOGbmCZvHFI/AAAAAAAAGl4/LuQRv6uYmywWjbzmJy82UwNu5qCg8fp1gCLcB/s1600/presta9.png)



# Pembahasan
[`^ kembali ke atas ^`](#)

**Prestashop** ditulis dalam bahasa pemrograman `PHP` yang support untuk penggunaan MySQL. Sebagai salah satu CMS yang paling banyak digunakan di dunia, aplikasi ini menawarkan berbagai kelebihan, diantaranya :
- Aplikasi memiliki panel administrasinya mudah digunakan dan fleksibel, sehingga dapat disesuaikan dengan kebutuhan.
- Mendukung berbagai layanan pembayaran utama, seperti `PayPal`, `VISA`, `MasterCard`, dan `Maestro`.
- Diterjemahkan dalam banyak bahasa, termasuk Bahasa Indonesia.
- Memiliki desain yang *responsive*, sehingga dapat dibuka menggunakan *device* apapun.
- Memiliki lebih dari tiga ratus fitur untuk memudahkan pengguna.
- Banyak pengguna yang berkontribusi pada *discussion boards* dan sejenisnya, sehingga masalah yang dihadapi pengguna dapat cepat terselesaikan.

Tentu saja, sebuah aplikasi pasti memiliki kekurangan. Kekurangan yang dimiliki **Prestashop** antara lain :
- Penggunaan fitur atau modul yang lengkap menyebakan proses loading dari aplikasi ini menjadi sangat lambat
- Penggunaan *resource* memory aplikasi ini cukup besar, terutama ketika menggunakan fitur atau modul yang lengkap.
- Sebagian besar modul dan tema yang tersedia tidak gratis.

Jika dibandingkan dengan CMS sejenisnya seperti **Microweber**, CMS ini memiliki beberapa keunggulan dan kelemahan. Berikut adalah beberapa perbandingan antara kedua CMS ini :
- **Microweber** menyediakan proses design yang fleksibel dengan fitur *Drag and Drop* tanpa batasan, sehingga pengguna bebas mengkreasikan tampilan websitenya. Sedangkan **Prestashop** hanya menyediakan fitur design berupa penggantian template dan logo, adapun template yang tersedia tidak gratis.
- Modul atau plugin yang tersedia pada **Prestashop** jauh lebih banyak dibandingkan pada **Microweber**.
- **Prestashop** memiliki pengguna yang jauh lebih banyak daripada **Microweber** yang aktif pada forum-forum diskusi untuk membantu pengguna pemula.
- **Microweber** lebih ringan daripada **Prestashop** karena modulnya yang sedikit.
- Proses instalasi **Prestashop** lebih mudah karena berbasis PHP saja, sedangkan **Microweber** menggunakan framework laravel sehingga proses instalasi lebih sulit, terutama dalam hal *dependency*.



# Referensi
[`^ kembali ke atas ^`](#)

1. [About PrestaShop](https://www.prestashop.com/) - PrestaShop
2. [How to Log Into a VPS with PuTTY on Windows](https://www.digitalocean.com/community/tutorials/how-to-log-into-a-vps-with-putty-windows-users) - DigitalOcean
3. [How to Install PrestaShop on Ubuntu 16.04](http://idroot.net/linux/install-prestashop-ubuntu-16-04/) - idroot
4. [One Click Install PrestaShop](https://www.prestashop.com/blog/en/how-to-install-prestashop/) - PrestaShop
5. [PrestaShop Review](http://whichshoppingcart.com/prestashop.html) - whishshoppingcart
