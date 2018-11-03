---
title: 使用JekyII建置Git-Page
date: 2017-11-20 12:00:00
tags:
categories: Ruby
---

## 建立GitPage來host你的靜態網頁 （持續修改中）
1. 假設帳號為`xxx`
2. 建立新的repository，並且將名稱設定為`xxx.github.io`
3. repository裡面簡單的放上index.html，打上網址`https://xxx.github.io`就可以顯示indexl.html的內容
4. github目前預設使用這個repository的master分支顯示網頁內容(過去是使用gh-page分支)

## 使用Git Theme
1. 就選擇Git Theme就會自動產生樣板....
2. 後端記得一樣是Ruby的Jekyll

## 使用JekyII用md語法來寫網頁
1. 安裝Ruby

2. 安裝jekyll與jekyll bundler 
```shell
gem install jekyll
gem install bundler
```
執行JekyII站台
```shell
bundle exec jekyII serv
```
## Trouble Shotting

### Invalid CP950 character 
`Error message`
```ruby
Jekyll: fix Scss Conversion error: Jekyll::Converters::Scss Invalid CP950 character \xE2 on line 54

less than 1 minute read
add 1 line at end of file
```
`how to fix` 
find `\ruby24\lib\ruby\gems\2.4.0\gems\sass-3.5.1\lib\sass.rb`
append the following text to the end `Encoding.default_external = Encoding.find(‘utf-8’)`

## 參考資料
* [Jekyllrb](https://jekyllrb.com)
* [Jekyllrb簡中版](http://jekyll.com.cn/)

## 後記
平常不用ruby，加上考量主要寫dotnet前端會用js，所以改用hexo了