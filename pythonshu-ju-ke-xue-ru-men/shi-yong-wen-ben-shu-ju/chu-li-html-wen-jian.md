本章介绍的第一种结构化文本文档是HTML：一种通常在Web上用于表示人类可读信息的标记语言。HTML文档包含文本已经用于控制文本的显示和释义的预定义标签。标签可以具有属性，这里就不再介绍HTML的标签和属性了。

HTML是XML的前身，人们发现它的初衷是使机器处理可读文档，它算不上是一门编程语言，只是一系列具有相似结构的标记语言。用户可以根据需要定义XML标签及其属性。

# XML≠HTML

虽然XML和HTML看上去是很相似，但一个典型的HTML文档通常不会是一个有效的XML文档。反过来，XML文档也不是HTML文档。

XML标签依赖于所使用的应用程序。只要遵循一些简单的规则（例如，包含在尖括号内），任何字母和数字组成的字符串都可以作为标签。XML标签只起到对文本进行解释描述的作用，而不控制文本的显示，因此常在不需要人类直接阅读的文章中使用。使用可扩展样式语言转换（XSLT），能将XML转换为HTML。

# BeautifulSoup

BeautifulSoup模块用于解析、访问以及修改HTML和XML文档。可以使用一个标记字符串、一个标记文件或一个标记文档的网址，来构建一个BeautifulSoup对象：

```
from bs4 import BeautifulSoup
from urllib.request import urlopen

#使用字符串构建soup
soup1=BeautifulSoup("<HTML><HEAD></HEAD><body></body></HTML>")

#使用本地文件构建soup
soup2=BeautifulSoup(open("myDoc.html"))

#使用web文档构建soup
#记住urlopen()不会添加"http://"!
soup3=BeautifulSoup(urlopen("http://www.networksciencelab.com/"))
```

BeautifulSoup对象构造函数的第二个可选参数是标记解析器：负责提取HTML标签和实体的Python组件。BeautifulSoup附带四个预先安装好的解析器：

* “html.parser”
* "lxml"
* 


