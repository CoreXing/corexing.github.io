---
title: 藍隊星星的數位鑑識筆記 Day.21 記憶體鑑識（七）Volatility3 Windows 指令教學
date: 2025-10-05
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

終於把環境安裝完了，現在是指令教學時間！

## Volatility3 Windows 指令教學

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
    vol [plugin] -h
    ```

### 指令參數

- windows.info：查看記憶體中系統的基本資訊
    ```bash
    vol -f [檔案] windows.info
    ```
- windows.pslist：查看開啟中或是運行中的 process
    ```bash
    vol -f [檔案] windows.pslist
    ```
- windows.cmdline.CmdLine：提取在 process 中執行的參數
    ```bash
    vol -f [檔案] windows.cmdline
    ```
- windows.psscan：掃描 process
    ```bash
    vol -f [檔案] windows.psscan
    ```
- windows.filescan：掃描記憶體中的所有檔案(包含檔案在記憶體中的位置)，可使用 `grep` 方便查閱
    ```bash
    vol -f [檔案] windows.filescan
    ```
- windows.dumpfiles：提取出檔案
    - `--pid <PID>`：將整個 process 的檔案 dump 下來
    - `--virtaddr <VIRTADDR>`：從虛擬位址 dump 單獨檔案（_FILE_OBJECT）下來
    - `--physaddr <PHYSADDR>`：從實體位址 dump 單獨檔案（_FILE_OBJECT）下來
    ```bash
    vol -f [檔案] windows.dumpfiles
    ```
- windows.memdump：提取出 process
    ```bash
    vol -f [檔案] windows.memmap ‑‑dump ‑‑pid <PID>
    ```

## 總結

這邊先列了一些可能會用到的指令，明天再來帶題目做練習喔！

# 參考資料

- [Volatility3内存取证工具使用详解](https://blog.csdn.net/Aluxian_/article/details/127064750)
- [Volatility CheatSheet - Windows memdump](https://hackmd.io/@TuX-/BymMpKd0s)
- [Support dumpfiles with offset](https://github.com/volatilityfoundation/volatility3/issues/565)