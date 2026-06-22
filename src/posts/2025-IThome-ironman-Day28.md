---
title: 藍隊星星的數位鑑識筆記 Day.28 硬碟鑑識（四）Autopsy
date: 2025-10-12
description: 本篇文章主要會介紹各種我所知道在做數位鑑識題目時可能會用到的工具，介紹完工具之後也會做一些題目。
tags: [IThome ironman, Blue Team]
categories: [Security]
draft: false
---

# 正文

## 前言

今天介紹一個我從來沒用過但聽說超級好用的工具。

## Autopsy

一款開源的數位鑑識工具，提供 GUI 圖形和 CLI 指令列的兩種介面，可根據不同需求做使用。主要功能是對不同型態的磁碟或檔案系統進行、分析，包括磁碟映像檔、虛擬機、邏輯檔案、行動裝置等等。

### 安裝步驟

1. 點擊 [Autopsy](https://www.autopsy.com/download/)
2. 根據自己的作業系統下載檔案（我用 Windows 做示範）
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387WQvimymb2T.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387WQvimymb2T.png)
3. 直接點開 .msi 檔案，進行安裝
4. 打開工具，看到畫面就是安裝成功了喔~
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387N8nlyLfzQQ.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387N8nlyLfzQQ.png)

### 使用介紹

1. 新增 New Case
2. 命名 Case 並選擇路徑
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387HzezTjQVwb.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387HzezTjQVwb.png)
3. Optional Information 可以選填，Case Number 就是案件編號、Examiner 就是檢查員的個人資訊
    ![https://ithelp.ithome.com.tw/upload/images/20251012/201623879VAWciiWHI.png](https://ithelp.ithome.com.tw/upload/images/20251012/201623879VAWciiWHI.png)
4. 按下 Finish 就會開始建立 Case
5. Select Host，每個資料來源一定會跟主機有關，所以在這邊添加主機
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387smUzcV19LG.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387smUzcV19LG.png)
6. Select Data Source Type，選擇資料類型，那做硬碟鑑識就選 Disk Image or VM File
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387sclxs0Exak.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387sclxs0Exak.png)
7. Select Data Source，選擇要分析的檔案，那 Sector Size 的部分我就讓它自動偵測
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387OmPP9L1L1S.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387OmPP9L1L1S.png)
8. Configure Ingest，選擇要攝取出來的資料，這邊我就不動預設
    ![https://ithelp.ithome.com.tw/upload/images/20251012/201623874xxEKImlLY.png](https://ithelp.ithome.com.tw/upload/images/20251012/201623874xxEKImlLY.png)
9. 按下 Next 和 Finish 後就開始分析了
10. 看到左邊的工作表，主要有五個部分
    - Data Sources：所有資料都會依照我們一般在主機的檔案總管中看到的方式進行組織，所以理論上可以從這邊看到整個的目錄結構。
    - File Views：根據檔案類型、狀態、大小進行組織，在這裡就可以直接看到所有的檔案。
    - Results：這是 Ingest Module 結果將出現的位置。
    - Tags：將顯示已標記的文件或結果（[tag](https://sleuthkit.org/autopsy/docs/user-docs/4.12.0/tagging_page.html)）。
    - Reports：將顯示由模塊或分析師生成的報告（[reports](https://sleuthkit.org/autopsy/docs/user-docs/4.12.0/reporting_page.html)），當人工分析完後，可以在 Tools > Generate Report 生成報告。
11. 有個貼心的設計是當你查看文件時，他有個視窗可以預覽檔案
    ![https://ithelp.ithome.com.tw/upload/images/20251012/201623878OV7Me8L5X.png](https://ithelp.ithome.com.tw/upload/images/20251012/201623878OV7Me8L5X.png)
12. 也可以直接對著檔案點擊右鍵，選擇 Extract File(s) 把檔案擷取下來
    ![https://ithelp.ithome.com.tw/upload/images/20251012/20162387arhcfl11jh.png](https://ithelp.ithome.com.tw/upload/images/20251012/20162387arhcfl11jh.png)

## 總結

好的，以上是跟鬼一樣的 Autopsy 工具教學。必須得說，我打開之後真的嚇到了，如果直接用這個工具去解題，應該真的會有很多簡單的題目直接被秒殺。

那今天終於把硬碟鑑識的工具學完啦！明天又是快樂的解題時間。

By the way 參考資料裡的 TryHackMe: Autopsy 這篇文章寫的超級詳細，推薦想了解更多的人可以去看看。

# 參考資料

- [Autopsy鑑識磁碟映像 分析模組萃取確切跡證](https://www.netadmin.com.tw/netadmin/zh-tw/technology/51ED251D5ABF480A9F8112A67D41797C)
- [Autopsy数字取证软件的下载安装（Windows）](https://blog.csdn.net/qq_75023818/article/details/136192707)
- [TryHackMe: Autopsy](https://medium.com/@enyel.salas84/tryhackme-autopsy-walkthrough-598bbaabe664)