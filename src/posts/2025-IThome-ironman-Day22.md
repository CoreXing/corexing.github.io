---
title: 藍隊星星的數位鑑識筆記 Day.22 記憶體鑑識（八）Volatility3 Windows 解題練習
date: 2025-10-06
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天的題目來自於 [MemLabs](https://github.com/stuxnet999/MemLabs/tree/master)，這是一個用於教學的 CTF 挑戰，旨在鼓勵學生、安全研究人員以及 CTF 玩家入門記憶體鑑識領域。

那網路上大部分找到的 MemLabs 都是用 Volatility2 來做解題的，今天我們用 Volatility3 解看看~ 

## 解題練習

### [**MemLabs Lab 3 - The Evil's Den**](https://github.com/stuxnet999/MemLabs/tree/master/Lab%203)

![https://ithelp.ithome.com.tw/upload/images/20251006/20162387puISCBa1eh.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387puISCBa1eh.png)

1. 這題重點有
    - **還原重要資訊**
    - flag 被拆成兩個部分
2. 首先用 `pslist` 看看有那些 process 正在執行中，可以注意到 notepad.exe 這個程式
    ```bash
    vol -f .\MemoryDump_Lab3.raw windows.pslist
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251006/201623876bQXYBeeKL.png](https://ithelp.ithome.com.tw/upload/images/20251006/201623876bQXYBeeKL.png)
3. 用 `cmdline` 看 process 執行的參數，在桌面上有一個 python 檔和一個文字檔
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387bCZP0f2p3p.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387bCZP0f2p3p.png)
4. 接下來就是要想辦法把它們 dump 下來，不過發生了一個問題，用 `filescan` 居然找不到這兩個檔案
    ```bash
    vol -f .\MemoryDump_Lab3.raw windows.filescan | grep evilscript.py
    vol -f .\MemoryDump_Lab3.raw windows.filescan | grep vip.txt
    ```
5. 如果這題用 Volatility2 是可以正常的得到檔案的偏移量，然後 dump 下來，但用 Volatility3 不行，為甚麼呢？以下是 ChatGPT 給出的解答，那簡單來說就是 Volatility3 相對於 Volatility2 更保守一點，可能會找不到已關閉或被刪除但仍殘留在記憶體中的檔案。
    - Volatility2 的 `filescan` 是用 signature-based carving（基於結構體特徵值的掃描），直接在整個記憶體影像中搜尋 FILE_OBJECT 的特徵（magic 值 + 偏移結構對齊），因此，即使該物件已經被解除映射（unlinked / freed），只要記憶體還留有痕跡，它還是能掃到殘留的 FILE_OBJECT。
    - Volatility3 的 `windows.filescan` 則多依賴 Windows kernel object table 與更嚴謹的物件驗證邏輯（解析 metadata），只列出「目前仍被 kernel 管理的合法 FILE_OBJECT」。
6. 這邊我試了蠻多辦法的，以下是我最後得出的結論，可能有點通靈。用 `memmap` 直接把整個 process dump 下來
    ```bash
    vol -f .\MemoryDump_Lab3.raw windows.memmap --pid 3432
    ```
7. 接著把內容用 Strings 轉換成 .txt 檔案
    ```bash
    strings -a pid.3432.dmp > 3432.txt
    ```
8. 嘗試搜尋 evilscript.py 和 vip.txt，可以注意到在搜尋 vip.txt 時出現了一句 python 語句
    ```bash
    strings -a pid.3432.dmp | grep -i "evilscript.py"
    strings -a .\dump\pid.3432.dmp | grep -i "vip.txt"
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387upckTImFGs.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387upckTImFGs.png)
9. 用這個線索在去搜尋，就可以找到完整的程式碼，那大意就是會把輸入做 xor 和 base64 然後輸出存進 vip.txt 裡面
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387qA1WUff4G9.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387qA1WUff4G9.png)
10. 根據以上線索我們可以知道 vip.txt 裡面存著一個 base64 的字串，那我們在想辦法搜尋一下，就可以找到字串（這邊我沒有一個很好的搜尋辦法，大家可以問問 ChatGPT5 之類的，歡迎留言提供好方法給我）
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387d6Jl4AXBcd.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387d6Jl4AXBcd.png)
11. 接下來事情就簡單了，撰寫一個解碼的程式
    ```python
    import string
    import base64
    
    def xor(s):
        a = ''.join(chr(ord(i)^3) for i in s)
        return a
    
    def decoder(x):
        base64_bytes = x.encode("ascii")
        sample_string_bytes = base64.b64decode(base64_bytes)
        return sample_string_bytes.decode("ascii")
    
    if __name__ == "__main__":
        c = "am1gd2V4M20wXGs3b2U="
        c = xor(decoder(c))
        print(c)
    ```
12. 執行程式碼，就看到了 flag 的前半段
    ![image](https://hackmd.io/_uploads/SkanxPOqyg.png)
13. 剩下 flag 的後半部了，因為題目有提示需要用到隱寫術，所以盲猜後半部的 flag 藏在圖片裡面，用 filescan 掃描所有圖片相關的檔案
    ```bash
    vol -f MemoryDump_Lab3.raw windows.filescan | grep -i ".png\|.jpg\|.jpeg\|.bmp"
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387DoDA63NtNx.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387DoDA63NtNx.png)
14. 檔案很多，但裡面有一個檔案叫 suspision1.jpeg，檔名有點像懷疑(suspicion)，所以先把它 dump 下來
    ```bash
    vol -f MemoryDump_Lab3.raw windows.dumpfile --physaddr 0x4f34148
    ```
    ![https://ithelp.ithome.com.tw/upload/images/20251006/20162387KYyqjWXTyn.png](https://ithelp.ithome.com.tw/upload/images/20251006/20162387KYyqjWXTyn.png)
15. 直接改掉附檔名，點開來是一張普通的照片
    ![image](https://hackmd.io/_uploads/HkEerDdckg.png)
16. 先看看圖片的資訊，發現需要密碼
    ```bash
    steghide info suspision1.jpeg
    ```
    ![image](https://hackmd.io/_uploads/ryIxLv_5ye.png)
17. 題目提示有說到需要 flag 的前半部才能得到後半部，所以直接輸入 flag 的前半部
    ```bash
    steghide info ../suspision1.jpeg -p inctf{0n3_h4lf
    ```
18. 裡面藏著一個祕密的文字檔，把它提取出來看就可以拿到後半部的 flag 了
    ```bash
    steghide extract -sf ../suspision1.jpeg -p inctf{0n3_h4lf
    ```
    ![image](https://hackmd.io/_uploads/SkxfwPOcJx.png)

## 總結

今天在 dumpfile 那邊卡了好久，原來 Volatility2 和 Volatility3 的 filescan 作法不一樣，所以才會說這兩個版本都建議下載來用，有些情況 2 比較好，但有些情況 3 比較好，不過那個部分也可以用 Volatility2 先掃描出他的位址後，再用 Volatility3 dump 下來。

希望今天大家都有學廢喔！

# 參考資料

- [Volatility3 Exercise — MemLabs Lab 1](https://mikellebandin.medium.com/volatility3-exercise-memlabs-lab-1-9c28245b710d)