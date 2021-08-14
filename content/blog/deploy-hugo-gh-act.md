---
title: "Deploy Hugo ke Github dengan Menggunakan Github Action"
image: "images/post/hugo+gh.png"
author : "kizbudin"
tags: ["Hugo","github action"]
categories: ["Web Dev"]
date: 2021-08-14T22:26:56+07:00
draft: false
---

## Apa itu Github Action?
Sederhananya Github Actions adalah alat otomasi (CI/CD) yang hadir di Github, Alat ini sangat membantu kita untuk membuat berbagai keperluan otomasi seperti unit test auto deploy . Dan tidak hanya itu, dengan Github Actions kita juga bisa menentukan saat saat apa saja otomasi akan terpanggil.

## Langkah untuk Deploy Hugo Website dengan menggunakan Github Action

### Step 1: Buat repository dengan nama ```<username>.github.io``` 
kita perlu membuat sebuah repository dengan nama <username>.github.io. repository ini selanjutnya akan digunakan agar bisa di akses dengan menggunakan url dengan nama url yang sama dengan nama repository nya.
### Step 2: Install dan buat Hugo Website 
Kita perlu install hugo pada sistem operasi yang sedang digunakan agar bisa membuat website hugo. silahkan lihat [dokumentasi berikut](https://gohugo.io/getting-started/quick-start/) untuk melakukan installasi dan panduan dasar dalam pembuatan website hugo.

### Step 3: Push Website Hugo ke dalam repository Github
Selanjutnya, buat lagi repository yang digunakan untuk menyimpan source code berisikan website Hugo lalu push source code yang sudah ada ke dalam branch main. push source code dengan menggunakan perintah berikut\
```bash
git init
git remote add origin git@github.com:<username>/<repo-name>
git add --all
git commit -m "Pesan Commit"
git push origin master
```
jika sudah menambahkan tema pada website hugo, tambahkan folder tema tersebut pada git submodule. ikuti perintah berikut
```bash
git submodule add <remote URL> themes/<nama tema>
```
sebelum melakukan push terhadap repository, lebih baik update submode untuk mendapatkan tema terbaru dari creator tema tersebut. ikuti perintah berikut\
```bash
git submodule update --init --recursive
git push origin master
```
atau nanti kita bisa melakukan update tema terbaru dengan menggunakan perintah pada github action.

### Step 4: Buat Github Token
Selanjutnya kita perlu membuat token yang dapat mengakses repsository yang telah ada dengan menggunakan [Halaman Github Token berikut](https://github.com/settings/tokens/new). kita akan mendapatkan sebuah character random berisikan token yang sudah di generate oleh sistem. copy dan simpan terlebih dahulu token yang sudah di dapat
![Github token](/images/post/hugogh/buat-token.jpg)

### Step 5: Tambahkan token sebagai secret pada github repo
Selanjutnya, kita bisa menambahkan pada Secret dalam setting pada repo, secret dapat di akses pada ```https://github.com/<username>/<repo-name>/settings/secrets``` seperti berikut
![tambah token](/images/post/hugogh/tambah-token.jpg)\

### Step 6: Buat Github action
Setelah itu, buat sebuah action pada direktori .github/workflows/ direktori tersebut berada dalam repository tempat source code hugo tersimpan, namakan file tersebut dengan nama main.yml. lalu isikan dengan kode berikut
```
name: CI
on: push
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Git checkout
        uses: actions/checkout@v2

      - name: Update theme
        # ini adalah opsional, jika kita menggunakan tema sebagai submodule, perintah ini akan melakukan update tema ke versi paling terbaru yang di release oleh developer tema tersebut
        run: git submodule update --init --recursive

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.64.0"

      - name: Build
        # Hapus parameter --minify jika kalian tidak membutuhkan minify
        # dokumentasi minify dapat dilihat pada: https://gohugo.io/hugo-pipes/minification/
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.TOKEN }}
          external_repository: <username>/<username>.github.io
          publish_dir: ./public
          #   keep_files: true
          user_name: <username>
          user_email: <username@email.com>
          publish_branch: master
```
Seperti yang sudah di tuliskan pada kode diatas perintah-perintah yang ada diatas akan otomatis di eksekusi dengan menggunakan VPS berbasis ubuntu-18.04 untuk menjalankan pipeline.

### Step 7: Push ke dalam github repo
Setelah selesai kita hanya perlu melakukan push pada repository github dan semuanya akan tereksekusi, untuk prosesnya dapat di akses pada tab action
![action](/images/post/hugogh/act.jpg)

### Kesimpulan
Kita dapat melakukan update pada website hugo yang sudah di hosting sebelumnya dengan menggunakan github pages, sekarang kita dapat mempublikasikan tulisan yang sudah di tulis tanpa perlu ribet melakukan push ke dalam 2 repository yang di gunakan untuk menyimpan dan hosting hugo website. berkat dengan github action ini publikasi tulisan dengan menggunakan hugo lebih mudah.