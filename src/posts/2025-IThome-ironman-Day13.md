---
title: 藍隊星星的數位鑑識筆記 Day.13 隱寫術 Steganography（五）Steganography in Python
date: 2025-09-27
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

這是我在 Day.11 很崩潰地嘗試各種工具時找到的 python 工具，感覺很有趣所以就想來介紹給大家試試~

## Steganography

一個整合了 WavSteg、LSBSteg、StegDetect 的工具，可以直接在 Terminal 上執行指令或是撰寫 Python 程式然後引用這個函式庫。

記得要先安裝好 [python3](https://www.python.org/downloads/) 的環境喔！

### 安裝步驟

- 用 pip 下載（如果 pip 的環境變數沒有設好可以用第二行的指令
    ```bash
    pip install stego-lsb
    python -m pip install stego-lsb
    ```
- 從 github 下載
    ```bash
    git clone https://github.com/ragibson/Steganography
    cd Steganography
    python setup.py install
    ```

## WavSteg

專門用來隱寫 .wav 檔案的工具。
- 參數：

| 參數 | 英文說明 | 中文說明 |
| --- | --- | --- |
| h, --hide | To hide data in a sound file | 嵌入檔案 |
| -r, --recover | To recover data from a sound file | 提取檔案 |
| -i, --input | Path to a .wav file | 檔案（.wav 檔）的路徑 |
| -s, --secret | Path to a file to hide in the sound file | 機密資訊檔案的路徑 |
| -o, --output | Path to an output file | 輸出檔案的路徑 |
| -n, --lsb-count | How many LSBs to use [default: 2] | 想要使用多少 LSB（預設 2） |
| -b, --bytes | How many bytes to recover from the sound file | 想從音檔中提取多少位元組 |
| --help | Show this message and exit. | 幫助 |

- 嵌入檔案（若機密檔案太大，會提供使用者嵌入該檔案的建議 LSB 數）
    ```bash
    stegolsb wavsteg -h -i <.wav檔案路徑> -s <機密檔案路徑> -o <輸出檔案路徑> -n <LSB 數>
    ```
- 提取檔案
    ```bash
    stegolsb wavsteg -r -i <.wav檔案路徑> -o <輸出檔案路徑> -n <LSB 數> -b <提取出的大小>
    ```

## LSBSteg

用來隱寫圖片檔案的工具。
- 參數：

| 參數 | 英文說明 | 中文說明 |
| --- | --- | --- |
| h, --hide | To hide data in an image file | 嵌入檔案 |
| -r, --recover | To recover data from a image file | 提取檔案 |
| -a, --analyze | Print how much data can be hidden within an image   [default: False] | 輸出該圖片可隱藏的資訊大小（預設不顯示） |
| -i, --input | Path to a bitmap (.bmp or .png) file | 圖片檔案的路徑 |
| -s, --secret | Path to a file to hide in the image file | 機密資訊檔案的路徑 |
| -o, --output | Path to an output file | 輸出檔案的路徑 |
| -n, --lsb-count | How many LSBs to use [default: 2] | 想要使用多少 LSB（預設 2） |
| -c, --compression | 1 (best speed) to 9 (smallest file size)  [default: 1] | 1（最快）到9（最小的檔案容量）（預設 1） |
| --help | Show this message and exit. | 幫助 |

- 嵌入檔案
    ```bash
    stegolsb steglsb -h -i <圖片檔案路徑> -s <機密檔案路徑> -o <輸出檔案路徑> -n <LSB 數> -c <整數範圍>
    ```
- 提取檔案
    ```bash
    stegolsb steglsb -r -i <圖片檔案路徑> -o <輸出檔案路徑> -n <LSB 數>
    ```

## StegDetect

用來偵測圖片隱寫的工具。
- 參數：

| 參數 | 英文說明 | 中文說明 |
| --- | --- | --- |
| -i, --input | To hide data in a image | 圖片檔案的路徑 |
| -n, --lsb-count | How many LSBs to use [default: 2] | 想要使用多少 LSB（預設 2） |
| --help | Show this message and exit. | 幫助 |

## 總結

因為今天有神盾獎又有其他的事情要忙，所以就很快速的和大家介紹這些工具~明天再來帶大家做一些隱寫術的題目喔！敬請期待。

# 參考資料

- [Steganography Github](https://github.com/ragibson/Steganography)
- [Steganography : Tools & Techniques](https://medium.com/@ria.banerjee005/steganography-tools-techniques-bba3f95c7148)
- [隐写术 - 有用工具和资源的列表](https://blog.csdn.net/qq_43500877/article/details/91626379)