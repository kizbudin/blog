---
title: "Write Up Diskoctf"
date: 2020-10-10T23:23:47+07:00
draft: false
author: fallcrescent
cover: /images/diskoctf/logokominfo.png
disqusShortname: fallcrescent
---
## Reverse Engineering
### Expr or Call
Berikut adalah challengenya
![Expr or call](/images/diskoctf/expr-chall-desc.png)
>Adakah jalan (commands) menuju bendera diantara alat dynamic debuggers ?  P.S: Akan sangat disayangkan bila soal ini di-solve menggunakan Decompiler , such waste time :)

buka menggunakan gdb lalu pasang breakpoint pada fungsi main
```
gdb -q main
break main
```
jalankan kembali programnya dengan menggunkaan perintah r pada gdb, selanjutnya masukan perintah jump pada fungsi flag, dan di dapatkan flagnya
![flag-expr](/images/diskoctf/flag-expr.png)

**Flag: DiskoCTF{when_there's_symbols_why_should_disassemble_it?}**