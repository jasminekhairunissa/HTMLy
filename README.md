<h1 align="center"><img src="https://i.ibb.co/WWSp7ZQ/logo-big.png" alt="logo-big" border="0"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ kembali ke atas ^`](#)

**HTMLy** adalah sebuah Content Management System (CMS) berbentuk platform *blogging* yang dibangun menggunakan PHP, mengutamakan kecepatan dan kesederhanaan. **HTMLy** memiliki banyak keunggulan, diantaranya adalah: tidak memerlukan database, menyediakan dukungan *markdown* WYSIWYG, hingga CSS yang fleksibel. Untuk keterangan lebih lanjut, dapat dilihat di <a href="https://www.htmly.com/">laman utama HTMLy</a>.


# Instalasi
[`^ kembali ke atas ^`](#)

#### Kebutuhan Sistem :
- Unix, Linux atau Windows.
- Apache 2
- PHP 7.2

#### Proses Instalasi :
1. Install Apache2 ke dalam server Ubuntu.
    ```
    $ sudo apt update
    $ sudo apt install apache2
    ```

2. Install PHP7.2.
    ```
    $ sudo apt-get install software-properties-common
    $ sudo add-apt-repository ppa:ondrej/php
    $ sudo apt update
    $ sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2 cli php7.2-curl php7.2-zip
    ```

3. Buka file **config** dari Apache 2. 
    ```
    $ sudo nano /etc/php/7.2/apache2/php.ini
    ```

4. Ubah isi file **php.ini** sesuai dengan baris berikut:
    ```
    file_uploads = On
    allow_url_fopen = On
    memory_limit = 256M
    upload_max_filesize = 100M
    max_execution_time = 360
    date.timezone = America/Chicago
    ```

5. Restart Apache 2.
    ```
    $ sudo systemctl restart apache2.service
    ```

6. Cek apakah Apache sudah bekerja atau belum dengan membuat file **phpinfo.php**.
    ```
    $ sudo nano /var/www/html/phpinfo.php
    
    ```

7. Isi file **phpinfo.php** dengan:
    ```
    <?php phpinfo( ); ?>
    ```

8. Buka http://localhost/phpinfo.php, jika Apache bekerja, maka akan muncul tampilan sebagai berikut:
   <img src="https://i.ibb.co/QjxHKHC/php-ubuntu-test-nginx.png" alt="php-ubuntu-test-nginx" border="0"></img>
   
9. Unduh packages HTMLy dari GitHub.
    ```
    $ sudo mkdir -p /var/www/html/htmly
    $ cd /var/www/html/htmly
    $ sudo wget https://github.com/danpros/htmly/releases/download/v2.7.4/installer.php
    
    $ sudo chown -R www-data:www-data /var/www/html/htmly/
    $ sudo chmod -R 755 /var/www/html/htmly/
    ```

10. Untuk konfigurasi buat file dengan nama **htmly.conf**
    ```
    $ sudo nano /etc/apache2/sites-available/htmly.conf
    ```

11. Isi file tersebut dengan:
    ```
    <VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/htmly/
     ServerName example.com
     ServerAlias www.example.com

     <Directory /var/www/html/htmly/>
          Options FollowSymlinks
          AllowOverride All
          Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined

    </VirtualHost>
    ```
12. Aktifkan HTMLy, *rewrite* modul, dan *restart* Apache.
    ```
    $ sudo a2ensite htmly.conf
    $ sudo a2enmod rewrite
    
    $ sudo systemctl restart apache2.service
    ```
13. Buka server hostname melalui browser, akan muncul tampilan sebagai berikut:
<img src="https://i.ibb.co/4VqMjJx/htmly-ubuntu-install.png" alt="htmly-ubuntu-install" border="0"></img>

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
   Pada menu profil kita bisa juga merubah profile picture ataupun merubah username kita ,
   <h1 align="center"><img src="https://i.ibb.co/sHWNZ3K/changepicture.png" alt="changepicture" border="0"></h1>
   <h1 align="center"><img src="https://i.ibb.co/LS41M82/changebb.png" alt="changebb" border="0"></h1>
   <h1 align="center"><img src="https://i.ibb.co/FHqzj3V/dahgnti.png" alt="dahgnti" border="0"></h1>
   <h1 align="center"><img src="https://i.ibb.co/7r7ZdQL/gantiprofil.png" alt="gantiprofil" border="0"></h1>

5. Juga terdapat fitur search, dimana kita dapan mencari akun-akun milik orang lain yang mungkin akan kita ikuti, atau kita tambahkan sebagai orang yang diikuti

     <h1 align="center"><img src="https://i.ibb.co/8mFLTH2/search.png" alt="search" border="0"></h1>

6. Kemudian, kita juga dapat mengklik menu setting, dan salah satunya kita bisa membuka Account. pada Account ini salah satu yang dapat kita lakukan adalah mengatur halaman apa yang kita inginkan muncul pada saat pertama kali masuk.

<h1 align="center"><img src="https://i.ibb.co/vYhn5sQ/account.png" alt="account" border="0"></h1>
        
7. Terdapat juga tab bernama Streams, disini terdapat pilihan content apa yang ingin kita lihat, mislnya content public atau local, seperti dibawah ini,

    <h1 align="center"><img src="https://i.ibb.co/k8Svdhr/streams-local.png" alt="streams-local" border="0"></h1>
    <h1 align="center"><img src="https://i.ibb.co/G5TBBVB/streams-public.png" alt="streams-public" border="0"></h1>

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

1. [Install HTMLy CMS On Ubuntu 16.04 / 18.04 LTS With Apache2, PHP 7.2 Support](https://websiteforstudents.com/install-htmly-cms-on-ubuntu-16-04-18-04-lts-with-apache2-php-7-2-support/) - websiteforstudents
2. [HTMLy](https://github.com/danpros.htmly) - HTMLy


