---
layout: post
title: '[flask] 在 Jinja2 中插入 JSON 数据'
subtitle: '解决传入 JSON 数据时出现的乱码'
date: 2018-08-18
categories: flask
cover: '../../../assets/img/jinja2-json-header.jpg'
tags: python flask Jinja2
---

之前在用 flask 搭建一个网页时，我想要在模版中传入一个 JSON 的字符串，结果发现通过 Jinja2 传入的 JSON 数据变成了了乱码。其中的引号和空格都变为了 `&#xx;` 的形式：

```javascript
var humidityJSON = \{{ humidityJSON }\};
```

传入之后：

```javascript
var humidityJSON = [{&#39;Time&#39;: &#39;08/18/2018 09:25:45&#39;, &#39;Humidity&#39;: 25}...
```

在一番检索后，我发现原来是因为 Jinja2 对 JavaScipt 的保护措施，导致不能很好地传入 JSON 数据。它会将其中的一些符号变为 HTML 中的编码。

这样直接就导致了生成的网页渲染失败。我尝试了一些方法，比如去掉传入字符串中的空格，但是其中的引号也同样会造成乱码，所以这种方法并不可行

尝试了半个上午无解之后，我去 Stack Overflow 上面搜索了这样一个问题，终于找到了解决方法：

[Stack Overflow - sending data as JSON object from Python to Javascript with Jinja](https://stackoverflow.com/questions/24719592/sending-data-as-json-object-from-python-to-javascript-with-jinja)

### 使用 tojson 设置

其实只需要在传入的数据后加上 tojson 的设置就能够完成传递 JSON 数据了。

```javascript
var humidityJSON = \{{ humidityJSON|tojson }\};
```

> Stack Overflow 网站上使用的方法是 {{ data|tojson|safe }} 但是其实在新版本的 Jinja2 之后就不需要 safe 选项了

```javascript
var humidityJSON = [{"Humidity": 25, "Time": "08/18/2018 09:25:45"}...
```

成功解决，果然 Stack Overflow 是个解决问题的好地方。
