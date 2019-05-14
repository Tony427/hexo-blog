---
title: Win10解壓縮亂碼
date: 2019-05-14 17:29:32
tags: win10
categories: Sofwtare
thumbnail: https://images.unsplash.com/photo-1516572980581-c9bd8bff75b0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80
banner:
---
# Win10 美版安裝中文語言包的雷
<!-- more -->
## 問題
目前是 `Dell XPS 9370` 美版整新機，因此出廠就是英文介面。本來是打算直接用英文介面，電腦拿去給公司資訊部設定AD，還有灌資安軟體，回來被直接安裝了中文語言包 :joy:

但總覺得使用上偶爾還是卡卡的，列一下遇到的雷。
- 先是遇到了 ***cmder*** 大家都是用`cp65001`的`utf-8`才能正常顯示游標以及中文。結果發現自己的要用`cp950`的`Big5`編碼，才不會有游標多一格以及亂跑的問題。
- 接下來發現別人傳給我的zip檔，內部的檔案中文會變成亂碼，也是去改 ***WinRAR*** 的編碼才能正常顯示。
![](https://i.imgur.com/tntQA5e.png)
![](https://i.imgur.com/X24tpqn.png)

嘗試過網路上將語言、地區等等都設成台灣，一樣無法解決。

## 結論
應該是系統不知道為何，預設編碼是`Big5`。

目前還找不到解法可以將系統預設改為`UTF-8`編碼。