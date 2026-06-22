---
title: 藍隊星星的數位鑑識筆記 Day.24 記憶體鑑識（十）Volatility3 Linux 解題練習
date: 2025-10-08
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天的題目來自於牛肉湯的 yunshiuan 在 2025 AIS3 CTF 組出的數位鑑識題目，再次感謝他願意提供題目給我做本次的解題練習~大家有興趣的話也可以去 Follow 他今年的鐵人文章喔！

以下是文章連結：[Blue 了 Blue 了！只會看封包與log的我錯了嗎！](https://ithelp.ithome.com.tw/users/20178071/ironman/8336)

## 解題練習

### 2025 AIS3 企鵝先生記憶分析術

![https://ithelp.ithome.com.tw/upload/images/20251008/201623877Xeiwqsb8i.png](https://ithelp.ithome.com.tw/upload/images/20251008/201623877Xeiwqsb8i.png)

1. 這題如果先用 Volatility2 去跑 image，會等超級久！所以需要用 Volatility3
2. 用 `window.info`  跑不出東西來，改用 banners 看看有沒有資訊，可以注意到他是一個 Ubuntu 5.15.0-151.161-generic 的作業系統
    ```bash
    vol -f .\ais3.mem banners
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387nkKzBNHKTZ.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387nkKzBNHKTZ.png)
3. 再用 `linux.pslist`  結果發現缺失 symbol tables
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387mfXA1LUxTM.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387mfXA1LUxTM.png)
4. 使用 Day.20 教過大家的方法自己建一個對應版本的 symbol tables，就可以正常顯示啦
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387vi76ZlUgx6.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387vi76ZlUgx6.png)
5. 先用 `linux.bash` 看一下有甚麼指令紀錄，可以看到上面有三個用 base64 編碼的指令
    ```bash
    vol -f .\ais3.mem linux.bash
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387DfJEpXFxJB.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387DfJEpXFxJB.png)
6. 進行解碼後會看到這些資訊
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387SxcgGdqtdd.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387SxcgGdqtdd.png)
7. 嘗試找找看者兩個酷酷的檔案
    ```bash
    vol -f .\ais3.mem linux.pagecache.Files | grep 'powershell.pyc'
    vol -f .\ais3.mem linux.pagecache.Files | grep 'secret.enc'
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387N4qJQbVnFd.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387N4qJQbVnFd.png)
8. 得到路徑後，就可以嘗試把檔案 dump 下來，然後直接改回正確的檔名
    ```bash
    vol -f .\ais3.mem linux.pagecache.InodePages --find /home/ais3/powershell.pyc --dump
    vol -f .\ais3.mem linux.pagecache.InodePages --find /home/ais3/secret.enc --dump
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387dau6620vfF.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387dau6620vfF.png)
9. 使用[線上反編譯網站](https://pylingual.io/)，把 pyc 處理一下，就可以得到原始碼
    ![https://ithelp.ithome.com.tw/upload/images/20251009/20162387UX0sQ4iXdV.png](https://ithelp.ithome.com.tw/upload/images/20251009/20162387UX0sQ4iXdV.png)
10. 接下來是不快樂的密碼學時間，對不起密碼學我就爛，這部分我還真不知道要怎麼辦，我再去研究研究。

## 總結

再次感謝牛肉湯的大大出了這題並授權我使用，他真的是我 Linux 記憶體鑑識的啟蒙XD，這真的很有趣，但那時卡在 symbol tables 和後面對 Linux 的指令不熟悉也查不到資料真的是受挫了好久，很開心今天可以做這題的 writeup 給大家。