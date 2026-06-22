---
title: 藍隊星星的數位鑑識筆記 Day.20 記憶體鑑識（六）Volatility3 symbol table
date: 2025-10-04
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

終於到了我當時在研究 Volatility3 時卡最久的 symbol table 了，這部份真的是很困擾，希望本篇文章可以幫助大家快速上手！

如果沒有先理解 symbol table，那在執行程式的時候通常會出現這個問題

![https://ithelp.ithome.com.tw/upload/images/20251004/20162387ZLuL7CqNDI.png](https://ithelp.ithome.com.tw/upload/images/20251004/20162387ZLuL7CqNDI.png)

## symbol table 介紹

在電腦裡程式被編譯成執行檔後，裡面其實還是有「模板的結構」，而這些結構和位置資訊合在一起，就叫做「**符號（Symbol）**」。

- 某個結構體（struct）在記憶體裡的樣子、欄位名稱、偏移量
- 某個函式或變數在程式裡的位置（記憶體位址）

「**符號表（Symbol Table）**」就是這些符號的對照表（像是一本「記憶體地圖說明書」）。

---

Volatility 需要這些符號資訊，才能知道記憶體裡某個位址的資料對應到哪個結構、哪個變數、是哪個程序的一部分。
Volatility3 使用 JSON 格式來儲存這些符號，而這個設計有以下好處：

- 區分不同模組的符號（例如 kernel vs. driver）
- 讀取不同來源的符號（PDB、DWARF…）
- 更容易擴充、跨系統使用

### 補充

Volatility2 的符號資料叫做 profile（整個系統的符號描述包），所有的符號都混在同一個 namespace裡，可能會導致符號衝突等問題。

## 現成的 symbol table

Volatility3 由於他 symbol table 的特性，讓他能夠支援各種平台，直接在網路上也可以看到很多別人已經包好的 symbol table 可以用。

- 官方提供的
    - [Github volatility3](https://github.com/volatilityfoundation/volatility3)
        ![https://ithelp.ithome.com.tw/upload/images/20251004/20162387D9PIF2vboC.png](https://ithelp.ithome.com.tw/upload/images/20251004/20162387D9PIF2vboC.png)
- 別人寫好的
    - [Github Windows symbol table](https://github.com/JPCERTCC/Windows-Symbol-Tables)
    - [Github Linux and macOS table](https://github.com/Abyss-W4tcher/volatility3-symbols)

### 步驟

1. 以官方為例，點擊 [Github volatility3](https://github.com/volatilityfoundation/volatility3)
2. 下載 .zip 檔案，然後解壓縮
3. 把檔案丟進 volatility3/symbols 裡面
    ![https://ithelp.ithome.com.tw/upload/images/20251004/20162387kSxjnJ8XTy.png](https://ithelp.ithome.com.tw/upload/images/20251004/20162387kSxjnJ8XTy.png)
4. 如果是用之前所說的 pip 下載的話，可以使用以下指令來看目錄位置在哪裡
    ```bash
    pip3 show volatility3
    ```

## 自己做 symbol table

如果剛好遇到 Linux 或 Mac 記憶體它需要的 symbol table 都沒有現成的、版本對不上的話，也可以自己客製化一個 symbol table~

### 步驟

1. 先執行 banners，查看 Kernel 版本，從這個版本可以知道它是甚麼作業系統以及版本
    ```bash
    vol -f <記憶體檔案> banners
    ```
2. 接著要下載對應的 Linux Kernel DWARF 檔案，但想下載就要先裝 debug symbol (dbgsym) 套件
    ```bash
    echo "deb http://ddebs.ubuntu.com $(lsb_release -cs) main restricted universe multiverse
    deb http://ddebs.ubuntu.com $(lsb_release -cs)-updates main restricted universe multiverse
    deb http://ddebs.ubuntu.com $(lsb_release -cs)-proposed main restricted universe multiverse" | \
    tee -a /etc/apt/sources.list.d/ddebs.list
    
    apt install ubuntu-dbgsym-keyring
    apt-get update
    ```
3. 裝完套件後，就可以安裝相對應的 Kernel 檔案
    ```bash
    apt install linux-image-5.15.0-138-generic-dbgsym
    ```
4. 獲得這個檔案後，我們要安裝工具把它轉成 JSON 格式，官方推薦使用 `dwarf2json` （這個工具要用 go 去跑）
    ```bash
    git clone https://github.com/volatilityfoundation/dwarf2json.git
    cd dwarf2json/
    sudo apt install golang-go
    go build
    ```
5. 工具安裝好後，開始轉換格式
    ```bash
    ./dwarf2json linux --elf /usr/lib/debug/boot/vmlinux-5.15.0-138-generic > vmlinux-5.15.0-138-generic.json
    ```
6. 最後把這個 json 檔案放進 volatility3/symbols/linux 裡面就好囉！

## 總結

今天內容相當豐富，在客製化 symbol tables 的部分卡超級久，總算是完成了。

# 參考資料

- [Volatility 3 Basics](https://volatility3.readthedocs.io/en/latest/basics.html)
- [Creating New Symbol Tables](https://volatility3.readthedocs.io/en/latest/symbol-tables.html)
- [Day4 如果我獲得到了一個讀心術，我想要分析企鵝先生的記憶](https://ithelp.ithome.com.tw/articles/10382488)
- [Github dwarf2json](https://github.com/volatilityfoundation/dwarf2json)
- [Build a Custom Linux Profile for Volatility3](https://medium.com/@alirezataghikhani1998/build-a-custom-linux-profile-for-volatility3-640afdaf161b)