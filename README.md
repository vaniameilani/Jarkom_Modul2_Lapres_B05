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

- Server MALANG       = DNS Server Master
- Server MOJOKERTO    = DNS Server Slave
- Server PROBOLINGGO  = Web Server
- Klien GRESIK & Klien SIDOARJO
- Router SURABAYA

**Tahap awal sebelum membuat DNS sesuai dengan modul Pengenalan UML, hanya saja ditambahkan server PROBOLINGGO pada topologi**

<img src="https://user-images.githubusercontent.com/61219556/98794126-42f5af80-243b-11eb-8f91-8aa742186803.PNG" width="500" height="auto">

#### 1. Membuat Domain alamat http://semeruyyy.pw
- Buka UML MALANG dan update package list dengan command 
  `apt-get update`.
- Install aplikasi bind9 pada UML MALANG dengan
   `apt-get install bind9 -y`.
- Buka file dengan perintah `nano /etc/bind/named.conf.local`, lalu isi configurasi domain dengan syntax sebagai berikut :

`zone "semerub05.pw" {
	type master;
	file "/etc/bind/jarkom/semerub05.pw";
};`

<img src="https://user-images.githubusercontent.com/61219556/99037041-7e1bee00-25b5-11eb-8ff5-cbf05829be6c.PNG" width="500" height="auto">

- Buat folder jarkom di dalam /etc/bind dengan perintah 
  `mkdir /etc/bind/jarkom`.
- Copy file **db.local** ke dalam folder **jarkom** dan ubah namanya menjadi **semerub05.pw**.
  `cp /etc/bind/db.local /etc/bind/jarkom/semerub05.pw`.
- Buka file **semerub05.pw** dengan perintah `nano /etc/bind/jarkom/semerub05.pw`. Lalu, edit file dengan memasukkan IP MALANG seperti berikut	:

<img src="https://user-images.githubusercontent.com/61219556/99037445-46fa0c80-25b6-11eb-8899-2341bb7d5b70.PNG" width="500" height="auto">

- Restart bind9 untuk meng-update perubahan dengan perintah `service bind9 restart`.

#### 2. Membuat alias dari http://www.semeruyyy.pw
#### 3. Membuat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO 
#### 4. Reverse domain untuk domain utama
#### 5. Membuat DNS Server Slave pada MOJOKERTO
#### 6. Membuat Subdomain dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO
#### 7. Membuat subdomain dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO
#### 8. Membuat domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw 
#### 9. Diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.
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
