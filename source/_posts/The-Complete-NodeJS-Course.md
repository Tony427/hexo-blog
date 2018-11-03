---
title: The-Complete-NodeJS-Course
date: 2017-10-27 12:00:00
tags:
categories: Nodejs
---
https://www.udemy.com/learn-website-hacking-penetration-testing-from-scratch/learn/v4/
## Express
### Express middleware
[Express middleware](http://expressjs.com/en/resources/middleware.html)
#### body-parser
>　Parse HTTP request body. See also: body, co-body, and raw-body.

#### cookie-parser
> Parse cookie header and populate req.cookies. See also cookies and keygrip.
> 

#### 

## mongoose
> mongoose 是一套給 Node.js 用的 MongoDB ODM，跟常聽到的 ORM 不同的地方只是一些技術名詞定義上的把戲，其實是差不多的意思。
> 

### 建立連線
```jsx
var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/test')
```
### Schema, Model與Entity
1. Schema => Model => Entity
```jsx
//建立一個新的Schema
var CatSchema = new mongoose.Schema({
    name:String
});

//由Schema建立Cat Model
var Cat = mongoose.model('Cat',CatSchema);

//New一個Cat的entity叫kitty
var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});
```

2. Model => Entity
```jsx
//或是直接指定術性名稱建立Cat Model
var Cat = mongoose.model('Cat',
  { name: String }
);

//New一個Cat的entity叫kitty
var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});
```

## connect-mongo
> In many circumstances, connect-mongo will not be the only part of your application which need a connection to a MongoDB database. It could be interesting to re-use an existing connection.

### 名詞
SQL資料庫的Table = NoSQL資料庫的collection

### 參考資料
[mongoose 簡單介紹](http://blog.chh.tw/posts/mongodb-odm-mongoose/)
[Mongoose的Schema, Model與Entity](https://ithelp.ithome.com.tw/articles/10161454)

## Template Engine
> Template Engine是用來render前端網頁的語法，例如在ASP.NET MVC使用的是Razor語法，將後端的資料顯示在前端網頁。
### ejs
> `ejs-mate`模組用來補足ejs不足的地方，主要可以適用`layout`、`partial`與`block`功能來引入頁面。
### Pug(Jade)
### Vue
#### 參考
[Vue.js 服務器端渲染指南](https://ssr.vuejs.org/zh/)