---
title: "Write up HackFest 0x02"
image: "images/post/hf-ccug.png"
author : "kizbudin"
tags: ["CTF"]
categories: ["CTF"]
date: 2019-01-27T21:22:22+07:00
draft: false
---
## Cryptography
### One Byte XOR
diberikan file ZIP berisi source code python encryptor dan hasil enkripsinya berikut adalah scriptnya
```python
from Crypto import Random
import base64

def main():
 flag = open("flag.txt","rb").read()
 key = Random.new().read(1)
 res = ""
 for i in range(len(flag)):
     res += chr(ord(flag[i]) ^ ord(key))
 res = base64.b64encode(res)
 open("encrypted","wb").write(res)

if __name__ == "__main__":
 main()
 ```
 Fungsi enkirptor diatas adalah meng-XOR setiap character dengan 1 byte key random, lalu hasilnya di encode ke base64. Untuk menyelesaikan challenge ini, penulis menggunakan Brute Force untuk angka dari 0 sampai 256 berikut adalah script solvernya
```python

from Crypto import Random
import base64

flag = open("flag.txt","rb").read()
flag = base64.b64decode(flag)

def xorString(string,key):
    res = ""
    for i in range(len(string)):
     res += chr(ord(flag[i]) ^ key)
    return res

for j in range(0,256):
    resu = xorString(flag,j)
    if "HackFest" in resu:
     print j,resu
```
Setelah di run maka di dapatkan output :
Selamat, ini **flagnya : HackFest{ketika_ragu_dan_galau_brute_force_aja}**

### Cryptizi
Diberikan file zip berisi source code python berupa enkriptor, berikut adalah Scriptnya
```python
def main():
    print("#[][][]ENKRIPTOR mini " )
    x = raw_input("\n\ntYPE : \n")
    x = duniasakti(x)
    print("\n\n {}RESULT :\n" + x)
    return 0

def duniasakti(yy):
    x = list(yy)
    a = len(x) - 1
    n = a/2
    for i in range(n,a+1):
     temp = x[a-i]
     x[a-i] = x[i]
     x[i]   = temp
    a = a + 1
    v = a * 3
    for h in range(a):
     temp = x[h]
     for j in range(256):
      karakter = chr(j)
      if(karakter==temp):
       x[h] = str(j*v)
       break
     continue
    x = "Un%".join(x)
    x = list(x)
    z = list()
    yuyu = ['1','2','3','4']
    kangkang = ['!*','x','_','q']
    for i in x:
     c = i in yuyu
     if(c == True):
      i = kangkang[yuyu.index(i)]
     z.append(i)
    z = ''.join(z)
    return(z)

if __name__ == '__main__':
    main()
```
enkripsi ini merubah text asli (Plain text) menjadi list dan merubahnya dengan sebuah inyeger lalu di gabung dengan "Un5%" dan merubah angka menjadi !*,x,_,q secara berurutan, berikut adalah solvernya
```python
cipher = open("xxcheap.enc","rb").read()

cipher = cipher.replace("!*","1")
cipher = cipher.replace("x","2")
cipher = cipher.replace("_","3")
cipher = cipher.replace("q","4")
cipher = cipher.replace("Un%"," ")
print cipher

a = 33
v = a * 3
x = [12375, 10395, 11484, 11286, 9999, 10197, 10890, 9999, 10791, 10395, 9900, 9405, 11484, 10395, 10692, 11583, 11385, 9405, 9603, 10395, 11385, 11583, 10890, 9603, 10791, 9405, 10197, 10890, 9603, 9900, 9603, 10593, 12177]
t = ""

print v

for i in range(33):
    h = x[i]
    y = h/v
    t += chr(y)

print(t[::-1])
```
maka akan keluar output
```
12375 10395 11484 11286 9999 10197 10890 9999 10791 10395 9900 9405 11484 10395 10692 11583 11385 9405 9603 10395 11385 11583 10890 9603 10791 9405 10197 10890 9603 9900 9603 10593 12177
99
{kadang_manusia_sulit_dimengerti}
```
**Flag : HackFest{kadang_manusia_sulit_dimengerti}**

## Reverse Engineering
### Ular Terbungkus
Diberikan file dengan ekstensi file python yang sudah di compile (.pyc) dan ketika di run maka akan menampilkan :
```
----- Sarang Ular -----
Flag :
```
untuk menyelesaikan challenge ini, penulis terlebih dahulu mendecompyle dengan bantuan Uncompyle6, dan saat di run akan tampil :
```bash
┌─[fallcrescent@parrot]─[~/Hackfest/Crypto/Multibyte_XOR_Source]
└──╼ $ uncompyle6 ular_terbungkus.pyc
# uncompyle6 version 3.2.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.15+ (default, Nov 28 2018, 16:27:22)
# [GCC 8.2.0]
# Embedded file name: ular_terbungkus.py
# Compiled at: 2018-12-12 08:33:32
from Crypto.Cipher import AES
import hashlib, base64

class Ular:

 def __init__(self):
     self.ular_flag = 'Gda+wKa52rT/1WaWzVXDVi23VemIwUNTpttG4JJddYGHDF4eSGj2/9tCZs8OTwzaizI75GTfxS/QGDKEVm9XBz2zM/WQGrM7gqDNfx3SCpVKtaENNnnYOZHwk8Lpo1wt'

 def unpad(self, s):
     return s[:-ord(s[-1])]

 def decrypt(self, s):
     cip = base64.b64decode(s)
     key = hashlib.md5(cip[:16]).hexdigest()
     IV = cip[-16:]
     cipher = cip[16:-16]
     aes_obj = AES.new(key, AES.MODE_CBC, IV)
     return self.unpad(aes_obj.decrypt(cipher))

 def get_flag(self):
     return self.decrypt(self.ular_flag)


def main():
 print '----- Sarang Ular -----'
 inp_flag = raw_input('Flag : ')
 real_flag = Ular().get_flag()
 if inp_flag == real_flag:
     print '[+] Correct'
 else:
     print '[x] Wrong'


if __name__ == '__main__':
 main()
# okay decompiling ular_terbungkus.pyc
```
dari script diatas penulis hanya membenarkan penulisan script dengan menghapous bagian if dan input lalu menampilkan fungsi real_flag
```python
def main():
 print '----- Sarang Ular -----'
 real_flag = Ular().get_flag()
 print(real_flag)
```
**Flag : HackFest{88df7b7657844da585f1590c887bbaefdb2685c6}**

