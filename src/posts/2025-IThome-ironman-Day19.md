---
title: 藍隊星星的數位鑑識筆記 Day.19 記憶體鑑識（五）Volatility3 介紹與安裝
date: 2025-10-03
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

介紹完 Volatility2 後，準備學 Volatility3 怎麼用啦！

## Volatility3 介紹

Volatility 是一個用於事件響應和惡意軟體分析的開源記憶體鑑識框架，能夠從電腦的隨機存取記憶體（RAM）中提取、分析和解釋數位證據。

- 注意：目標主機在做取證前盡量不要切斷電源，以免揮發性記憶體資料消失
- 版本：
    - Volatility2 - 採用 python2 撰寫，是目前的穩定版本（2.6 為最終的版本）
    - Volatility3 - 採用 python3 撰寫，持續更新中
- Volatility3 特點：
    - 採用 **符號表 (Symbol Tables)** 的方式，直接解析系統提供的符號，避免 profile 相依，對 Windows、Linux、MacOS 的支援更靈活。
    - 採用了更現代化的符號解析方式，Volatility 3 在結構解析上比 Volatility 2 更準確，減少誤判。

## 安裝教學

1. 需要先安裝 [python3](https://www.python.org/downloads/)（python3.8 以上的版本）
2. 點擊 [Volatility3](https://github.com/volatilityfoundation/volatility3/releases/tag/v2.26.2)
3. 下載 .whl 檔案
    ![https://ithelp.ithome.com.tw/upload/images/20251003/20162387iZVp4R26kQ.png](https://ithelp.ithome.com.tw/upload/images/20251003/20162387iZVp4R26kQ.png)
4. 使用 pip 進行安裝
    ```bash
    pip3 install .\volatility3-2.26.2-py3-none-any.whl
    ```
5. 驗證是否安裝成功
    ```bash
    vol -h
    ```
6. 出現幫助的內容就是安裝成功囉！
    ![https://ithelp.ithome.com.tw/upload/images/20251003/20162387dsZ4Zf9ruq.png](https://ithelp.ithome.com.tw/upload/images/20251003/20162387dsZ4Zf9ruq.png)

## 總結

今天又安裝了一個工具（我的儲存空間正在哭），如果有需要的話，也可以考慮直接使用 docker 去安裝 Volatility3 喔，在 [Docker Hub](https://hub.docker.com/) 已經有別人包好的 image 可以下載，但裡面似乎還沒有裝好 symbol tables，所以暫時可能還沒辦法用。

那至於 symbol tables 是甚麼呢？我們留到下一篇再給大家娓娓道來。

# 參考資料

- [Github Volatility3](https://github.com/volatilityfoundation/volatility3)
- [How to Install Volatility 2 and Volatility 3 on Windows](https://letsdefend.io/blog/how-to-install-volatility-2-and-volatility-3-on-windows)