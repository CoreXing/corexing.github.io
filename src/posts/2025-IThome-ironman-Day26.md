---
title: 藍隊星星的數位鑑識筆記 Day.26 硬碟鑑識（二）binwalk、testdisk
date: 2025-10-10
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

介紹一些我之前在做硬碟鑑識時有用過的工具。

## binwalk

一種用於分析二進制檔案的工具。它可以識別、提取和分析二進制文件中的內容，包括嵌入在韌體、映像檔、壓縮檔案等中的數據。常用於滲透測試、數位鑑識、韌體分析和反向工程等領域。

- 識別檔案：通常直接輸入 ，就可以看看檔案裡面有沒有甚麼酷東西了
    ```bash
    binwalk <檔案名稱>
    ```
- 提取檔案：
    ```bash
    binwalk -e <檔案名稱>
    ```

### 補充

通常用 binwalk 把檔案提取出來後，會搭配 7z 做使用，因為提取出來的檔案可能會有壓縮檔之類的，而 7z 又是個強大的壓縮工具，所以這時用 7z 就可以很順便地把檔案解壓縮出來。

By the way ~我超級容易忘記 linux 的解壓縮指令怎麼打~

- 壓縮：
    ```bash
    7z a <壓縮後的檔案名稱>.7z <檔案名稱>
    ```
- 解壓縮：
    ```bash
    7z x <要解壓縮的檔案名稱>
    ```

## testdisk

專門用於修復分區表、恢復丟失分區。它會深入分析並嘗試重建檔案系統的 metadata，因此有機會能夠找回檔案的原始檔名與資料夾結構，但如果檔案實際內容已被覆寫，則還原出的檔案會是空的。

1. 分析檔案：
    ```bash
    testdisk <檔案名稱>
    ```
2. 接下來會進到它的指令畫面
    1. 選擇 Disk
    2. 選擇分區類型（通常都選 Intel）
    3. 選擇動作（要分析的話當然是選 Analyse）
    4. 選擇 Quick Search
    5. 選擇分區，按下 P 可以看到檔案的列表
    6. 看到檔案後，可以根據下面這個按鍵操作，把檔案複製出來
        ![https://ithelp.ithome.com.tw/upload/images/20251010/201623873gpktkZvN2.png](https://ithelp.ithome.com.tw/upload/images/20251010/201623873gpktkZvN2.png)
    7. 按下複製後，選擇儲存路徑，再次按下 C
    8. 複製成功後，可以一直按 q 離開

## 總結

testdisk 的好處就是它能夠復原出原始的檔名和目錄結構，但因為它的復原方式的原因，很容易你用下去之後就甚麼檔案都沒有，所以可以把 testdisk 當作一個復原的備案。像是你今天已經用 binwalk 復原成功了，但可能你需要知道原始檔名，就可以用 testdisk 試試看。

# 參考資料

- [GNU / Linux 各種壓縮與解壓縮指令](https://note.drx.tw/2008/04/command.html)
- [TestDisk 教學 (2025更新)：免費強大分割區救援/磁碟修復與資料救援工具](https://jamespolik.pixnet.net/blog/post/347354335)