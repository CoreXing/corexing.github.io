---
title: 藍隊星星的數位鑑識筆記 Day.29 硬碟鑑識（五）解題練習
date: 2025-10-13
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天的題目來自 [BTLO](https://blueteamlabs.online/login) 這個藍隊解題網站。

這題可有趣了，它是我在 BTLO 這個網站中所解的第一題，那時解完真的是一個「哇！長知識了」的感覺，而且他的題目其實蠻大雜燴的~所以剛好很適合作為硬碟鑑識的結束。

## 解題練習

### [**Employee of the Year**](https://blueteamlabs.online/home/challenge/employee-of-the-year-df16bc36f3)

![https://ithelp.ithome.com.tw/upload/images/20251013/20162387CfvxIsTDBg.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387CfvxIsTDBg.png)

- 前置
    - 這個網站一題裡面會有很多小題
        ```bash
        1. What is the text written on the recovered gif image?
        2. Submit Flag1
        3. Submit Flag2
        4. Submit Flag3
        5. What is the filesystem of the provided disk image?
        6. What is the original filename of the recovered mp4 file?
        ```
    - 提供了兩個檔案
        - BTLO.txt：普通的題目憑證申明，與題目無關
        - recoverfiles.dd：題目說 John 不小心刪掉了一些重要檔案，而這個檔案就是我們要復原的目標
    - 使用 binwalk 看一下裡面有甚麼，發現裡面有 pdf、zip、圖片等等的檔案
        ```bash
        binwalk recoverfiles.dd
        ```
        
        ```bash
        ## DECIMAL HEXADECIMAL DESCRIPTION
        --------------------------------------------------------------------------------
        1048576       0x100000        Linux EXT filesystem, blocks count: 2304, image size: 2359296, rev 1.0, ext4 filesystem data, UUID=20027131-e907-4aa4-a6c1-bdaa183a183a
        5578752       0x552000        GIF image data, version "89a", 280 x 250
        5677056       0x56A000        PDF document, version: "1.5"
        5677398       0x56A156        Zlib compressed data, default compression
        5677863       0x56A327        Zlib compressed data, default compression
        5688175       0x56CB6F        Zlib compressed data, default compression
        5693440       0x56E000        Zip archive data, at least v2.0 to extract, name: word/numbering.xml
        5693864       0x56E1A8        Zip archive data, at least v2.0 to extract, name: word/settings.xml
        5694444       0x56E3EC        Zip archive data, at least v2.0 to extract, name: word/fontTable.xml
        5694885       0x56E5A5        Zip archive data, at least v2.0 to extract, name: word/styles.xml
        5695743       0x56E8FF        Zip archive data, at least v2.0 to extract, name: word/document.xml
        5696451       0x56EBC3        Zip archive data, at least v2.0 to extract, name: word/_rels/document.xml.rels
        5696766       0x56ECFE        Zip archive data, at least v2.0 to extract, name: _rels/.rels
        5697000       0x56EDE8        Zip archive data, at least v2.0 to extract, name: word/theme/theme1.xml
        5698647       0x56F457        Zip archive data, at least v2.0 to extract, name: [Content_Types].xml
        5699590       0x56F806        End of Zip archive, footer length: 22
        10186752      0x9B7000        PNG image, 1760 x 857, 8-bit/color RGB, non-interlaced
        10186843      0x9B705B        Zlib compressed data, compressed
        ```
    - 用 testdisk 可以看到檔案結構和檔案名稱，但沒辦法還原(因為檔案大小是 0)
        ![https://ithelp.ithome.com.tw/upload/images/20251013/201623875Rjybjmd4M.png](https://ithelp.ithome.com.tw/upload/images/20251013/201623875Rjybjmd4M.png)
- 還原檔案
    - 如果用 foremost 將所有檔案提取出來，發現裡面有很多檔案
        ```bash
        foremost recoverfiles.dd
        ```
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387h4KetowDNv.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387h4KetowDNv.png)
    - 如果用 photorec 將所有檔案提取出來，發現裡面總共有 6 個檔案(這邊可以理解到說 foremost 把 docx 分析成了一堆 xml 檔案)
        ```bash
        photorec recoverfiles.dd
        ```
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387mBb8FO4rSX.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387mBb8FO4rSX.png)
- 第一題：What is the text written on the recovered gif image?
    1. 因為第一題問 gif 檔案，所以直接點開 git 檔，就可以得到答案
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387gHoRCKb2jE.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387gHoRCKb2jE.png)
- 第二題：Submit Flag1
    1. 從 testdisk 可以知道 Flag1 要看 png 檔案，直接點開來就會看到答案
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387i25p56sNIl.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387i25p56sNIl.png)
- 第三題：Submit Flag2
    1. 從 testdisk 可以知道 Flag2 要看 docx 檔案
    2. 最開始我是用 foremost，然後把每個 .xml 都看過，最後在 `document.xml` 中發現 base64 字串
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387FOxJEOayiK.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387FOxJEOayiK.png)
    3. 如果直接 photorec 提取出來的 docx，直接點開就會看到 base64 字串
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387ineN5hztKc.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387ineN5hztKc.png)
    4. 進行解碼就會得到答案
        ![https://ithelp.ithome.com.tw/upload/images/20251013/201623873mBhYZ98Hx.png](https://ithelp.ithome.com.tw/upload/images/20251013/201623873mBhYZ98Hx.png)
- 第四題：Submit Flag3
    1. 從 testdisk 可以知道 Flag3 要看 pdf 檔案
    2. 這邊有嘗試過 binwalk 也沒看它裡面有再藏東西
    3. 這時用到了許久不見的工具 exiftool，看看它的 meatadata，好像看到奇怪的東西了
        ![https://ithelp.ithome.com.tw/upload/images/20251013/201623878xoupq1OjW.png](https://ithelp.ithome.com.tw/upload/images/20251013/201623878xoupq1OjW.png)
    4. 這邊用 strings 也可以找到喔
        ```bash
        strings 00011088.pdf | grep -i "flag"
        ```
    5. 直接上繳答案會說是錯的，仔細觀察了一下會發現它有 % 這個符號，而且格式也跟前面 Flag1 和 Flag2 不一樣，所以猜想是編碼問題，用 [cyberchef](https://gchq.github.io/CyberChef/) 處理一下
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387xvUSqlLyQm.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387xvUSqlLyQm.png)
- 第五題：What is the filesystem of the provided disk image?
    1. 先用 file 看看有沒有資訊，這邊只知道它的 ID 是 0x83，可以確定是 Linux 原生檔案系統 (Linux native filesystem)
        ![https://ithelp.ithome.com.tw/upload/images/20251013/201623870MniKBsHIT.png](https://ithelp.ithome.com.tw/upload/images/20251013/201623870MniKBsHIT.png)
    2. 很高機率會是 ext2、ext3 或 ext4，因為範圍很小所以可以直接猜
    3. 另外也可以使用 cfdick 工具，就會直接告訴你 filesystem
        ```bash
        cfdisk recoverfiles.dd
        ```
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387Skq9YUfFe4.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387Skq9YUfFe4.png)
- 第六題：What is the original filename of the recovered mp4 file?
    1. 從 testdisk 就可以直接知道 mp4 的檔名
        ![https://ithelp.ithome.com.tw/upload/images/20251013/20162387dTSvnj5dXC.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387dTSvnj5dXC.png)

### 補充

cfdisk 是一個在 Linux 系統中用來建立、刪除和修改磁碟裝置上分割區的指令。它提供了一個基於文字的圖形介面來顯示或操作磁碟分割。

![https://ithelp.ithome.com.tw/upload/images/20251013/20162387yjC3JoMGFT.png](https://ithelp.ithome.com.tw/upload/images/20251013/20162387yjC3JoMGFT.png)

## 總結

不知道大家有沒有注意到，我昨天用 autopsy 時就是用這題的檔案下去做示範的，也就是說用 autopsy 基本上這題可以秒殺掉，有興趣的話大家可以自己去玩看看喔~