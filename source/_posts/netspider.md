---
title: .netcore 网络爬虫
subtitle: 爬虫
tags: 
  - 软件开发
date: 2023-10-12
---

网络爬虫（又被称为网页蜘蛛，网络机器人）就是模拟浏览器发送网络请求，接收请求响应，一种按照一定的规则，自动地抓取互联网信息的程序。知名的 *Google*、*Baidu* 就是基于网络爬虫程序满足到日常用户的搜索需求。
原则上,只要是浏览器(客户端)能做的事情，爬虫都能够做。

在 *B/S* 架构的基础上，通过编程自定义 *Request-Headers* 向网络服务器请求数据（ *HTML* ），然后解析 *HTML* ，提取出自己想要的数据，在按页的如Blog的数据网站，爬虫的机械方式可以更快，更方便的抓取数据，并存于本地文件中。

{% plantuml %}
!theme sketchy-outline

hide footbox

Clawer --> Clawer: Page from start to end
...request & response...
Clawer --> Server: Make a network request
Server --> Clawer: Authentication response
Clawer --> Clawer: Parse response data

{% endplantuml %}
