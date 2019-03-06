<h1 align="center"><img src="https://i.ibb.co/WWSp7ZQ/logo-big.png" alt="logo-big" border="0"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
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

Ada beberapa cara lain untuk menginstall **HTMLy**:

###Menggunakan Web Installer

1. Unduh <a href="https://github.com/danpros/htmly/releases/tag/v2.7.4">installer.php</a>.

2. Pindahkan file **installer.php** ke folder root web (misal: /var/www/htmly)

3. Instalasi Selesai. Kunjungi web melalui **localhost/installer.php** atau **www.example.com/installer.php**

###Menggunakan Zip Archive

1. Unduh folder zip <a href = "https://github.com/danpros/htmly/releases/tag/v2.7.4">terbaru</a>.

2. Ekstrak zip ke folder server.

3. Instalasi Selesai. Kunjungi web melalui **localhost/installer.php** atau **www.example.com/installer.php**




# Cara Pemakaian
[`^ kembali ke atas ^`](#)

Cara pemakaian **HTMLy** ini sangat mudah, karena aplikasi ini telah menyediakan *interface* yang mudah dimengerti. Berikut untuk lebih jelasnya :
1. Ketika proses penginstalan selesai, maka akan muncul halaman seperti dibawah, isikan data diri berikut akun-akun social media yang kita miliki.

    <h1 align="center"><img src="https://i.ibb.co/7GcX8ZL/Capture.png" alt="Capture" border="0"></h1>


2. Setelah itu kita akan otomatis masuk ke halaman HTMLy milik kita, dan kita bisa membuat post untuk pertama kali dan mempublishnya, 

   <h1 align="center"><img src="https://i.ibb.co/LtHp8Qw/hhhh.png" alt="hhhh" border="0"></h1>
   <h1 align="center"><img src="https://i.ibb.co/Fmq1BdF/heuehu.png" alt="heuehu" border="0"></h1>

3. Terdapat beberapa menu di tab bagian atas, yang pertama yaitu menu Admin, pada saat diklik akan ada tampilan berisi post yang pernah kita publish, kita bisa mengedit ataupun menghapusnya
   kita juga bisa melihat statistiknya
    <h1 align="center"><img src="https://i.ibb.co/pQzzynX/static.png" alt="static" border="0"></h1>
    
4. Terdapat pula menu Posts yang berisi list postingan yang ada, dapat diedit ataupun dihapus
    <h1 align="center"><img src="https://i.ibb.co/568trf7/posst.png" alt="posst" border="0"></h1>
   
5. Kemudian terdapat pula menu Add Content, disinilah kita bisa menambhkan konten yang akan kita post, bisa berbentuk regular, atau yang memiliki fitur gambar, video, dan sebagainya

     <h1 align="center"><img src="https://i.ibb.co/8x3gWSd/add-content.png" alt="add-content" border="0"></h1>

6. Terdapat juga menu categories, dimana akan ditampilkan list kategori yang ada, dan kita juga bisa menambahkan kategori sesuai keinginan kita

<h1 align="center"><img src="https://i.ibb.co/fqMQNdD/categories.png" alt="categories" border="0"></h1>
        
7. Kita juga bisa mengedit profile kita pada menu Edit Profile

    <h1 align="center"><img src="https://i.ibb.co/QJMNvVX/editprofil.png" alt="editprofil" border="0"></h1>

8. Tersedia pula menu Backup , 

    <h1 align="center"><img src="https://i.ibb.co/gzRZGW9/backup.png" alt="backup" border="0"></h1>

9. Kemudian beginilah tampilan Home     
    
    <h1 align="center"><img src="https://i.ibb.co/G3CmVzR/home.png" alt="home" border="0"></h1>

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
2. [HTMLy](https://github.com/danpros.htmly) - github
3. [Installations](https://docs.htmly.com/basics/installations) - htmly


