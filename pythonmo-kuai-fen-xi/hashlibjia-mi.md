hashlib模块是用来对字符串进行hash加密的模块，明文与密文是一一对应不变的关系；用于注册、登录时用户名、密码等加密使用。

# 一、函数分析

##### 1、共有5种加密算法

md5\(\),sha1\(\),sha224\(\),sha256\(\),sha3840\(\),sha512\(\)，分别得到不同的加密密文。

##### 2、hashlib.hexdigest\(\)：获取加密的密文，16进制，无参数

##### 3、hashlib.digest\(\)：获取加密的密文，二进制，无参数

##### 4、hashlib.copy\(\)：复制一份当前创建的hash对象，无参数

##### 5、update\(str1,encoding\('utf-8''\)\)：更新加密的密文，得到的密文与原来的密文不相同

##### 6、hash.name：查看当前获得的hash对象的加密算法

##### 7、hash.digest\_size：hash密钥占多少个字节

##### 8、hash.block\_size：hash数据块的大小

##### 9、hashlib.algorithms\_guaranteed：查看所有平台都支持的hash算法

##### 10、hashlib.algorithms\_available：查看所有的hash加密算法

# 二、运用：

##### 1、创建哈希对象，有两种方式：

```
m=hashlib.new('md5',b'cai')#选择md5加密函数加密字符串‘cai’
m=hashlib.md5('cai'.encode('utf-8'))#加密的另一种写法
```



##### 2、













