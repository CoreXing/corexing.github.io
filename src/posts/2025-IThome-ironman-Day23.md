---
title: 藍隊星星的數位鑑識筆記 Day.23 記憶體鑑識（九）Volatility3 Linux 指令教學
date: 2025-10-07
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

Windows 學完後，輪到 Linux 啦！

## Volatility3 Linux 指令教學

### 指令基本

- 指令格式
    
    ```
    vol -f [檔案] [plugin]
    ```
    
- 幫助
    
    ```
    vol -h
    ```
    
- 參數幫助
    
    ```bash
    vol -f [plugin] -h
    ```
    

### 指令參數

- linux.banners：嘗試尋找記憶體 Linux 系統的核心版本訊息（banner）
    
    ```bash
    vol -f [檔案] linux.banners
    ```
    
- linux.pslist：查看開啟中或是運行中的 process
    
    ```bash
    vol -f [檔案] linux.pslist
    ```
    
- linux.bash：查看在 Bash Shell 中運行的命令。
    
    ```bash
    vol -f [檔案] linux.bash
    ```
    
- linux.psscan：掃描 process
    
    ```bash
    vol -f [檔案] linux.psscan
    ```
    
- linux.pagecache.Files：掃描記憶體中的所有檔案(包含檔案在記憶體中的位置)，可使用 `grep` 方便查閱
    
    ```bash
    vol -f [檔案] windows.filescan
    ```
    
- linux.pagecache.InodePages：列出並復原檔案
    - `-find <FIND PATH>` ：根據路徑找到檔案
    - `--inode <INODE>`：找到位於 Inode address 的檔案
    - `--dump` ：提取檔案
    
    ```bash
    vol -f [檔案] linux.pagecache.InodePages
    ```
    

## 總結

在網路上 Linux 和 Mac 記憶體的相關資料比較難找到，大部分的題目都還是以 Windows 為主，希望今天這篇 Linux 相關的指令文章可以幫助到大家，若未來有知道或用到更多指令會再慢慢更新上來~如果有甚麼好用的指令，也歡迎大家提供給我喔！

~~如果哪天我成為盤子買到 Mac 電腦，我再來寫 Mac 的指令教學。~~

# 參考資料

- [Github Volatility3](https://github.com/volatilityfoundation/volatility3)
- [Linux Tutorial](https://volatility3.readthedocs.io/en/v2.7.0/getting-started-linux-tutorial.html)
- [Cannot dump a file with linux.pagecache.InodePages command](https://github.com/volatilityfoundation/volatility3/issues/1360)
- [volatility3.plugins.linux.pagecache module](https://volatility3.readthedocs.io/en/latest/volatility3.plugins.linux.pagecache.html)