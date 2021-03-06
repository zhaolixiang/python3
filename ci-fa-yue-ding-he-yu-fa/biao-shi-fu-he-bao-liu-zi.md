> 上一篇文章：  
> 下一篇文章  
> Python词法约定和语法专题：总览

标识符是用来识别变量、函数、类、模块和其他对象的名称。标识符可以包含字母、数字和下划线，但必须以非数字字符开始。字母目前只允许使用ISO-Latin字符集种的字符A~Z和a~z。由于标识符是区分大小写的，所以FOO和foo是两个不同的标识符。诸如￥、%、@、$等特殊符号不允许出现在标识符种，另外保留字也不能单独作为标识符名称、下面是说有的保留字：

| and | del | from | nonlocal | try |
| :--- | :--- | :--- | :--- | :--- |
| as | elif | global | not | while |
| break | except | import | pass | yield |
| assert | else | if | or | with |
| class | exec | in | print | continue |
| finally | is | raise | def | for |
| lambda | return |  |  |  |

以下划线开始或结束的标识符通常具有特殊意义。例如：以一个下划线开始的标识符（如_foo）不能使用from module import \*语句导入。前后均带有下划线的标识符（如`__init__`）是为特殊方法保留的，而只有前面带有双下划线的标识符(如__bar)则用于实现私有的类成员。

