---
title: 藍隊星星的數位鑑識筆記 Day.17 記憶體鑑識（三）Volatility2 指令教學
date: 2025-10-01
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

是時候來到 Volatility2 的用法教學了，這邊整理了一些我之前在做題目有用到的指令。

## Volatility2 指令

### 指令基本

- 指令格式
    ```bash
    python vol.py -f [檔案] --profile==[操作系統] [參數]
    ```
- 幫助
    ```bash
    python vol.py --help
    ```
- 查看操作系統的設定檔：在分析前，需要先知道設定檔內容（輸出的 profile 可以隨意使用其他中任意一個）
    ```bash
    python vol.py -f test.raw imageinfo
    ```

### 指令參數

- pslist：查看開啟中或是運行中的 process
    ```bash
    python vol.py -f test.raw --profile==[操作系統] pslist
    ```
- cmdscan：查看 Terminal 紀錄(_COMMAND_HISTORY)
    ```bash
    python vol.py -f test.raw --profile==[操作系統] cmdscan
    ```
- consoles：查看 consoles 紀錄(_CONSOLE_INFORMATION)
    ```bash
    python vol.py -f test.raw --profile==[操作系統] consoles
    ```
- cmdline：提取在 process 中執行的參數
    ```bash
    python vol.py -f test.raw --profile==[操作系統] cmdline
    ```
- filescan：掃描記憶體中的所有檔案(包含檔案在記憶體中的位置)，可使用 `grep` 方便查閱
    ```bash
    python vol.py -f test.raw --profile==[操作系統] filescan
    ```
- dumpfiles：提取出檔案
    ```bash
    python vol.py -f test.raw --profile==[操作系統] dumpfiles -Q [偏移量] -D [想儲放的位置]
    ```
- memdump：提取出 process
    ```bash
    python vol.py -f test.raw --profile==[操作系統] dumpfiles -p [pid] -D [想儲放的位置]
    ```
- connections：查看網路連線
    ```bash
    python vol.py -f test.raw --profile==[操作系統] connections
    ```
- hashdump：查看使用者的帳號密碼資料(密碼為 hash 值)
    ```bash
    python vol.py -f test.raw --profile==[操作系統] hashdump
    ```
- lsadump：從註冊表中提取 LSA 密鑰資料
    ```bash
    python vol.py -f test.raw --profile==[操作系統] lasdump
    ```

## 總結

好的，今天學會 Volatility2 怎麼用了，那明天就可以準備來實戰練習囉！

# 參考資料

- [Github Volatility](https://github.com/volatilityfoundation/volatility)
- [内存取证-volatility工具的使用 （史上更全教程，更全命令）](https://blog.csdn.net/m0_68012373/article/details/127419463)