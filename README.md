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
    -i. Ketika mencoba register dengan username yang sudah terdaftar, maka message pada log adalah REGISTER: ERROR User already exists
    -ii. Ketika percobaan register berhasil, maka message pada log adalah REGISTER : INFO User USERNAME registered successfully
    -iii. Ketika user mencoba login namun passwordnya salah, maka message pada log adalah LOGIN: ERROR Failed login attempt on user USERNAME
    -iv. Ketika user berhasil login, maka message pada log adalah LOGIN: INFO User USERNAME logged in

d. Setelah login, user dapat mengetikkan 2 command dengan dokumentasi sebagai berikut :
    -i. dl N ( N = Jumlah gambar yang akan didownload) 
        Untuk mendownload gambar dari https://loremflickr.com/320/240 dengan jumlah sesuai dengan yang diinputkan oleh user. Hasil download akan dimasukkan ke dalam folder
        dengan format nama YYYY-MM-DD_USERNAME. Gambar-gambar yang didownload juga memiliki format nama PIC_XX, dengan nomor yang berurutan (contoh : PIC_01, PIC_02, dst. ).
        Setelah berhasildidownload semua, folder akan otomatis di zip dengan format nama yang sama dengan folder dan dipassword sesuai dengan password user tersebut. Apabila
        sudah terdapat file zip dengan nama yang sama, maka file zip yang sudah ada di unzip terlebih dahulu,barulah mulai ditambahkan gambar yang baru, kemudian folder di zip
        kembali dengan password sesuai dengan user.
    -ii. att
        Menghitung jumlah percobaan login baik yang berhasil maupun tidak dari user yang sedang login saat ini. 
    
    
### Pada register.sh    

Mencatat tanggal dan jamnya dicatat
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
```
local lengthPassword=${#password}
local locFolder=/home/wahid/sisop/modul1
local locUsers=$locFolder/users

if [[ ! -d "$locUsers" ]]
    then
        mkdir $locUsers
    fi
```
Pada code diatas akan akan mencari tau panjang password dan lokasi dari folder yang akan digunakan untuk menyipan user yang akan mendaftar, lalu dicek apakah directory sudah ada atau belum, jika tidak ada maka akan dibuat dengan menjalanakan perintah mkdir

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

### Pada main.sh
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
        checkUser=$(egrep $username "$locUser")
        checkPass=$(egrep $password "$locUser")

        if [[ ! -f "$locUser" ]]
        then
                echo "no user registered yet"
        else
                if [[ -n "$checkUser" ]] && [[ -n "$checkPass" ]]
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
        fi
}
```
pada fungsi login akan dicek folder dan file user terlebih dahulu, jika file user.txt ditemukan maka akan mengecek username dan password akan dicek kembali apakah ada dalam $lockUser jika ada maka login berhasil dan jika tidak ada maka login gagal dan akan dicatat ke dalam $locLog.Pada saat login berhasil maka user akan disuruh memasukan 2 command yaitu att dan dl,jika memasukkan command att maka akan masuk ke dalam fungsi "func_att"
```
func_att(){
        awk -v user="$username" 'BEGIN {count=0} $5 == user || $9 == user {count++} END {print (count)}' $locLog
}
```
pada fungsi "func_att" akan menampilkan berapa kali percobaan login yang telah dilakukan oleh user yang login, kemudian akan dicatat dalam $locLog. jika user memasukan command dl maka akan masuk ke dalam fungsi "func_dl_pic"
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
Pada fungsi tersebut user diminta memasukan jumlah gambar yang akan di download lalu dicek apakah $folder.zip sudah ada. $folder.zip merupakan nama file yang sesuai dengan tanggal,jika $folder.zip belum ada maka akan dibuatkan $folder.zip  countnya 0 dan akan mulai start download dengan menggunakan fungsi "func_start_dl" 
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
Pada fungsi tersebut akan mulai download gambar dengan jumlah yang diminta dengan menggunakan wget https://loremflickr.com/320/240 -O $folder/PIC_$i.jpg.Nama filenya .jpgnya sesuai dengan nama folder/PIC_(urutan gambar).jpg lalu foldernya di zip dan diberi password kemudian folder yang tidak memiliki format .zip akan dihapus.
Kembali pada fungsi "func_dl_pic" jika $folder.zip telah ada maka file tersebut akan di unzip terlebih dahulu dengan menggunakan fungsi "func_unzip"
```
func_unzip(){
        unzip -P $password $folder.zip
        rm $folder.zip

        count=$(find $folder -type f | wc -l)
        func_start_dl
}
```
Pada fungsi tersebut $folder.zip akan di unzip dengan menggunakan password user dan file .zip dihapus, lalu count akan dihitung dengan menghitung jumlah gambar yang telah di download, count ini digunakan untuk penamaan file gambarnya, lalu akan masuk ke dalam fungsi "func_start_dl".





## Soal 2
Pada tanggal 22 Januari 2022, website https://daffa.info di hack oleh seseorang yang tidak
bertanggung jawab. Sehingga hari sabtu yang seharusnya hari libur menjadi berantakan.
Dapos langsung membuka log website dan menemukan banyak request yang berbahaya.
Bantulah Dapos untuk membaca log website https://daffa.info

### A
```
locFolder=/home/wahid/sisop/modul1
locLog=$locFolder/log_website_daffainfo.log
folder=$locFolder/forensic_log_website_daffainfo_log

#create dir
if [[ -d "$folder" ]]
then
        rm -rf $folder
        mkdir $folder
else
        mkdir $folder
fi
```

Dapat dilihat, pada locFolder adalah folder untuk menyimpan semua informasi yang nantinya akan dibutuhkan. Pada source code diatas akan dicek terlbih dahulu, apakah folder forensic_log_website_daffainfo_log sudah ada atau tidak, jika ada maka folder yang lama akan dihapus dan akan dibuatkan folder yang baru, jika folder tidak ada maka akan langsung dibuatkan folder baru.

### B
```
cat $locLog | awk -F: '{gsub(/"/, "", $3)
        arr[$3]++}
        END {
                for (i in arr) {
                        count++
                        res+=arr[i]
                }
                res=res/count
                printf "rata rata serangan perjam adalah sebanyak %.3f request per jam\n\n", res
        }' >> $folder/ratarata.txt
```
# Soal 3
Conan adalah seorang detektif terkenal. Suatu hari, Conan menerima beberapa laporan tentang hewan di kebun binatang yang tiba-tiba hilang. Karena jenis-jenis hewan yang hilang banyak, maka perlu melakukan klasifikasi hewan apa saja yang hilang
    a.Untuk mempercepat klasifikasi, Conan diminta membuat program untuk membuat 2 directory di “/home/[USER]/modul2/” dengan nama “darat” lalu 3 detik kemudian membuat directory ke 2 dengan nama “air”.
    b.Kemudian program diminta dapat melakukan extract “animal.zip” di “/home/[USER]/modul2/”.
    c.Tidak hanya itu, hasil extract dipisah menjadi hewan darat dan hewan air sesuai dengan nama filenya. Untuk hewan darat dimasukkan ke folder “/home/[USER]/modul2/darat” dan untuk hewan air dimasukkan ke folder “/home/[USER]/modul2/air”. Rentang pembuatan antara folder darat dengan folder air adalah 3 detik dimana folder darat dibuat terlebih dahulu. Untuk hewan yang tidak ada keterangan air atau darat harus dihapus.
    d.Setelah berhasil memisahkan hewan berdasarkan hewan darat atau hewan air. Dikarenakan jumlah burung yang ada di kebun binatang terlalu banyak, maka pihak kebun binatang harus merelakannya sehingga conan harus menghapus semua burung yang ada di directory “/home/[USER]/modul2/darat”. Hewan burung ditandai dengan adanya “bird” pada nama file.
    e.Terakhir, Conan harus membuat file list.txt di folder “/home/[USER]/modul2/air” dan membuat list nama semua hewan yang ada di directory “/home/[USER]/modul2/air” ke “list.txt” dengan format UID_[UID file permission]_Nama File.[jpg/png] dimana UID adalah user dari file tersebut file permission adalah permission dari file tersebut.
Contoh : conan_rwx_hewan.png

catatan:
-Tidak boleh memakai system().
-Tidak boleh memakai function C mkdir() ataupun rename().
-Gunakan exec dan fork
-Direktori “.” dan “..” tidak termasuk

### A
```
void create_dir(){
    pid_t ch = fork();
    int status;

    if(ch == 0){
        char *argv[] = {"mkdir", "-p", "/home/rendi/modul2/darat", NULL};
        execv("/bin/mkdir", argv);
    }else{
        while((wait(&status)) > 0);
        sleep(3);
        char *argv[] = {"mkdir", "-p", "/home/rendi/modul2/air", NULL};
        execv("/bin/mkdir", argv);
    }
}
```
Membuat directory darat dan air pada /home/rendi/modul2/.Pembuatan directory dimualai dari directory darat terlebih dahulu kemudian tunggu 3 detik setelah itu membuat directory air.Untuk jeda 3 detik digunakan "sleep(3)"

### B
```
void unzip(){

    int status;

    if(fork() == 0){
        char *argv[] = {"unzip", "/home/rendi/sisop/modul2/animal.zip", "-d", "/home/rendi/modul2/", NULL};
        execv("/bin/unzip", argv);
    }
    while((wait(&status)) > 0 );
}
```
File zip berada pada directory "/home/rendi/sisop/modul2/animal.zip" kemudian diunzip ke dalam directory "/home/rendi/modul2/" dengan sebuah perintah "-d" yaitu tempat kemana file akan diunzip.
### C
```
void delete_substr(char *str, char *substr){
    char *comp;
    int png = strlen(substr);
    while((comp = strstr(str, substr))){
        *comp = '\0';
        strcat(str, comp+png);
    }
}
void move_animals(){
    DIR *dir;
    struct dirent *dp;
    
    char from[PATH_MAX];
    char dest[PATH_MAX];

    strcpy(from, "/home/rendi/modul2/animal/");
    dir = opendir(from);

    while((dp = readdir(dir)) != NULL){
        if(strcmp(dp->d_name, ".") != 0 && strcmp(dp->d_name, "..") != 0 ){
            
            char fileName[NAME_MAX];
            strcpy(fileName, dp->d_name);
            delete_substr(fileName, ".jpg");

            strcpy(from, "/home/rendi/modul2/animal/");
            strcat(from, dp->d_name);
            strcpy(dest, "/home/rendi/modul2/");

            pid_t child = fork();
            int status;
            
            char *token;
            token = strtok(fileName, "_");
            int flag = 0;
            while(token != NULL){

                if(flag == 1){
                    if(strcmp(token, "air") == 0){
                        // printf("AIR: %s --> %s\n\n", from, dest);
                        strcat(dest, "air");
                        // printf("air %s --> %s\n", from, dest);

                        if(child == 0){
                            char *argv[] = {"mv", from, dest, NULL};
                            execv("/bin/mv", argv);
                        }
                        while((wait(&status)) > 0);
                    }

                    if(strcmp(token, "darat") == 0 || strcmp(token, "bird") == 0){
                        // printf("DARAT: %s --> %s\n\n", from, dest);
                        strcat(dest, "darat");
                        // printf("darat %s --> %s\n", from, dest);
                        if(child == 0){
                            char *argv[] = {"mv", from, dest, NULL};
                            execv("/bin/mv", argv);
                        }
                        while((wait(&status)) > 0);
                    }
                }

                token = strtok(NULL, "_");
                flag++;
            }

            if(child == 0){
                char *argv[] = {"rm", "-rf", from, NULL};
                execv("/bin/rm", argv);
            }
            while((wait(&status)) > 0);
        }
    }
}
```
Langkah awalnya yaitu menuju directory "/home/rendi/modul2/animal/" kemudian abaikan file "." dan ".." kemudian simpan nama file lalu sesuaikan dengan kriteria nama file yang dimaksud jika nama file ada kata darat maka akan dipindah ke directory "/home/rendi/modul2/animal/darat" jika nama file ada kata air maka file akan dipindahkan ke directory "/home/rendi/modul2/animal/air" dan jika nama file tidak ada kata darat ataupun air maka file tersebut akan dihapus.
### D
```
void delete_bird(){
    DIR *dir;
    struct dirent *dp;
    
    char path[PATH_MAX];

    strcpy(path, "/home/rendi/modul2/darat/");
    dir = opendir(path);

    while((dp = readdir(dir)) != NULL){
        if(strcmp(dp->d_name, ".") != 0 && strcmp(dp->d_name, "..") != 0 ){
            
            strcpy(path, "/home/rendi/modul2/darat/");
            strcat(path, dp->d_name);
            pid_t child = fork();
            int status;

            if(strstr(dp->d_name, "bird") != 0){
                if(child == 0){
                    char *argv[] = {"rm", "-rf", path, NULL};
                    execv("/bin/rm", argv);
                }
                while((wait(&status)) > 0);
            }
        }
    }
}
```
Karena file dengan nama bird ada di folder darat maka buka directory "/home/rendi/modul2/darat/" lalu selama filenya masih ada isinya cek nama file ("." dan ".." diabaikan) jika nama file terdapat kata "bird" maka file akan dihapus.
### E
```
void create_list(){
    FILE *fp;
    DIR *dir;
    struct stat info;
    struct stat fs;
    struct dirent *dp;
    int r;
    int p;

    char path[PATH_MAX];
    char permission[NAME_MAX];
    char list[NAME_MAX];

    strcpy(path, "/home/rendi/modul2/air/");
    strcpy(list, path);
    strcat(list, "list.txt");
    fp = fopen(list, "a");
    dir = opendir(path);

    // permission
    p = stat(path, &fs);
    if (p == -1){
        fprintf(stderr,"File error\n");
        exit(1);
    }

    // owner
    r = stat(path, &info);
    if( r==-1 ){
        fprintf(stderr,"File error\n");
        exit(1);
    }

    while((dp = readdir(dir)) != NULL){
        if(strcmp(dp->d_name, ".") != 0 && strcmp(dp->d_name, "..") != 0 ){
            
            strcat(path, dp->d_name);

            char owner[NAME_MAX];
            char p_r[NAME_MAX];
            char p_w[NAME_MAX];
            char p_x[NAME_MAX];
            
            // owner
            struct passwd *pw = getpwuid(info.st_uid);
            if (pw != 0) strcpy(owner, pw->pw_name); //puts(pw->pw_name);

            // permission
            if (fs.st_mode & S_IRUSR) strcpy(p_r, "r");
            if (fs.st_mode & S_IWUSR) strcpy(p_w, "w");
            if (fs.st_mode & S_IXUSR) strcpy(p_x, "x");

            if(strstr(dp->d_name, "list") == 0){
                fprintf(fp, "%s_%s%s%s_%s\n", owner, p_r, p_w, p_x, dp->d_name);
            }
        }
    }

    fclose(fp);
    return;
}
```
Pertama buka diectory "/home/rendi/modul2/air/" lalu buat file "list.txt" lalu pada directory "/home/rendi/modul2/air/" setiap file akan dicek UID dan file permission lalu akan dituliskan ke dalam file "list.txt" dengan format penamaan yang sudah ditentukan.Untuk menadapatkan UID dilakukan dengan cara berikut
```
 // owner
            struct passwd *pw = getpwuid(info.st_uid);
            if (pw != 0) strcpy(owner, pw->pw_name); //puts(pw->pw_name);
```
Untuk mendapatkan file permission menggunakan cara berikut
```
 // permission
            if (fs.st_mode & S_IRUSR) strcpy(p_r, "r");
            if (fs.st_mode & S_IWUSR) strcpy(p_w, "w");
            if (fs.st_mode & S_IXUSR) strcpy(p_x, "x");
```
Kemudian akan ditulis di "list.txt" dengan cara berikut
```
if(strstr(dp->d_name, "list") == 0){
                fprintf(fp, "%s_%s%s%s_%s\n", owner, p_r, p_w, p_x, dp->d_name);
           }
```


