跨站请求伪造（Cross-site request forgery，CSRF 或XSRF）是一种对网站的恶意利用。通过CSRF，攻击者可以冒用用户的身份，在用户不知情的情况下执行恶意操作。

# 1、CSRF攻击原理

下图展示了CSRF的基本原理。其中Site1是存在CSRF漏洞的网站，而SIte2是存在攻击行为的恶意网站。

![](/assets/Jietu20181008-221918.jpg)上图内容解析如下：

* 用户首先访问了存在CSRF漏洞网站Site1，成功登陆并获取了Cookie，此后，所有该用户对Site1的访问均会携带Site1的Cookie，因此被Site1认为是有效操作。
* 此时用户又访问了带有攻击行为的站点Site2，而Site2的返回页面中带有一个访问Site1进行恶意操作的连接，但却伪装成了合法内容，比如下面的超链接看上去是一个抽奖信息，实际上却是想Site1站点提交提款请求

```
<a href='http://www.site1.com/get_money?amount=500&dest_card=34XXXXX">
三百万元抽奖，免费拿
</a>
```

* 用户一旦点击恶意链接，就在不知情的情况下向Site1站点发送了请求。因为之前用户在Site1进行过登陆且尚未退出，所以Site1在收到用户的请求和附带的Cookie时将被认为该请求是用户发送的正常请求。此时，恶意站点的目的也已经达到。

# 2、用Tornado防范CSRF攻击

为了防范CSRF攻击，要求每个请求包括一个参数值作为令牌的匹配存储在Cookie中的对应值。

Tornado应用可以通过一个Cookie头和一个隐藏的HTML表单元素向页面提供令牌。这样，当一个合法页面的表单被提交时，它将包括表单值和已存储的Cookie。如果两者匹配，则Tornado应用认可请求有效。

开启Tornado的CSRF防范功能需要两个步骤。

##### 【1】在实例化tornado.web.Application时传入xsrf\_cookies=True参数，即：

```

```

或者：

```

```

> 当tornado.web.Application需要初始化的参数过多时，可以像本例一样通过setting字典的形式传入命名参数

##### 【2】在每个具有HTML表达的模板文件中，为所有表单添加xsrf\_form\_html\(\)函数标签，比如：

```
<form action="/login" method="post">
{% module xsrf_form_html() %}
<input type="text" name="message"/>
<input type="submit" value="Post"/>
</form>
```

这里的{% module xsrf\_form\_html\(\) %}起到了为表单添加隐藏元素以防止跨站请求的作用。

Tornado的安全Cookie支持和XSRF防范框架减轻了应用开发者的很多负担，没有他们，开发者需要思考很多防范的细节措施，因此Tornado内建的安全功能也非常有用。



















