---
title: 藍隊星星的數位鑑識筆記 Day.16 記憶體鑑識（二）Volatility2 介紹與安裝
date: 2025-09-30
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

終於進入到了記憶體鑑識的工具教學時間啦！這個工具也是我今年年初再刷題目的時候才學會的，是個很有趣的領域。

## Volatility2 介紹

Volatility 是一個用於事件響應和惡意軟體分析的開源記憶體鑑識框架，能夠從電腦的隨機存取記憶體（RAM）中提取、分析和解釋數位證據。

- 注意：目標主機在做取證前盡量不要切斷電源，以免揮發性記憶體資料消失
- 版本：
    - Volatility2 - 採用 python2 撰寫，是目前的穩定版本（2.6 為最終的版本）
    - Volatility3 - 採用 python3 撰寫，持續更新中

Volatility2 和 3 有點互補的作用，有時 Volatility2 會跑很久，但 Volatility3 一下就出來了；有時 Volatility3 的插件可能還不夠完整，就會需要用到 Volatility2 完整的插件。

## 安裝教學

1. 需要先安裝 [python2](https://www.python.org/downloads/release/python-2718/)
2. 點擊 [Volatility2.6](https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1)
3. 下載 volatility-2.6.zip
    ![https://ithelp.ithome.com.tw/upload/images/20250930/201623877BpRPwenyi.png](https://ithelp.ithome.com.tw/upload/images/20250930/201623877BpRPwenyi.png)
4. 解壓縮後進入到目錄中，安裝好需要的環境
    ```bash
    python2 setup.py install
    ```
5. 執行程式
    ```bash
    python2 vol.py -h
    ```
6. 若能夠正常出現幫助的內容，就代表安裝成功了
    ![https://ithelp.ithome.com.tw/upload/images/20250930/20162387ttkptiYGbM.png](https://ithelp.ithome.com.tw/upload/images/20250930/20162387ttkptiYGbM.png)
7. 中間會跳這個多 Failed 是正常的，可以根據需求下載這些額外的插件

## 總結

今天先開心的把工具準備好了，明天會教大家 Volatility2 的指令用法喔~

# 參考資料

- [Github Volatility](https://github.com/volatilityfoundation/volatility)
- [内存取证-volatility工具的使用 （史上更全教程，更全命令）](https://blog.csdn.net/m0_68012373/article/details/127419463)
- [記憶體分析工具Volatility簡介](https://blog.neithnet.com/4422.html)
- [Day2 撥開電腦的記憶找出最黑暗的部分](https://ithelp.ithome.com.tw/articles/10380401)