---
title: 藍隊星星的數位鑑識筆記 Day.18 記憶體鑑識（四）Volatility2 解題練習
date: 2025-10-02
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天的題目來自於 [MemLabs](https://github.com/stuxnet999/MemLabs/tree/master)，這是一個用於教學的 CTF 挑戰，旨在鼓勵學生、安全研究人員以及 CTF 玩家入門記憶體鑑識領域。

## 解題練習

### [MemLabs Lab 0 - Never Too Late Mister](https://github.com/stuxnet999/MemLabs/blob/master/Lab%200/README.md)

![https://ithelp.ithome.com.tw/upload/images/20251002/20162387YgB4nnsYxk.png](https://ithelp.ithome.com.tw/upload/images/20251002/20162387YgB4nnsYxk.png)
- 這題在 README.md 裡面有寫 writeup，那我這邊就不再另外寫囉~推薦大家可以自己去看看。
- 補充說明，這題最後一個部份要 Hash Cracking，但這個 Hash 在原本的資料庫裡面好像被刪除了，所以現在沒辦法 Crack 出來 flag。（如果有其他工具可以 crack 出來再麻煩留言跟我說~）
    ![https://ithelp.ithome.com.tw/upload/images/20251002/20162387o9RuY8mpeB.png](https://ithelp.ithome.com.tw/upload/images/20251002/20162387o9RuY8mpeB.png)

### [MemLabs Lab 1 - Beginner's Luck](https://github.com/stuxnet999/MemLabs/blob/master/Lab%201/README.md)

![https://ithelp.ithome.com.tw/upload/images/20251002/20162387B0Uza2TFjp.png](https://ithelp.ithome.com.tw/upload/images/20251002/20162387B0Uza2TFjp.png)
1. 這題稍微節錄幾個重點
    1. get all her important files
    2. a black window pop up with some thing being executed
    3. When the crash happened, she was trying to **draw something**
    4. 3 flags
2. 取得設定檔內容
    ```bash
    python vol.py -f MemoryDump_Lab1.raw imageinfo
    ```
    ![r1xRSdB91l.png](https://hackmd.io/_uploads/r1xRSdB91l.png)
3. 查看開啟中或是運行中的 process
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 pslist
    ```
    ![HykOUdr91l.png](https://hackmd.io/_uploads/HykOUdr91l.png)
4. 我問了 ChatGPT 這些 process 裡面有哪些是較為可疑的，以下是他的回答，而我接下來會針對這些 process 逐一檢查
    - cmd.exe
    - DumpIt.exe (這是用來 dump 映像檔的工具，所以她不可疑)
    - WinRAR.exe
    - mspaint.exe
    - SearchIndexer.exe
    - SearchProtocolHost.exe
5. 首先是 cmd.exe，照著 Lab0 的作法，先看一下 Terminal 紀錄，可以看到 Cmd #0 有個 `St4G3$1`，這不是 Windows 內建的指令
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 cmdscan
    ```
    ![BkLrtdB9Jg.png](https://hackmd.io/_uploads/BkLrtdB9Jg.png)
6. 再看一下 console 紀錄
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles
    ```
    ![B117iOrqye.png](https://hackmd.io/_uploads/B117iOrqye.png)
7. 在裡面看到了一組 base64 的字串
    ![rJJIiOSq1l.png](https://hackmd.io/_uploads/rJJIiOSq1l.png)
8. 直接拿去 [base64 解碼](https://www.base64decode.org/)，就得到了第一個 flag
    ```bash
    flag{th1s_1s_th3_1st_st4g3!!}
    ```
9. 第二個檢查 WinRAR.exe，直接用 ChatGPT 給的指令下去查看看
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 cmdline | grep -i winrar
    ```
    ![r1-tpnL5yg.png](https://hackmd.io/_uploads/r1-tpnL5yg.png)
10. 試著用 filescan 查剛剛看到的檔案
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 filescan | grep Important.rar
    ```
    ![ryeMypI9Je.png](https://hackmd.io/_uploads/ryeMypI9Je.png)
11. 先用 dumpfiles 看第一個檔案
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000003fa3ebc0 -D ./
    ```
12. 查了一下文章發現 dumpfiles 是把檔案轉儲到自己的電腦上，所以打開目錄，目錄中出現了一個 file.None.0xfffffa8001034450.dat
13. 把檔案改名並且轉換成正確的檔案格式
    ![SkIQGpI9yx.png](https://hackmd.io/_uploads/SkIQGpI9yx.png)
14. 用 rar 指令解壓縮檔案的時候發現這個壓縮檔需要密碼，同時附帶了一個 comment
    ![By-dB6Icke.png](https://hackmd.io/_uploads/By-dB6Icke.png)
15. 聽 ChatGPT 說這次用 hashdump 看看系統的帳號 Hash
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 hashdump
    ```
    ![rJSvYT8c1g.png](https://hackmd.io/_uploads/rJSvYT8c1g.png)
16. 看到了一串跟 Alissa 有關的 hash 值，將 f4ff64c8baac57d22f22edc681055ba6 直接輸入進去發現不對！？再看了一次上面的提示發現要轉成大寫 F4FF64C8BAAC57D22F22EDC681055BA6，再次輸入就成功了
    ![rJBfs6LcJl.png](https://hackmd.io/_uploads/rJBfs6LcJl.png)
17. 再來是檢查第四個 mspaint.exe，上網查了一下這個就是小畫家，跟題目「Crash 發生時，她正試著畫點什麼。」好像有點關聯，照著剛剛轉儲 important.rar 的方式把 mspaint.exe 轉儲下來後，發現開不起來
18. 題目說「When the crash happened, she was trying to draw something.」，當時因為還在畫，檔案可能沒有被儲存，所以 dumpfile 下來才會沒辦法執行
    - dumpfiles：取出記憶體中暫存的檔案
    - memdump：取出 process 當下的執行狀態
19. 改用 memdump，把 mspaint.exe 當時的執行狀態 dump 下來(2424 是剛剛 `pslist` 時獲得的)
    ```bash
    python vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 memdump -p 2424 -D .
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251002/20162387GdbCat3zLx.png](https://ithelp.ithome.com.tw/upload/images/20251002/20162387GdbCat3zLx.png)
20. 為了能正確的開啟 2424.dmp，我用 file 看了一下他的檔案格式，發現是 data，所以在改副檔名後嘗試執行看看，但還是因為格式無法執行
21. 因為這檔案理論上是圖片，所以我找到了一篇[文章](https://worktile.com/kb/ask/317583.html)上面都是與圖片相關的指令，就每個都試試看
    - `eog 2424.data`：無法載入圖片
    - `display 2424.data`：no decode delegate for this image format
    - `convert 2424.data 2424.jpg`：no decode delegate for this image format、no images defined `2424.jpg`
    - `identify 2424.data`：no decode delegate for this image format
    - `exiftool 2424.data`：看起來沒有重要資訊
    - `mogrify -format png 2424.data`：no decode delegate for this image format
    - `gimp 2424.data`：圖片好像被成功打開了，但甚麼都沒看到
        ![SyaFcXv51x.png](https://hackmd.io/_uploads/SyaFcXv51x.png)
22. 嘗試調整高度和寬度就出現了類似 flag 的字
    ![Sy2b3Xwq1g.png](https://hackmd.io/_uploads/Sy2b3Xwq1g.png)
23. 把現在的畫面截圖下來丟到小畫家翻轉圖片，就會得到 flag 了
    ![image](https://hackmd.io/_uploads/rkkYnmvckl.png)

## 總結

到這邊大家應該已經完成 Lab0 和 Lab1 了，這個 MemLabs 可以說是我的記憶體鑑識啟蒙，題目設計得非常好，真的很推薦大家試著把剩下的題目都做看看！

# 參考資料

- [MemLabs Writeup](https://hackmd.io/9u88tDegS_2f2lI2ssl2Ag#Lab1-Beginners-Luck)