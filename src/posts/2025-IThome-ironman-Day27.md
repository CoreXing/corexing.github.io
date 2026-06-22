---
title: 藍隊星星的數位鑑識筆記 Day.27 硬碟鑑識（三）foremost、photorec
date: 2025-10-11
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

介紹一些我之前在做硬碟鑑識時有用過的工具。

## foremost

一種 File Carving 工具，針對「原始資料區塊」來還原檔案，從檔案標頭的 Magic Number 來辨識檔案類型，然後從該位置開始抓資料，直到檔案結尾的標記為止。

- 提取檔案：
    ```bash
    foremost <檔案名稱>
    ```

### 補充

在最一開始的時候，我會用 binwalk 搭配 foremost 做使用（當時還不知道 binwalk 也可以提取檔案，而且有時 binwalk 提取出來的格式比較麻煩，還要再處理過）。先用 binwalk 確定裡面有藏檔案，再用 foremost 提取檔案出來，大部分情況下 foremost 提取出來的檔案準確度蠻高的，只是缺點就是因為他是用 magic number 復原的，所以沒辦法復原目錄結構和檔案名稱，另外，有時檔案的類型也會跟原始檔案有落差（像是 docx 可能會被分析成壓縮黨裡面包著一堆 xml）

## photorec

一種 File Carving 工具，原理和 foremost 差不多。自訂性較差、分析速度也較慢，但恢復成功率通常被認為很高，且能處理更多碎片化的檔案。另外，有提供基於文字的使用者介面 (TUI)。

1. 分析檔案：
    ```bash
    photorec <檔案名稱>
    ```
2. 接下來會進到它的指令畫面
    1. 選擇 Disk，按下 Proceed
    2. 選擇分區，按下 Search
    3. 選擇 filesystem type（大部分是選 ext2/ext3）
    4. 選擇分析範圍（理論上 Disk 很大的話選 Whole 會比較久，這邊可以根據需求選就好）
    5. 選擇提取出來的檔案要儲存的位置，按下 C
    6. 複製成功後，可以一直按 q 離開

### 補充

photorec 和 foremost 雖然原理差不多，但實際還原出來的檔案還是有落差的，主要歸結於它們處理檔案簽名 (File Signatures) 和檔案結束 (File End) 的邏輯不同：

1. 檔案簽名（Header/Footer）的定義差異
    - PhotoRec：雖然它有大量的內建支援，但其程式碼中對特定檔案類型（例如某些舊版或不常見的檔案格式）的 Header 和 Footer 定義可能不夠精確或過於嚴格。
    - Foremost：這些工具使用一個可編輯的純文字配置文件（例如 `foremost.conf`）。如果鑑識人員手動調整了配置檔案中的 Header 或 Footer 定義，使其能夠識別 PhotoRec 遺漏的、略微不同或損壞的簽名，那麼 Foremost 就可能成功恢復。
2. 處理「無 Footer 檔案」或「檔案大小限制」的差異
    - Foremost：
        - 最大大小限制 (Max Size Limit)： 可以在配置文件中為每種檔案類型設定一個最大允許大小。一旦找到 Header，它就會繼續讀取直到達到這個最大大小限制，或者直到找到下一個檔案的 Header。
        - 優點： 如果檔案沒有 Footer，但其大小在預期範圍內，Foremost 仍能將其恢復。
    - PhotoRec：
        - PhotoRec 處理無 Footer 檔案的方式可能不同，或者它對下一個 Header 的檢查邏輯可能更為嚴格。如果它沒有檢測到 Footer，並且找不到下一個 Header 作為結束點，它可能會停止恢復或將該檔案標記為無效。
        - 結果： 如果 Foremost 的最大大小限制恰好涵蓋了遺失的檔案，而 PhotoRec 因缺乏 Footer 而停止，Foremost 就會恢復成功。
3. 處理「碎片化檔案」的演算法差異
    - PhotoRec： PhotoRec 通常被認為在處理**碎片化（Non-contiguous）**檔案方面有更複雜的啟發式演算法（Heuristics），它會嘗試將不連續的資料塊重新組合起來。
    - Foremost： 傳統的 Foremost 主要是基於連續塊 (Contiguous Blocks) 的恢復。它找到 Header 後會連續地讀取數據塊，直到 Footer 或大小限制。對於高度碎片化的檔案，Foremost 可能只能恢復第一個（或前幾個）連續的資料塊，而 PhotoRec 可能能多恢復一部分碎片。

## 總結

理論上我現在在解題目，就是把這些工具都試試看，看誰恢復得最好XD。不過明天會教的另一個工具聽說是硬碟鑑識的神，但我沒有用過，希望學會之類真的就一勞永逸了。