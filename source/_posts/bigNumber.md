---
layout: post
title: "js, json 中处理大数"
date: 2018-01-11 23:55:00 +0800
categories: 
- [js, json]
tags: 
- js
- json
---

在 js 中处理较大型数字，经常会遇到精确度不够的问题
<!-- more -->

在 js 中处理较大型数字，经常会遇到精确度不够的问题，[`big.js`](http://mikemcl.github.io/big.js/) 这个库就是为了解决js中大数的问题。

如果在一段 json 中，如果包含了较大的数，那么在用 `JSON.parse` 解析的时候就会丧失精度，比如
```js
let json = '{"id": 25148844545547858495}'

JSON.parse(json)  // { id: 25148844545547858000 }
```
这时可用一个基于 `big.js` 的库 `json-bigint` 来解决这个问题：
```js
let json = '{"id": 25148844545547858495}'

JSONBig.parse(json) // { id: BigNumber { s: 1, e: 19, c: [ 251488, 44545547858495 ] } }
```
可以通过传入一个函数把这个 `BigNumber` 转成字符串：
```js
let json = '{"id": 25148844545547858495}'

JSONBig.parse(json, (k, v) => v.isBigNumber ? v.toString() : v )
// { id: '25148844545547858495' }
```
其他不是大数的值不受影响：
```js
let json = '{"id": 25148844545547858495, "littleNumber": 123}'

JSONBig.parse(json, (k, v) => v.isBigNumber ? v.toString() : v )
// { id: '25148844545547858495', littleNumber: 123}
```