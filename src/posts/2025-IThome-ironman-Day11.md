---
title: 藍隊星星的數位鑑識筆記 Day.11 隱寫術 Steganography（三）聲音隱寫
date: 2025-09-25
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天先來學學一個酷酷的 DeepSound 工具！這工具真的非常簡單友善，愛了。

## DeepSound

一種隱寫技術（Steganography）的工具和音頻轉換器，它將機密資訊隱藏到音頻文件中。此工具能夠直接從音檔或音軌中提取機密資訊。

### 安裝步驟

1. 點擊 [DeepSound](https://github.com/Jpinsoft/DeepSound/releases/tag/v2.2.2404.04)
2. 下載 DeepSound_2_2_2404_14_Setup.msi
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387Ip7tmr5CGO.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387Ip7tmr5CGO.png)
3. 點擊 .msi 檔案進行安裝
4. 開啟程式，出現這個畫面就是安裝成功了
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387EdJ64YZUXr.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387EdJ64YZUXr.png)

### 嵌入檔案

1. 點擊上面功能欄的 Hide Data Inside Audio
2. 點擊 Open carrier files 選擇表面的音檔
3. 可根據需求選擇輸出的品質，品質越低可以放的檔案大小就越大
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387TViovpCETK.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387TViovpCETK.png)
4. 點擊 Add secret files 選擇要隱藏的資料（可放多的檔案）
5. 如果有不小心放錯檔案，可以對檔案右鍵直接刪除
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387aJBhkXIPdS.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387aJBhkXIPdS.png)
6. 點擊 Encode secret files 按鈕
7. 接著你可以選擇輸出類型、路徑以及是否要密碼
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387D9nUXc6gkk.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387D9nUXc6gkk.png)
8. 點擊 Encode secret files
9. 出現這個畫面代表成功了
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387xxrtMV3mxk.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387xxrtMV3mxk.png)

### 提取檔案

1. 點擊上面功能欄的 Hide Data Inside Audio
2. 點擊 Open carrier files 選擇目標音檔
3. 如果裡面有偷藏東西，就會直接顯示在下面
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387Lgld8vuyVG.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387Lgld8vuyVG.png)
4. 點擊 Extract secret files 按鈕
5. 出現這個畫面代表成功了
    ![https://ithelp.ithome.com.tw/upload/images/20250925/20162387MT8kQltUP9.png](https://ithelp.ithome.com.tw/upload/images/20250925/20162387MT8kQltUP9.png)

## 工具練習

歡迎來到快樂的工具練習，本環節只是提供檔案讓大家可以練習看看工具。flag format: Star{……..}
1. [audiocleaner_20250925_160148_file.wav](https://drive.google.com/file/d/1yMhlNvxC5b33ng8cT-OirevHRDWR47I_/view?usp=sharing)

## 總結

先跟大家說聲抱歉，原本計畫今天要發文件隱寫的文章，但我試了 StegoStick、StegJ，各自都發生了奇怪的問題 QQ，我真的很努力了，也調整過很多個 Java 版本，但我猜真的是這兩個工具太舊了（前一次更新的時間都是十多年前了），所以問題有點多，而且實際上去查一些 Writeup 好像也沒有甚麼人會用這兩個工具。

綜合以上原因，我稍微調整了隱寫術系列的內容，接下來幾天會以聲音隱寫為主（除非工具又出現其他問題了OAO），慢慢地介紹各種相關工具以及可能出現的狀況，這一塊真的是我的知識盲區，所以還請大家鞭小力一點。

# 參考資料

~~這工具介面簡單到不需要看別人的教學~~