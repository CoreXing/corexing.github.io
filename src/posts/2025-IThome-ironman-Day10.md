---
title: 藍隊星星的數位鑑識筆記 Day.10 隱寫術 Steganography（二）圖片隱寫
date: 2025-09-24
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天會先介紹兩個圖片隱寫的工具，接著直接做題目！

## Steghide

一種隱寫技術(Steganography)的工具，可以將資訊隱藏在圖片或音檔中。
- 嵌入數據：
    ```bash
    steghide embed -cf <圖片檔或音檔名稱> -ef <要隱藏的資訊> -p <密碼>
    ```
- 提取數據：
    ```bash
    steghide extract -sf <檔案名稱> -p <密碼>
    ```
    

## **OpenStego**

主要提供兩個功能，資料隱藏（Data Hiding）以及水印（Watermarking）。

### 安裝步驟

1. 點擊 [openstego](https://github.com/syvaidya/openstego/releases) 進入官方的 github
2. 下載  Setup-OpenStego-0.8.6.exe
3. 直接點擊 .exe 檔案進行安裝（記住安裝路徑）
4. 打開安裝路徑的目錄
    ![https://ithelp.ithome.com.tw/upload/images/20250924/201623876N2KMJDClN.png](https://ithelp.ithome.com.tw/upload/images/20250924/201623876N2KMJDClN.png)
5. 點擊 openstego.bat，程式就會出現囉！
    ![https://ithelp.ithome.com.tw/upload/images/20250924/20162387PFjK24FE6a.png](https://ithelp.ithome.com.tw/upload/images/20250924/20162387PFjK24FE6a.png)

### 嵌入檔案

1. 點擊左邊功能欄的 Hide data
    ![https://ithelp.ithome.com.tw/upload/images/20250924/20162387PFjK24FE6a.png](https://ithelp.ithome.com.tw/upload/images/20250924/20162387PFjK24FE6a.png)
2. 選擇想要隱藏起來的 Message file
3. 選擇表面的 Cover file
4. 選擇輸出檔案的路徑以及名稱
5. 輸入密碼（密碼可以是空白的）
6. 點擊 Hide data 按鈕

### 提取檔案

1. 點擊左邊功能欄的 Extract data
    ![https://ithelp.ithome.com.tw/upload/images/20250924/20162387dxQNKRJnaS.png](https://ithelp.ithome.com.tw/upload/images/20250924/20162387dxQNKRJnaS.png)
2. 選擇想要提取出來的檔案
3. 選擇輸出檔案的路徑
4. 輸入密碼（若密碼是空白的，就不用輸入）
5. 點擊 Extract data 按鈕

## 解題練習

歡迎來到快樂的解題練習，因為找不太到合適的題目，所以這次是我自己出題喔！大家可以點擊連結去下載檔案，我會把 writeup 放在 hackmd 中。flag format: Star{……}
1. [星星出題 Something to say](https://drive.google.com/file/d/1YFV6yqYO6StzHaMisQFihiBwwGpVfzHA/view?usp=sharing)
    ```bash
    可愛的小貓咪有話想跟你說，但牠有點害羞，就把訊息藏在自拍照裡面了。
    ```
2. [星星出題 Dinner](https://drive.google.com/file/d/13fmsSkyDyICkekUa7Jp1oxSGJ3FJ2kp3/view?usp=sharing)
    ```bash
    好餓！好想吃鬆餅喔！順便問問室友要吃甚麼好了，但邪惡的室友又把訊息藏在自拍照裡面了。
    ```
    

## 總結

今天很努力的出了兩題簡單的小題目，如果題目有問題歡迎大家在下面留言告訴我喔！

# 參考資料

- [OpenStego](https://www.openstego.com/)
- [基于LSB的隐写术以及OpenStego的基本操作]([https://cryptoboy.home.blog/2018/11/22/基于lsb的隐写术以及openstego的基本操作/](https://cryptoboy.home.blog/2018/11/22/%E5%9F%BA%E4%BA%8Elsb%E7%9A%84%E9%9A%90%E5%86%99%E6%9C%AF%E4%BB%A5%E5%8F%8Aopenstego%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C/))