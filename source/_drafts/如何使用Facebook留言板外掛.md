---
title: 如何使用Facebook留言板外掛
date: 2017-11-25 12:00:00
tags: 
- Facebook
- plugin
categories: Software
thumbnail: https://images.unsplash.com/photo-1540205441050-9bb1625e6dae?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjF9
---

<!-- more -->

1. 申請Facebook Developer應用程式，獲得一組應用程式ID
![](/assets/img/post/2017112501.jpg)

2. 在自己網站上的layout網頁，在`<body>` tag後嵌入fbsdk。
```javascript=
<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/zh_TW/sdk.js#xfbml=1&version=v2.11&appId=你的應用程式編號';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>
```

3. 在想要產生留言的網頁找出想嵌入留言外掛的html位置，加入以下html碼。
```htmlmixed=
<div class="fb-comments" data-href="要放留言板外掛的網址(目前的頁面)" data-numposts="5"></div>
```

4. 發佈你的應用程式
![](/assets/img/post/2017112503.jpg)

5. 更詳細設定請參考下列官方文件

## 參考資料
* [Facebook官方文件](https://developers.facebook.com/docs/plugins/comments)
