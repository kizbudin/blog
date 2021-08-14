---
title: "Writeup HackFest 2017"
image: "images/post/hf-ccug.png"
author : "kizbudin"
tags: ["CTF"]
categories: ["CTF"]
date: 2018-10-11T18:56:24+07:00
draft: false
---
## Web Hacking
### 1. MR Robot
Deskripsi Soal :
Catch My Robot\
Diberikan tampilan awal sebagai berikut<br>
![deskripsi soal](/images/post/wu-hf2017/robot/1.JPG)\
Dari soal kita sudah bisa mengetahui kalau pada website ini mempunyai file robots.txt sehingga kita bisa membukanya hanya dengan menambahkan /robots.txt pada akhir URL, maka di dapat tampilan berikut\
![petunjuk](/images/post/wu-hf2017/robot/2.JPG)\
sesuai dengan petunjuk yang diberikan maka kita hanya menambahkan /this_is_the_flag.txt pada akhir URL dan didapatkan flag\
![flag-robot](/images/post/wu-hf2017/robot/flag-robot.JPG)\
**Flag: HackFest{RobotsTXT_is_NOT_Secure_Anymore!}**

### 2. Header Information
Untuk mendapatkan flag ini hanya membuka header yang di berikan oleh server, kita dapat melihatnya dengan bantuan browser serta menekan CTRL+SHIFT+I dan klik network atau dapat menggunakan tool Burpsuite\
![header](/images/post/wu-hf2017/header-info/header.JPG)\
**Flag: HackFest{Informasi_Header_Pada_Web}**\

### 3. Google Flag
Deskripsi Soal: Hanya google bot yang dapat mengakses flag.\
![google](/images/post/wu-hf2017/google-bot/1.JPG)\
kita dapat mengubah user agent yang akan kita kirimkan ke server dengan menggunakan extension user agent switcher atau dengan menggunakan burpsuite.\
![ua](/images/post/wu-hf2017/google-bot/2.jpg)\
Jika kita menggunakan extension user agent switcher, maka pilih google bot, dan di dapatkan flag\
![flag](/images/post/wu-hf2017/google-bot/3.jpg)\
**Flag: HackFest{Haloo_Google!}**

### 4. Belanja Akhir tahun
Tampilan awal dari halaman sebagai berikut, dan hanya memiliki uang Rp10 saja. Untuk itu kita dapat memanipulasi dari source secara langsung, Terlihat sebuah inputan hidden yang dapat diduga sebagai value pengiriman uang ke server\
![1](/images/post/wu-hf2017/belanja/1.jpg)\
Kita dapat menggantikan lebih dari kebutuhan yang diperlukan, misalkan dengan angka 90000, dan kirim kembali data tersebut.\
![2](/images/post/wu-hf2017/belanja/2.jpg)\
Maka di dapatkan flag\
**Flag: HackFest{Belanja_Berhasil_Dengan_Manipulasi_Inspeksi_Elemen}**

## Digital Forensics
### Security Wargame
Diberikan file berupa gambar, nah karena ini adalah soal digital Forensics maka kita di wajibkan unttuk mencari informasi dari sebuah gambar tersebut.
![sec wargame](/images/post/wu-hf2017/sec-wargame/ctf_security_wargame.jpg)
Pada Linux Parrot terdapat command-command yang dapat digunakan untuk mencari informasi pada sebuah file gambar diantaranya adalah: binwalk, foremost dan exiftool (install). Dengan mencoba binwalk akan terlihat ada yang mencurigakan yaitu terdapat file zip yang disembunyikan pada sebuah gambar.\
```bash
fallcrescent@ccug Downloads]$ binwalk ctf_security_wargame.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, EXIF standard
12            0xC             TIFF image data, little-endian offset of first image directory: 8
270337        0x42001         Zip archive data, encrypted at least v2.0 to extract, compressed size: 126, uncompressed size: 148, name: flag.txt
270607        0x4210F         End of Zip archive, footer length: 22
```
Gunakan Foremost untuk mengeluarkan zip yang ada pada gambar tersebut, ketika ingin mengextrack file zip diperlukan password.. pertanyaanya adalah apa dan dimana passwordnya berada? Gunakan exiftool untuk melihat detail informasi pada gambar
```bash
[fallcrescent@ccug Security wargame]$ exiftool ctf_security_wargam1e.jpg 
ExifTool Version Number         : 12.25
File Name                       : ctf_security_wargam1e.jpg
File Size                       : 271 KiB
File Modification Date/Time     : 2018:01:18 09:37:28+07:00
File Access Date/Time           : 2018:10:18 13:47:39+07:00
File Inode Change Date/Time     : 2019:11:07 10:14:25+07:00
Image Description               : Capture The Flag : Security Wargame
Orientation                     : Horizontal (normal)
Software                        : Google
Artist                          : HackFest
XP Title                        : Capture The Flag : Security Wargame
XP Comment                      : ZIP Password : ZAKZyJvzFKjv2qEm
XP Author                       : HackFest
XP Keywords                     : HackFest Prelim
XP Subject                      : Forensic : File Carving
```
Extract file ZIP menggunakan Password yang di dapat dari exiftool kemudian di dapatkan Flagnya
**Flag : HackFest{file_carving_adalah_salah_satu_topik_dalam_digital_forensic}**

### Broken ZIP
Diberikan sebuah file zip yang berisi file flag. Terlihat bahwa file tersebut sehat – sehat saja namun sebenarnya filenya sakit (rusak) karena tidak dapat dibuka ataupun di extract. Untuk mengetahui dimana rusaknya biasanya terdapat pada bagian header filenya. Buka dengan hexeditor atau editor hex lainnya untuk melihat dimana kerusakannya.<br>
![broken](/images/post/wu-hf2017/broken-zip/brokem.JPG)<br>
Terlihat bahwa didalam zip tersebut terdapat file flag.png. Pada hex yang terseleksi(berwarna hijau) seharusnya berisi panjang dari nama file yang ada pada zip tersebut (flag.png → len = 8). Maka pada bit tersebut harus di ubah menjadi hex panjang file yang sebenarnya yaitu 08. Kemudian extract zipnya dan dapatkan flag.png nya.\

Belum selesai sampai disitu ternyata flagnya berupa QR Code. Buka Web ini untuk mendecode online QR Code, atau bisa menggunakan QRCode Reader yang ada pada handphone anda.\

**Flag : HackFest{congratulations_you_have_repaired_this_broken_ZIP}**