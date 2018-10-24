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

* “html.parser”：默认的解析器，解析速度非常快，但是规则较为严格、对用于简单的html文档。
* "lxml"：解析速度非常快，规则较宽松
* "xml"：仅适用于XML文件

* "html5lib"：解析速度非常慢，规则也非常宽松，用于处理具有复杂结构的HTML文档，而如果不考虑解析速度，可用于所有HTML文档。

soup准备就绪后，可以使用函数soup.prettify\(\)完美地打印出原始标记文档。

函数soup.get\_text\(\)返回标记文档中除了所有标签的文本部分。当我们感兴趣的内容是纯文本时，就可以使用这个函数实现标记文本向纯文本的转换。

```
from bs4 import BeautifulSoup
htmlString="""
<html>
<head><title>My Document</title></head>
<body>
main text
</body>
</html>
"""

soup=BeautifulSoup(htmlString,features="html.parser")
print("#"*20)
print(soup.get_text())
print("#"*20)
print(soup.prettify())
print("#"*20)
```

结果：

```
####################


My Document

main text



####################
<html>
 <head>
  <title>
   My Document
  </title>
 </head>
 <body>
  main text
 </body>
</html>

####################
```

标签通常用于定位某些文件片段。例如，我们所感兴趣的部分可能是第一个表的第一行这样具体的片段。使用标签可以实现诸如此类的定位功能，尤其是当标签具有class或id属性时，而使用纯文本时不可能实现这个功能的。

BeautifulSoup对标签之间的垂直和水平关系使用统一的实现方法。类似于文件系统的层次结构，这种位置关系被表示为标签对象的属性。例如：soup的标题soup.title就是soup的对象属性。标题父元素的name对象的值是soup.title.parent.name.string，而第一个表第一行中的第一个单元格大概能表示为：soup.body.table.tr.td。

任何标签t都有一个名称t.name，一个字符串值（t.string表示原始内容，t.stripped\_strings表示删除空白后的列表）、父标签t.parent、下一个标签t.next和前一个标签t.prev，以及零个或多个子标签t.children\(标签中的标准\)。

BeautifulSoup通过Python字典接口实现对HTML标签属性的访问。如果标签对象t表示超链接（例如&lt;a href="index.html"&gt;），则超链接目标的字符串值为t\["href"\].string。

> Html标签是不区别大小写的，这点应引起注意

BeautifulSoup最有用的函数应该就是find\(\)和find\_all\(\)了，二者分别用于找到某个标签的第一个实例和所有实例。

* 标签&lt;h2&gt;的所有实例：

```
soup.find_all("h2")
```

* 所有粗体或斜体格式：

```
soup.find_all(["i","b","em","strong"])
```

* 具有某个属性（例如id="link3"）的所有标签：

```
soup.find(id="limk3")
```

* 使用字典符号或tag.get\(\)函数，找出所有超链接已经第一个链接的目标网址：

```
links=soup.find_all("a")
firstLink=links[0]["href"]
#或者
firstLink=links[0].get("href")
```

顺便提一下，如果标签不具备所查询的属性，则上面示例中的两种表打死都无法使用。鉴于可能遇到这样的情况，必须先使用tag.has\_attr\(\)函数检查属性是否存在，然后才能提取它。一下示例结合了BeautifulSoup和列表推导功能，来提取所有链接及其各自的网址和标签（这在用递归的方式抓取网页时非常有用）：

```
with urlopen("http://ww.xxx.com/") as doc:
    soup=BeautifulSoup(doc)

links=[(link,string,link["href"])  for link in soup.find_all("a") if link.has_attr("href")]
```

HTML、XML之所以强大，是因为它多样化的功能，但这种多功能也未尝不是它的魔咒，尤其是涉及表格数据处理时。

