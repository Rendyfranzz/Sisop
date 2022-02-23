# soal-shift-sisop-modul-1-C07-2022

## Anggota Kelompok ##

NRP | NAMA
------------- | -------------
5025201039  | Abd. Wahid
5025201056  | Rendi Dwi Francisko
5025201239  | Kadek Ari Dharmika

## Soal 1
Pada suatu hari, Han dan teman-temannya diberikan tugas untuk mencari foto. Namun, karena laptop teman-temannya rusak ternyata tidak bisa dipakai karena rusak, Han dengan senang hati memperbolehkan teman-temannya untuk meminjam laptopnya. Untuk mempermudah pekerjaan mereka, Han membuat sebuah program.
  a. Han membuat sistem register pada script register.sh dan setiap user yang berhasil didaftarkan disimpan di dalam file ./users/user.txt. Han juga membuat sistem login yang dibuat di script main.sh
  b. Demi menjaga keamanan, input password pada login dan register harus tertutup/hidden dan password yang didaftarkan memiliki kriteria sebagai berikut
    i. Minimal 8 karakter
    ii. Memiliki minimal 1 huruf kapital dan 1 huruf kecil
    iii. Alphanumeric
    iv. Tidak boleh sama dengan username
  c. Setiap percobaan login dan register akan tercatat pada log.txt dengan format : MM/DD/YY hh:mm:ss MESSAGE. Message pada log akan berbeda tergantung aksi yang dilakukan user.
    i. Ketika mencoba register dengan username yang sudah terdaftar, maka message pada log adalah REGISTER: ERROR User already exists
    ii. Ketika percobaan register berhasil, maka message pada log adalah REGISTER : INFO User USERNAME registered successfully
    iii. Ketika user mencoba login namun passwordnya salah, maka message pada log adalah LOGIN: ERROR Failed login attempt on user USERNAME
    iv. Ketika user berhasil login, maka message pada log adalah LOGIN: INFO User USERNAME logged in
   d. Setelah login, user dapat mengetikkan 2 command dengan dokumentasi sebagai berikut :
    i. dl N ( N = Jumlah gambar yang akan didownload) 
      Untuk mendownload gambar dari https://loremflickr.com/320/240 dengan jumlah sesuai dengan yang diinputkan oleh user. Hasil download akan dimasukkan ke dalam folder dengan         format nama YYYY-MM-DD_USERNAME. Gambar-gambar yang didownload juga memiliki format nama PIC_XX, dengan nomor yang berurutan (contoh : PIC_01, PIC_02, dst. ). Setelah             berhasildidownload semua, folder akan otomatis di zip dengan format nama yang sama dengan folder dan dipassword sesuai dengan password user tersebut. Apabila sudah terdapat       file zip dengan nama yang sama, maka file zip yang sudah ada di unzip terlebih dahulu,barulah mulai ditambahkan gambar yang baru, kemudian folder di zip kembali dengan             password sesuai dengan user.
    ii. att
    Menghitung jumlah percobaan login baik yang berhasil maupun tidak dari user yang sedang login saat ini.
    
    
    
    
    
    **inisiasi tempat file user.txt dan log.txt**
``` c
local locUser=/home/rendi/sisop/modul1/users/user.txt
local locLog=/home/rendi/sisop/modul1/log.txt
```
