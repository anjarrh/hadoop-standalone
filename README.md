# Hadoop Stand-Alone di Ubuntu 16.04 #
## Penjelasan Singkat : ##
Hadoop terdiri dari empat lapisan utama terdiri dari :
* Hadoop Common : kumpulan utilitas dan pustaka yang mendukung modul Hadoop lainnya
* HDFS (Hadoop Distributed File System) : bertanggung jawab untuk meneruskan data ke disk
* YARN (Yet Another Resource Negotiator) : "sistem operasi" untuk HDFS
* MapReduce : model pemrosesan asli untuk kelompok Hadoop yang mendistribusikan pekerjaan dalam cluster atau peta, kemudian mengatur dan mengurangi hasil dari node menjadi respons terhadap permintaan

## Kebutuhan : ##
* Server Ubuntu 16.04
* OpenJDK 1.8.0_171
* Hadoop 2.8.4

## Instalasi Java ##
* Mengupdate package list
```
sudo apt-get update
```
* Menginstal OpenJDK yang merupakan JDK default pada Ubuntu 16.04
```
sudo apt-get install default-jdk
```
* Mengecek hasil instalasi Java dilihat dari versinya
```
java -version
```
![Versi Java](https://github.com/anjarrh/hadoop-standalone/blob/master/jdkversion.png "JDK Version")

## Instalasi Hadoop ##
* Cek versi Hadoop terbaru yang akan digunakan, pada praktikum ini menggunakan hadoop-2.8.4 di web http://hadoop.apache.org/releases.html
* Menginstal Hadoop 2.8.4
```
wget http://www-eu.apache.org/dist/hadoop/common/hadoop-2.8.4/hadoop-2.8.4.tar.gz
```
* Mengekstrak file hadoop-2.8.4.tar.gz
```
tar -xzvf hadoop-2.8.4.tar.gz
```
* Memindahkan file ekstrak hadoop ke /usr/local/hadoop
```
sudo mv hadoop-2.8.4 /usr/local/hadoop
```
## Konfigurasi JAVA_HOME Hadoop ##
* Hadoop perlu mengatur path Java, baik sebagai variabel environment atau pada file konfigurasi Hadoop
Path Java /usr/bin/java merupakan symlink ke /etc/alternatives/java yang merupakan default Java binary. 
```
readlink -f /usr/bin/java | sed "s:bin/java::"
```
Menggunakan readlink dengan flag -f untuk mengikuti symlink disetiap path secara recursive. Lalu menggunakan sed untuk memotong bin/java dari keluaran agar mengeluarkan nilai yang benar untuk JAVA_HOME.
![Hasilnya Hadoop Java Path](https://github.com/anjarrh/hadoop-standalone/blob/README/hadoopjava.png "Hasil Hadoop Java Path")
* Buka file hadoop-env.sh
```
sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```
Setelah terbuka atur nilai JAVA_HOME nya, ada dua cara yaitu :
1. Menggunakan nilai static, dengan cara ditulis apa adanya
```
...
#export JAVA_HOME=${JAVA_HOME}
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-i386/jre/
... 
```
2. Menggunakan nilai dinamik, dengan readlink
![hadoopdynamicval](https://github.com/anjarrh/hadoop-standalone/blob/README/hadoopdynamicval.png "Hadoop Dynamic Value")

## Jalankan Hadoop ##
* Menjalankan perintah berikut
```
/usr/local/hadoop/bin/hadoop
```
![Hasil Menjalankan Hadoop](https://github.com/anjarrh/hadoop-standalone/blob/README/hadooprun.png "Hasil Menjalankan Hadoop")
* Menjalankan contoh MapReduce
```
mkdir ~/input
cp /usr/local/hadoop/etc/hadoop/*.xml ~/input
```
Jalankan perintah ini untuk mendapatkan hasil keluaran.
```
/usr/local/hadoop/bin/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar grep ~/input ~/grep_example 'principal[.]*'
```
Jika telah berhasil akan terlihat summary proses yang dilakukan.
* Baca isi ~/grep_example
```
cat ~/grep_example/*
```
![Hasil Menjalankan MapReduce](https://github.com/anjarrh/hadoop-standalone/blob/README/hadoopmapreduce1.png "Hasil Menjalankan MapReduce")

![Hasil Menjalankan MapReduce](https://github.com/anjarrh/hadoop-standalone/blob/README/hadoopmapreduce2.png "Hasil Menjalankan MapReduce")

Referensi :
* https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-in-stand-alone-mode-on-ubuntu-16-04
* http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.8.4/hadoop-2.8.4.tar.gz
