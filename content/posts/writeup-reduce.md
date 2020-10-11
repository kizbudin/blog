---
title: "Write up Reduce CTF"
date: 2020-10-10T12:58:10+07:00
draft: false
author: fallcrescent
cover: images/hexer/hexer.png
---
## Hexer
diberikan clue sebagai berikut
>Hi I'm hexer and i will hex you  
49 6d 20 68 65 78 65 72


seperti pada judul soal ini merupakan bilangan hexadesimal, bisa langsung decode dengan menggunakan Python dan di dapatkan flag:
![flag_Hexer](/images/hexer/flag.png)
**Flag : REDUCE{Im hexerS}**

## kode standar amerika
diberikan kode sebagai berikut
>65 109 101 114 105 99 97 110 32 83 116 97 110 100 97 114 100 32 67 111 100 101 32 102 111 114 32 73 110 102 111 114 109 97 116 105 111 110 32 73 110 116 101 114 99 104 97 110 103 101

selanjutnya kita bisa langsung mengkonversikan kode ascii tersebut ke dalam bentuk plain text dengan menggunakan tools online
![ascii](/images/ascii/ascii.png)
**Flag : REDUCE{American Standard Code for Information Interchange}**

## Spam Banget
>w dapet pesan nih dari sang heker, namanya cesar chip8. Katanya sih ada flagnya di dalem situ
[sini](https://pastebin.com/MzA4daX5)

setelah di buka ternyata itu adalah string yang sudah di enkripsi menggunakan caesar cipher, untuk itu saya menggunakan bantuan [website ini](https://cryptii.com/pipes/caesar-cipher) untuk melakukan decode, setelah di decode ternyata kita mendapatkan pesan spam yang tidak ada artinya, tapi tenang saja dengan menggunakan [tools ini](https://www.spammimic.com/decode.shtml) di dapatkan flag.  
**Flag: REDUCE{Fl4g_1s_Fl46}**  

to be continued..