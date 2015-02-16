---
title: 浏览器/HTTP代理那点事儿
tagline: ""
last_updated: null
category : proxy
layout: post
tags : [http, proxy]
---
{% include JB/setup %}

IE、Chrome、Firefox或其他各种浏览器的代理选项中，类型有`HTTP`,`Secure(安全)`,`FTP`,`Socks(套接字)`4种，
外加一个选项`Use the same proxy server for all protocols.(对所有协议使用相同的代理服务器)`，
用了n年的代理，最近才算是100%的整清楚他们之间的关系。

<!-- more -->

## 从IE说起

IE浏览器的`代理设置`界面如下：

![IE Proxy Setting](/images/2015-02-03-about-ie-proxy/IE_Proxy_Setting.png)


**如果我要搭建一个代理服务器，那么服务器应该使用什么协议呢？**答案是：

* 如果HTTP、HTTPS、Socks都被设置了的话，代理服务器端需要开启http代理协议；
* 如果只有Socks被设置了的话(其他地址都为空)，代理服务器端需要开启的是Socks4/5协议；
* 如果勾选了`Use the same proxy server for all protocols.`，那么代理服务器端还是HTTP协议。
* 如果真的像我截图这样，几种type分别设置了不同的代理端口呢？答案是HTTP协议将走1111端口的http代理，HTTPS协议将走2222端口的http代理。（我没有找到什么时候Socks会起作用）。

可以看到，`Socks(套接字)`其实和其他几个的含义感觉并不太一样，很可能包含两层意思:

- `Socks4/5协议`：见[wiki页](http://zh.wikipedia.org/wiki/SOCKS)；
- `Socket`: 见[wiki页](http://zh.wikipedia.org/wiki/Berkeley%E5%A5%97%E6%8E%A5%E5%AD%97)。主流浏览器的HTTP、HTTPS的底层都是走tcp协议，也就是`socket`。


## 代理类型

在代理软件开发的一些官方文档中，`协议类型`一般用`scheme`来表示，也就是你在浏览器中输入的是`http://`，`https://`，或者是`ftp://`。

而`代理协议类型`，才会用`protocol`来表示，包括`socks4/5`，`HTTP 代理`，`HTTPS 代理`。

* HTTP1.1的[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)中包含`HTTP 代理 protocol`。
* `HTTPS 代理`好像只有Chrome支持，看到一些android加速app也在用`HTTPS 代理`，服务器端可以用`stunnel`。



## 总结

感觉好像越说越乱，虽然我自己是理解了。。。。


