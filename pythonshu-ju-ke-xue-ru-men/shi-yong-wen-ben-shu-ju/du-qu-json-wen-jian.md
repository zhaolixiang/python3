JSON是一种轻量级的数据交换格式。该格式跟编程语言无关，这一点不同于pickle模块，但在数据表示方面，JSON有更多的限制，不如pickle灵活。

# 1、JSON支持的数据类型

* 原子数据类型：字符串、数字、true、false和null
* 数组：使用方括号\[ \]表示，对应Python的列表。
* 对象：使用花括号{ }表示，对应Python的字典，对象各项由冒号分隔的键和值组成。
* 数组、对象和原子数据类型的任何递归结合（对象数组、项为数组的对象、等待）

有一个不尽如人意之处，那就是某些Python数据类型的机构（比如集合和复数）无法存储在JSON文件中。因此，要在导出到JSON之前，将它们转换为JSON可表示的数据类型，例如，将复数存储为两个double类型的数字组成的数组，将集合存储为一个由集合的各项所组成的数组。

将复杂数据存储到JSON文件中的操作成为序列化，相应的反向操作成为反序列化。Python通过json模块中的函数，实现JSON序列化和反序列化。

* 函数dump\(\)将一个能用JSON表示的Python对象导出（dump）到文件。

* 函数dumps\(\)导出的Python对象为文本字符串（这是为了打印出美观的结果或实现进程间通信）。

* dump\(\)和dumps\(\)这两个函数都实现了序列化。

# 2、pickling的意义

当数据保存到JSON文件时，只是保存了变量的值，重新加载后，这些值都是相互独立的。当使用pickle来保存同样的数据时，保存的是原始变量的引用，重新加载后，对同一变量的所有引用关系维持不变。

# 3、JSON转换

函数loads\(\)将有效的JSON字符串转换为Python对象（即将对象”加载“到Python中）。实际中，总会遇到这样的转换。

类似的，函数load\(\)将已打开的文本文件的内容转换为一个Python对象。

把多个对象存储在一个JSON文件中是一种错误的做法，但是如果已有的文件包含多个对象，则可将其以文本的方式读入，进而将文本转换为对象数组（在本文中各个对象之间添加方括号和逗号分隔符），并使用loads\(\)将文本发序列化为对象列表。

一下代码片段实现了将任意（可序列化的）对象按先序列化、后反序列化的顺序进行处理：

```
object=XXXX
#将对象保存到文件
with open("data.json","w") as out_json:
    json.dump(object,out_json,indent=None,sort_keys=False)

#将文件载入对象
with open("data.json") as in_json:
    object1=json.load(in_json)
#将对象序列化为字符串
json_string=json.dumps(object1)

#把字符串解析为json
object2=json.loads(json_string)
```

尽管经历了四次【痛苦】的转换，object 、object1和object2扔具有相同的值。

