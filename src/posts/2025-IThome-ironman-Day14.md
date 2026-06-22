---
title: 藍隊星星的數位鑑識筆記 Day.14 隱寫術 Steganography（六）解題練習
date: 2025-09-28
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

## 解題練習

### [picoCTF hideme](https://play.picoctf.org/practice/challenge/350?page=1&tag=32)

![https://ithelp.ithome.com.tw/upload/images/20250928/20162387eEHfAuFk1z.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387eEHfAuFk1z.png)

1. 直接使用 `binwalk` 檢查一下檔案有沒有藏東西，可以注意到裡面有 zip 檔案
    ![https://ithelp.ithome.com.tw/upload/images/20250928/20162387fHWKtrgDVr.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387fHWKtrgDVr.png)
2. 用 `binwalk -e` 把檔案提取出來
    ![https://ithelp.ithome.com.tw/upload/images/20250928/201623876GQwdEDj0q.png](https://ithelp.ithome.com.tw/upload/images/20250928/201623876GQwdEDj0q.png)
3. 打開資料夾，可以看到裡面有一個 secret 的目錄
    ![https://ithelp.ithome.com.tw/upload/images/20250928/20162387DvKdlpInj3.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387DvKdlpInj3.png)
4. 點進去後會看到一個 flag.png
    ![https://ithelp.ithome.com.tw/upload/images/20250928/20162387K2UTA8Qqho.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387K2UTA8Qqho.png)

### [picoCTF **St3g0**](https://play.picoctf.org/practice/challenge/305?page=1&tag=32)

![https://ithelp.ithome.com.tw/upload/images/20250928/20162387nlXJuDSSSI.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387nlXJuDSSSI.png)

1. 直接使用 `binwalk` 檢查一下檔案有沒有藏東西，但好像沒什麼資訊
    ![https://ithelp.ithome.com.tw/upload/images/20250928/20162387c164YKh4Mt.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387c164YKh4Mt.png)
2. 因為這個是 png 檔案，沒辦法使用 `steghide`，改用另一個圖片隱寫工具 `zsteg` 
    ![https://ithelp.ithome.com.tw/upload/images/20250928/20162387ddG4r5p1ui.png](https://ithelp.ithome.com.tw/upload/images/20250928/20162387ddG4r5p1ui.png)
3. 出乎意料地 flag 直接就出現了XD

## 總結

今天因為搬家的關係，有點太累了，就先練習到這邊~順便又學會了新工具 zsteg，有興趣的朋友們可以到參考資料的 github 連結裡面查看更多關於 zsteg 這個工具的內容喔！

# 參考資料

- [Github zsteg](https://github.com/zed-0xff/zsteg)