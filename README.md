# Jarkom_Modul2_Lapres_B05

- Vania Meilani Taqiyyah (05111840000045)
- Elvira Catrine Natalie (05111840000016)

## Penting untuk dibaca!
1. Disarankan untuk mengganti password user kelompok.
2. [SARAN] Selalu mem-backup konfigurasi pada setiap UML untuk mengantisipasi kejadian yang tidak
  diinginkan. Back up disimpan di luar UML.
3. yyy = **b05**
4. IP Server Probolinggo : NID_DMZ_tiap_kelompok + 4 = **810.151.83.52**
5. Jika tidak bisa menginstall php5 maka gunakan php7.0.
6. File pendukung: 
  - File/folder pendukung untuk web http://semeruyyy.pw bisa di download dengan cara:
  wget 10.151.36.202/semeru.pw.zip
  - File/folder pendukung untuk web http://penanjakan.semeruyyy.pw bisa di download dengan cara:
  wget 10.151.36.202/penanjakan.semeru.pw.zip
  - File/folder pendukung untuk web http://naik.gunung.semeruyyy.pw bisa di download dengan cara:
  wget 10.151.36.202/naik.gunung.semeru.pw.zip
  
## Soal
<img src="https://user-images.githubusercontent.com/61219556/98789471-ea231880-2434-11eb-8375-b9219c917a68.PNG" width="500" height="auto">

1. Server MALANG       = DNS Server Master
2. Server MOJOKERTO    = DNS Server Slave
3. Server PROBOLINGGO  = Web Server
4. Klien GRESIK & Klien SIDOARJO
5. Router SURABAYA

**Tahap awal sebelum membuat DNS sesuai dengan modul Pengenalan UML, hanya saja ditambahkan server PROBOLINGGO pada topologi**

<img src="https://user-images.githubusercontent.com/61219556/98794126-42f5af80-243b-11eb-8f91-8aa742186803.PNG" width="500" height="auto">

### 1. Membuat Domain alamat http://semeruyyy.pw
- Buka UML MALANG dan update package list dengan command <br>
  `apt-get update`. </br>
- Install aplikasi bind9 pada UML MALANG dengan <br>
   `apt-get install bind9 -y`. </br>
- Buka file dengan perintah `nano /etc/bind/named.conf.local`, lalu isi configurasi domain dengan syntax sebagai berikut : <br>
```
zone "semerub05.pw" {
	type master;
	file "/etc/bind/jarkom/semerub05.pw";
};
```
</br>
<img src="https://user-images.githubusercontent.com/61219556/99037041-7e1bee00-25b5-11eb-8ff5-cbf05829be6c.PNG" width="500" height="auto">

- Buat folder jarkom di dalam /etc/bind dengan perintah <br>
  `mkdir /etc/bind/jarkom`. </br>
- Copy file **db.local** ke dalam folder **jarkom** dan ubah namanya menjadi **semerub05.pw**. <br>
  `cp /etc/bind/db.local /etc/bind/jarkom/semerub05.pw`. </br>
- Buka file **semerub05.pw** dengan perintah `nano /etc/bind/jarkom/semerub05.pw`. Lalu, edit file dengan memasukkan IP PROBOLINGGO seperti berikut	:

<img src="https://user-images.githubusercontent.com/61219556/99183483-b286da00-276e-11eb-9b54-85192eda99a1.PNG" width="500" height="auto">

- Restart bind9 untuk meng-update perubahan dengan perintah `service bind9 restart`.
- Untuk cek koneksi DNS, lakukan `ping semerub05.pw` pada UML client GRESIK dan SIDOARJO.

<img src="https://user-images.githubusercontent.com/61219556/99183494-cb8f8b00-276e-11eb-9cff-fe5fe3f4380d.PNG" width="500" height="auto">

### 2. Membuat alias dari http://www.semeruyyy.pw
- Pada UML MALANG, buka file **semerub05.pw** dan tambahkan configurasi berupa **record CNAME** seperti berikut	:

<img src="https://user-images.githubusercontent.com/61219556/99183510-ecf07700-276e-11eb-8010-4f43c2babedb.PNG" width="500" height="auto">

- Restart bind9 dengan perintah <br>
  `service bind9 restart`. </br>
- Lalu, buka UML client yaitu GRESIK atau SIDOARJO untuk melakukan `ping semerub05.pw` dan hasil harus mengarah ke host dengan IP PROBOLINGGO.

<img src="https://user-images.githubusercontent.com/61219556/99183530-18736180-276f-11eb-860a-578b858cd374.PNG" width="500" height="auto">

### 3. Membuat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO 
- Buka UML MALANG, buka file **semerub05.pw** dan edit dengan menambahkan subdomain untuk **semerub05.pw** yang mengarah pada IP PROBOLINGGO <br>
`nano /etc/bind/jarkom/semerub05.pw`</br>

<img src="https://user-images.githubusercontent.com/61219556/99183555-583a4900-276f-11eb-9f9c-3f9fd041de5b.PNG" width="500" height="auto">

- Restart service bind9
- Lalu `ping penanjakan.semerub05.pw` pada UML client GRESIK.

<img src="https://user-images.githubusercontent.com/61219556/99183564-6d16dc80-276f-11eb-8f54-8b74164c7d15.PNG" width="500" height="auto">

### 4. Reverse domain untuk domain utama
- Buka UML MALANG, edit file pada `nano /etc/bind/named.conf.local` dengan menambahkan configurasi seperti berikut : <br>
```
zone "83.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/83.151.10.in-addr.arpa";
};
```
</br>
<img src="https://user-images.githubusercontent.com/61219556/99039786-8d516a80-25ba-11eb-97f0-5941d25929df.PNG" width="500" height="auto">

- Copy file **db.local** ke dalam folder **jarkom** dan ubah nama menjadi **83.151.10.in-addr.arpa**.
- Edit file dengan mengganti IP PROBOLINGGO seperti gambar berikut	:

<img src="https://user-images.githubusercontent.com/61219556/99183816-43f74b80-2771-11eb-9ccc-bc5a70a4dce1.PNG" width="500" height="auto">

- Restart bind9 untuk menyimpan perubahan yang ada.
- Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada UML client GRESIK atau SIDOARJO . <br>
`apt-get update`</br><br>
`apt-get install dnsutils`</br><br>
`host -t PTR 10.151.83.52`</br>
</br>
<img src="https://user-images.githubusercontent.com/61219556/99183825-54a7c180-2771-11eb-8918-6d7a14f9f20f.PNG" width="500" height="auto">

### 5. Membuat DNS Server Slave pada MOJOKERTO
- Buka UML MALANG, edit file `etc/bind/named.conf.local` seperti dibawah ini :
```
zone "semerub05.pw" {
    type master;
    notify yes;
    also-notify { 10.151.83.51; }; // IP MOJOKERTO
    allow-transfer { 10.151.83.51; }; 
    file "/etc/bind/jarkom/jarkom2020.com";
};
```
<img src="https://user-images.githubusercontent.com/61219556/99141888-29927480-2682-11eb-94ac-68b6ae988e6b.PNG" width="500" height="auto">

- Restart bind9 untuk menyimpan perubahan yang ada.
- Buka UML MOJOKERTO, edit file `etc/bind/named.conf.local` seperti dibawah ini :
<br>
```
zone "semerub05.pw" {
    type slave;
    masters { 10.151.83.50; }; // IP MALANG
    file "/var/lib/bind/semerub05.pw";
};
```
</br>

<img src="https://user-images.githubusercontent.com/61219556/99141963-f0a6cf80-2682-11eb-8f85-b39173469820.PNG" width="500" height="auto">

- Restart bind9 untuk menyimpan perubahan yang ada.
- Buka UML MALANG, matikan server bind9 (untuk melakukan testing) dengan perintah <br>
`service bind9 stop` </br>
- Lakukan `ping semerub05.pw`.

img src="https://user-images.githubusercontent.com/61219556/99183836-6f7a3600-2771-11eb-867e-5384acf64501.PNG" width="500" height="auto">

### 6. Membuat Subdomain dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO & 
### 7. Membuat subdomain dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO
- Buka UML MALANG, edit file `etc/bind/jarkom/semerub05.pw` seperti berikut :

<img src="https://user-images.githubusercontent.com/61219556/99142139-dd94ff00-2684-11eb-813e-9e90f10aee50.PNG" width="500" height="auto">

- Lalu, edit `file/etc/bind/named.conf.options` pada UML MALANG 
`nano /etc/bind/named.conf.options`
- Kemudian comment **dnssec-validation auto;** dan tambahkan baris berikut pada `/etc/bind/named.conf.options`<br>
`allow-query{any;};`

<img src="https://user-images.githubusercontent.com/61219556/99142447-9a885b00-2687-11eb-9a83-806bb4ffa566.PNG" width="500" height="auto">

- Kemudian, edit file `etc/bind/named.conf.local`

<img src="https://user-images.githubusercontent.com/61219556/99142503-07035a00-2688-11eb-9337-9d269bda21af.PNG" width="500" height="auto">

- Restart bind9 
- Buka UML MOJOKERTO, lalu buka dan edit file pada `/etc/bind/named.conf.options` dengan menambahkan `allow-query{any;};`.
- Lalu, edit file `/etc/bind/named.conf.local` menjadi seperti dibawah ini :

<img src="https://user-images.githubusercontent.com/61219556/99142602-22bb3000-2689-11eb-8f9d-1c9c971e570b.PNG" width="500" height="auto">

- Kemudian buat direktori dengan nama **delegasi**
- Copy **db.local** ke direktori **delegasi** dan edit namanya menjadi `gunung.semerub05.pw`
```
mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/gunung.semerub05.pw
```
- Kemudian, edit file `gunung.semerub05.pw` hingga menjadi seperti di bawah ini :

<img src="https://user-images.githubusercontent.com/61219556/99142873-b261de00-268b-11eb-9e52-730be8938972.PNG" width="500" height="auto">

- Restart bind9
- Lakukan testing pada UML Client GRESIK dengan `ping gunung.semerub05.pw` dan `ping naik.gunung.semerub05.pw`

<img src="https://user-images.githubusercontent.com/61219556/99142919-0c62a380-268c-11eb-881d-5cbc9ac2e84e.PNG" width="500" height="auto">

### 8. Membuat domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw 
- Buka UML PROBOLINGGO, pindah ke dircetory `/etc/apache2/sites-available`.
- Copy file **default** menjadi file **semerub05.pw**.
- Buka file **semerub05.pw** , lalu tambahkan 
```
ServerName semerub05.pw
ServerAlias semerub05.pw
```
- Ubah DocumentRoot menjadi /var/www/semerub05.pw

<img src="https://user-images.githubusercontent.com/61219556/99143152-d9b9aa80-268d-11eb-9a0f-1ca85d13ace5.PNG" width="500" height="auto">

- Gunakan perintah `a2ensite semerub05.pw`
- Gunakan perintah `service apache2 restart`

<img src="https://user-images.githubusercontent.com/61219556/99143193-184f6500-268e-11eb-87a1-053c4fa9771d.PNG" width="500" height="auto">

- Pindah ke directory `/var/www`
- Download folder dengan perintah `wget 10.151.36.202/semeru.pw.zip`
- Unzip folder dengan perintah `unzip [nama file]` dan ubah namanya menjadi **semerub05.pw**
- Buka browser dan akses http://semerub05.pw

<img src="https://user-images.githubusercontent.com/61219556/99143249-957ada00-268e-11eb-8e1f-b999262eec3c.PNG" width="500" height="auto">

### 9. Diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.
- Jalankan perintah `a2enmod rewrite` untuk mengaktifkan `module rewrite`.
- Restart apache dengan perintah `service apache2 restart`
- Pindah ke directory `/var/www/jarkom2020.com` dan buat file **.htaccess** dengan isi file :
```
 RewriteEngine On
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^([^\.]+)$ $1.php [NC,L]
 ```
 - Pindah ke directory `/etc/apache2/sites-available` kemudian buka file **semerub05.pw** dan tambahkan :
```
<Directory /var/www/semerub05.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
 </Directory>
 ```
 - Restart apache dengan perintah `service apache2 restart`
 - Buka browser dan akses http://semerub05.pw/home
 
 <img src="https://user-images.githubusercontent.com/61219556/99143353-3b2e4900-268f-11eb-87c1-ef5fd878155e.PNG" width="500" height="auto">
 
#### 10. Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur folder sebagai berikut:
/var/www/penanjakan.semeruyyy.pw
                    /public/javascripts
                    /public/css
                    /public/images
                    /errors
#### 11. Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan. 
#### 12. Disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.
#### 13. Dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semeruyyy.pw/js. Untuk web http://gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai. 
#### 14. Web http://naik.gunung.semeruyyy.pw sudah bisa diakses hanya dengan menggunakan port 8888. DocumentRoot web berada pada /var/www/naik.gunung.semeruyyy.pw. 
#### 15. Membuat web http://naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” 
#### 16. Setiap mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http://semeruyyy.pw. 
#### 17. Semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.
