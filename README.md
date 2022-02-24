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
    
    
# Pada register.sh    
inisiasi tempat file user.txt dan log.txt    
    
``` 
local locUser=/home/rendi/sisop/modul1/users/user.txt
local locLog=/home/rendi/sisop/modul1/log.txt
```
Kemudian tanggal dan jamnya dicatat
```
calendar=$(date +%D)
time=$(date +%T)
```
Lalu user memasukan input username serta password
```
printf "Enter your username: "
read username

printf "Enter yout password: "
read -s password
```

Setelah itu user dan password divalidasi dengan fungsi "func_check_password",dalam fungsi tersebut username dan password dicek agar sesuai dengan yang diminta pada soal

Cek apakah user sudah dibuat
```
 if grep -q $username "$locUser"
    then
        existsUser="User already exists"

        echo $existsUser
        echo $calendar $time REGISTER:ERROR $existsUser >> $locLog
```
jika username sudah ada pada $locUser maka username tersebut tidak valid sehingga tidak masuk sebagai username baru

Cek apakah password sama dengan username
```
 elif [[ $password == $username ]]
    then
        echo "Password cannot be the same as username"
```
jika password sama dengan username yang dimasukan maka username tersebut tidak valid 

Cek apakah password kurang dari 8 karakter
```
elif [[ $lengthPassword -lt 8 ]]
    then
        echo "Password must be more than 8 characters"
```
jika password kurang dari 8 karakter maka password tersebut tidak dapat digunakan sebagai password yang valid

Cek apakah password terdapat huruf kapital,huruf kecil,dan angka
```
elif [[ "$password" != *[[:upper:]]* || "$password" != *[[:lower:]]* || "$password" != *[0-9]* ]]
    then
        echo "Password must be at least upper, lower and number!"
```
Jika password tidak terdapat minimal satu huruf kapital,huruf kecil,dan angka maka password tersebut tidak valid

Username dan password valid
```
else
        echo "Register successfull!"
        echo REGISTER:INFO User $username registered successfully >> $locLog
        echo $calendar $time $username $password >> $locUser
```
Jika telah memenuhi semua kondisi maka user tersebut valid sehingga dapat dimasukan ke dalam $locUser

# Pada main.sh
pada main.sh terdapat 2 command yaitu att dan dl yang dimana command att digunakan untuk menghitung percobaan login dan command dl digunakan untuk download gambar pada web https://loremflickr.com/320/240.

Inisiasi directory untuk menyimpan file hasil download
```
folder=$(date +%Y-%m-%d)_$username
locLog=/home/rendi/sisop/modul1/log.txt
locUser=/home/rendi/sisop/modul1/users/user.txt
```
nama folser tersebut adalah YYYY-MM-DD_username

Seperti yang terdapat pada register.sh,tanggal dan waktunya dicatat menggunakan
```
calendar=$(date +%D)
time=$(date +%T)
```
Setelah itu user memasukan username dan password untuk login
```
printf "Enter your username: "
read username

printf "Enter your password: "
read -s password
```
Setelah username dan password dimasukan maka akan dicek dengan fungsi "func_check_password"
```
func_check_password(){
        local lengthPassword=${#password}

        if [[ $lengthPassword -lt 8 ]]
        then
                echo "Password must be more than 8 characters"

        elif [[ "$password" != *[[:upper:]]* || "$password" != *[[:lower:]]* || "$password" != *[0-9]* ]]
        then
                echo "Password must be at least upper, lower and number!"

        else
                func_login
        fi
}
```
jika password kurang dari 8 karakter serta tidak memiliki minimal satu hurud kapital,huruf kecil,dan angka maka login akan gagal,jika tidak maka login berhasil dan akan masuk ke dalam fungsi "func_login"
```
func_login(){
        if grep -q $password "$locUser"
        then
                echo "$calendar $time LOGIN:INFO User $username logged in" >> $locLog
                echo "Login success"

                printf "Enter command [dl or att]: "
                read command
                if [[ $command == att ]]
                then
                        func_att
                elif [[ $command == dl ]]
                then
                        func_dl_pic
                else
                        echo "Not found"
                fi

        else
                fail="Failed login attemp on user $username"
                echo $fail

                echo "$calendar $time LOGIN:ERROR $fail" >> $locLog
        fi
}
```
pada fungsi login password akan dicek kembali apakah ada dalam $lockUser jika ada maka login berhasil dan jika tidak ada maka login gagal dan akan dicatat ke dalam $locLog.Pada saat login berhasil maka user akan disuruh memasukan 2 command yaitu att dan dl,jika memasukkan command att maka akan masuk ke dalam fungsi "func_att"
```
func_att(){
        awk '
        BEGIN {print "Count login attemps"}
        /LOGIN/ {++n}
        END {print "Login attemps:", n}' $locLog
}
```
pada fungsi "func_att" akan menampilkan berapa kali percobaan login yang telah dicatat dalam $locLog,jika user memasukan command dl maka akan masuk ke dalam fungsi "func_dl_pic"
```
func_dl_pic(){
        printf "Enter number: "
        read n

        if [[ ! -f "$folder.zip" ]]
        then
                mkdir $folder
                count=0
                func_start_dl
        else
                func_unzip
        fi

}
```
Pada fungsi tersebut user memasukan jumlah gambar yang akan di download lalu dicek apakah $folder.zip sudah ada.$folder.zip merupakan nama file yang sesuai dengan tanggal,jika $folder.zip belum ada maka akan dibuatkan $folder.zip countnya 0 dan akan mulai start download dengan menggunakan fungsi "func_start_dl" 
```
func_start_dl(){
        for(( i=$count+1; i<=$n+$count; i++ ))
        do
                wget https://loremflickr.com/320/240 -O $folder/PIC_$i.jpg
        done

        zip --password $password -r $folder.zip $folder/
        rm -rf $folder
}
```
Pada fungsi tersebut akan mulai dowonload gambar dengan jumlah yang diminta dengan menggunakan wget https://loremflickr.com/320/240 -O $folder/PIC_$i.jpg.Nama filenya .jpgnya sesuai dengan nama folder/PIC_(urutan gambar).jpg lalu foldernya di zip dan diberi password kemudian folder yang tidak memiliki format .zip akan dihapus.
Kembali pada fungsi "func_dl_pic" jika $folder.zip telah ada maka folder tersebut akan di unzip terlebih dahulu dengan menggunakan fungsi "func_unzip"
```
func_unzip(){
        unzip -P $password $folder.zip
        rm $folder.zip

        count=$(find $folder -type f | wc -l)
        func_start_dl
}
```
Pada fungsi tersebut $folder.zip akan di unzip dengan menggunakan password user lalu count akan dihitung dengan menghitung jumlah gambar yang telah di download,count ini digunakan untuk penamaan file gambarnya,lalu akan masuk ke dalam fungsi "func_start_dl".








