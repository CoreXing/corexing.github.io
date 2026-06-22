---
title: 藍隊星星的數位鑑識筆記 Day.12 隱寫術 Steganography（四）聲音隱寫之 Audacity
date: 2025-09-26
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天進入到了一個有趣的環節，也是我只有做過一、兩次的題型，希望學會之後就一勞永逸了XD

## Audacity 介紹

一款免費開源的音頻編輯工具，操作簡單且功能十分強大。因為這是一個正規的音頻編輯工具，所以它並非用於隱寫，而是用於分析音檔中是否藏有機密的資訊。

### 安裝步驟

1. 點擊 [Audacity](https://www.audacityteam.org/download/)
2. 根據自己的作業系統下載檔案
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387X5ysSBqAIv.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387X5ysSBqAIv.png)
3. 點下去後，這邊可以選要直接下載還是要透過 Muse Hub 下載，如果你沒有用其他音樂軟體的需求的話可以選擇直接下載就好
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387ZeT6zqDrjM.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387ZeT6zqDrjM.png)
4. 下載好之後，直接開始安裝
    ![https://ithelp.ithome.com.tw/upload/images/20250926/201623872GiZaXVBZV.png](https://ithelp.ithome.com.tw/upload/images/20250926/201623872GiZaXVBZV.png)
5. 啟動 Audacity 就安裝成功囉！
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387Z9cTFzG9in.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387Z9cTFzG9in.png)

## 可能出現的題型

### 頻譜隱寫

頻譜隱寫是將字串隱藏在頻譜中，這類的音訊通常會有一個較明顯的特徵，聽起來會是一段雜音或比較刺耳。

- 頻譜（spectrum）：二維的資訊，橫軸是頻率，縱軸是強度
    1. 選擇範圍（可以 Ctrl+A 或是按住波型拉出一個範圍）
        ![https://ithelp.ithome.com.tw/upload/images/20250926/201623877zioI72ZvJ.png](https://ithelp.ithome.com.tw/upload/images/20250926/201623877zioI72ZvJ.png)
    2. 在工具列表的分析中選擇描繪頻譜
        ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387zOUtPHC1sV.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387zOUtPHC1sV.png)
    3. 接下來就會根據所選的範圍顯示出頻譜出來
        ![https://ithelp.ithome.com.tw/upload/images/20250926/201623879wOb5bvN8U.png](https://ithelp.ithome.com.tw/upload/images/20250926/201623879wOb5bvN8U.png)
- 頻譜圖（spectrogram）：三維的資訊，橫軸是時間，縱軸是頻率，強度用顏色表示
    1. 對波型左半邊的區域點擊右鍵
        ![https://ithelp.ithome.com.tw/upload/images/20250926/201623871ZQJvUhcEF.png](https://ithelp.ithome.com.tw/upload/images/20250926/201623871ZQJvUhcEF.png)
    2. 選擇頻譜圖，就會出現整個音檔的頻譜圖了
        ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387EdVkCOJRfA.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387EdVkCOJRfA.png)
    3. 通常如果有隱藏資訊，在頻譜圖上稍微縮放一下應該就會看到了

### 波型隱寫（摩斯密碼）

波型隱寫是將字串隱藏在波型中，這類的音訊通常你用 Audacity 等工具直接打開觀察波型就會注意到了。

1. Audacity 打開音檔後，預設顯示的就是波型，稍微縮放一下就會看到了
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387L9wox7P1bl.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387L9wox7P1bl.png)
    

### 音頻反轉

音頻反轉就是整個音檔的聲音被反轉了，這類的音訊聽起來會有很怪的違和感。

1. 在工具列表的效果 > 特殊中，可以選擇上下倒轉或是時間反轉（可以都試試看，因為不知道題目會怎麼搞）
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387HsRaJc0cxk.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387HsRaJc0cxk.png)
2. 聲音反轉後，直接播放就會得到機密的訊息了

## 解題練習

歡迎來到快樂的解題練習，大家可以點開連結自己先去試試看，如果真的卡關了，再去看 writeup 喔！（這題最後提到的資訊還需要再處理過，因為這部分不是我筆記中會提到的內容，所以如果真的得到資訊後完全卡住不知道怎麼辦，就大膽的點進去 writeup 吧！因為你已經完成最核心的部分了，你超棒~）

1. [GDSC CTF What Does The Alien Say?](https://github.com/gdsc-apu/gdsc-ctf/tree/main/monthly/march2024/What%20does%20the%20alien%20say) → [writeup](https://medium.com/@erichdryn/what-does-the-alien-say-gdsc-ctf-writeup-4007cc1f4eb4)
    ![https://ithelp.ithome.com.tw/upload/images/20250926/20162387VTVe90uyRU.png](https://ithelp.ithome.com.tw/upload/images/20250926/20162387VTVe90uyRU.png)
    

## 總結

終於填上音檔隱寫這個大坑了！原本是想帶大家做 BTLO 的題目，但那題前面還有硬碟鑑識的部分，所以會放到教完硬碟鑑識後再給大家試試看，現在學會分析音檔後真的覺得那題蠻有趣的。

# 參考資料

- [Muse Hub](https://cn.midifan.com/modulesoftware-detailview-10060.htm)
- [DAY 27 audacity使用後優缺點介紹](https://ithelp.ithome.com.tw/m/articles/10364087)
- [音频隐写](https://ctf-wiki.org/misc/audio/introduction/)
- [ctf工具——Audacity的安装和使用](https://blog.csdn.net/2401_89382898/article/details/146265483)
- [CTF（Capture The Flag）音频隐写进阶](https://zhuanlan.zhihu.com/p/675850084)
- [DAY 34 - 頻譜（spectrum）和頻譜圖（spectrogram）](https://ithelp.ithome.com.tw/articles/10228694)
- [有關如何在 Audacity 中反轉音訊的詳細指南 [2024]](https://www.free-videoconverter.net/zh_tw/audio-editing/reverse-audio-in-audacity/)