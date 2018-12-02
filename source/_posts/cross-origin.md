---
layout: post
title: "跨域问题详解"
date: 2018-03-23 00:00:00 +0800
updated: 2018-12-02
---

作为一个前端，在工作中经常会遇到跨域的问题...
<!-- more -->

作为一个前端，在工作中经常会遇到跨域的问题，

## 什么是跨域

跨域的正规叫法应该是跨源，跨源相对于同源来说，何谓同源，MDN定义：

> 如果两个页面的**协议**，**端口**（如果有指定）和**域名**都相同，则两个页面具有相同的源。 

子域名之间是不同源的。在一个页面中，发往另一个源的的请求就叫跨源（跨域，以下皆称跨域）。

## 为什么要跨域

做过服务端的都知道，一个http接口，是没有限制的，谁都可以访问，但是浏览器出于安全考虑，限制了跨域的请求。

普通情况下，用 XHR 对象请求一个跨域的接口浏览器会直接抛出一个错误：

![xhr跨域请求的错误](https://raw.githubusercontent.com/rannuo/images/master/blog/xhr-cross-origin-error.png)


其实此时server是正常收到请求，并正常返回：

![后台正常](https://raw.githubusercontent.com/rannuo/images/master/blog/cross-origin-succ.png)

用 fetch 结果也一样

## 如何解决跨域问题

### 1.jsonp

jsonp 就是利用script标签的可跨域请求脚本来实现的
在浏览器中添加一个script标签，请求跨域的服务器上的一个接口，一般可以自定义一个callback的参数，

![浏览器jsonp](https://raw.githubusercontent.com/rannuo/images/master/blog/jsonp-client.png)

服务端这样写

![服务端jsonp](https://raw.githubusercontent.com/rannuo/images/master/blog/jsonp-server.png)

### 2.CORS

服务端在响应的头里指定这个字段 `Access-Control-Allow-Origin` 

![服务端cors](https://raw.githubusercontent.com/rannuo/images/master/blog/cors-server.png)

则客户端通过fetch或xhr对象请求，就可以拿到请求返回的资源了

![fetch-cors](https://raw.githubusercontent.com/rannuo/images/master/blog/cors-fetch.png)

### 3.代理转发

因为同源策略是浏览器的策略，在后台服务上不存在这个策略，所以我们可以通过后台服务进行转发。
前两种解决方案都是需要请求资源的服务端进行一些适配，而这个方法就不需要资源服务端进行任何修改。所以这种方案在使用第三方接口时，是最好用最常用的方案。

## 详见MDN

[CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)