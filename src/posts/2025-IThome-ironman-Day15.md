---
title: 藍隊星星的數位鑑識筆記 Day.15 記憶體鑑識（一）介紹
date: 2025-09-29
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

如果今天駭客在進行攻擊結束後，想要隱藏蹤跡，就會進行目標證據檔案的硬碟清除，可能會格式化硬碟多次來洗掉整個犯案軌跡，這時記憶體鑑識就派上用場了！

## 記憶體鑑識介紹

記憶體鑑識是一種數位鑑識技術，主要針對揮發性記憶體（Volatile Memory）做證據擷取。

- 主要用途：駭客攻擊調查、惡意軟體偵測、還原特定類型的資料
- 常見的副檔名：`.raw`、`.bin`、`.mem`、`.dmp`
- 工具：
    - Volatility（免費）
    - AccessData FTK Imager（免費）
    - Bulk Extractor（免費）

## 總結

以上是簡單的記憶體鑑識，明天開始會有一系列的 Volatility 工具教學！

# 參考資料

- [Day 4 數位鑑識 分析記憶體檔案](https://vocus.cc/article/6418218efd89780001eaf064)