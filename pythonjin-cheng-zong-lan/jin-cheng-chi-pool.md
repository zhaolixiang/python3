# 4、进程池Pool

当我们需要创建大量的进程时，利用multiprocessing模块提供的Pool来创建进程。

进程初始化时，会指定一个最大进程数量，当有新的请求需要创建进程时，如果此时进程池还没有到达设置的最大进程数，该进程池就会创建新的进程来处理该请求，并把该进程放到进程池中，如果进程池已经达到最大数量，请求就会等待，知道进程池中进程数量减少，才会新建进程来执行请求。

* ##### 语法

```
pool=Pool(numprocess,initializer,initargs)
numproxess:需要创建的进程个数，如果忽略将使用cpu_count()的值。即cpu支持的最大进程数量。
initializer:每个进程启动时都要调用的对象。
initargs:为
initalizer传递的参数。
```

* ##### 常用方法

multiprocessing.Pool常用函数解析：

```
apply_async(要调用的方法,参数列表,关键字参数列表)：使用非阻塞方式调用指定方法，并行执行（同时执行）

apply(要调用的方法,参数列表,关键字参数列表)：使用阻塞方式调用指定方法，，阻塞方式就是要等上一个进程退出后，下一个进程才开始运行。

close():关闭进程池，不再接受进的进程请求，但已经接受的进程还是会继续执行。

terminate()：不管程任务是否完成，立即结束。

join():主进程堵塞（就是不执行join下面的语句），直到子进程结束，注意，该方法必须在close或terminate之后使用。

pool.map(func,iterable,chunksize):将可调用对象func应用给iterable的每一项，然后以列表形式返回结果，
通过将iterable划分为多块，并分配给工作进程，可以并行执行。chunksize指定每块中的项数，
如果数据量较大，可以增大chunksize的值来提升性能。

pool.map_async(func,iterable,chunksize,callback):与map方法不同之处是返回结果是异步的，
如果callback指定，当结果可用时，结果会调用callback。

pool.imap(func,iterable,chunksize):与map()方法的不同之处是返回迭代器而非列表。

pool.imap_unordered(func,iterable,chunksize):与imap()不同之处是：结果的顺序是根据从工作进程接收到的时间而定的。

pool.get(timeout):如果没有设置timeout，将会一直等待结果，
如果设置了timeout，超过timeout将引发multiprocessing.TimeoutError异常。

pool.ready():如果调用完成，返回True

pool.successful():如果调用完成并且没有引发异常，返回True，如果在结果就绪之前调用，jiang引发AssertionError异常。

pool.wait(timeout):等待结果变为可用，timeout为等待时间。
```

* ##### 实例1：阻塞与非阻塞对比

```
#阻塞与非阻塞对比
from multiprocessing import Pool
import os
import time
import random#用来生成随机数

def test1(name):
    print("%s运行中，pid=%d，父进程：%d"%(name,os.getpid(),os.getppid()))
    t_start=time.time()
    #random.random()会生成一个0——1的浮点数
    time.sleep(random.random()*3)
    t_end=time.time()
    print("%s执行时间：%0.2f秒"%(name,t_end-t_start))

pool=Pool(5)#设置线程池中最大线程数量为5
for xx in range(0,7):
    #非阻塞运行
    pool.apply_async(test1,("mark"+str(id),))
print("--start1--")
pool.close()#关闭线程池，关闭后不再接受进的请求
pool.join()#等待进程池所有进程都执行完毕后，开始执行下面语句
print("--end1--")
print("*"*30)
pool=Pool(5)#设置线程池中最大线程数量为5
for xx in range(0,7):
    #阻塞运行
    pool.apply(test1,("mark"+str(id),))
print("--start2--")
pool.close()#关闭线程池，关闭后不再接受进的请求
pool.join()#等待进程池所有进程都执行完毕后，开始执行下面语句
print("--end2--")
```

结果：

```
--start1--
mark<built-in function id>运行中，pid=28631，父进程：28626
mark<built-in function id>运行中，pid=28632，父进程：28626
mark<built-in function id>运行中，pid=28633，父进程：28626
mark<built-in function id>运行中，pid=28634，父进程：28626
mark<built-in function id>运行中，pid=28636，父进程：28626
mark<built-in function id>执行时间：0.27秒
mark<built-in function id>运行中，pid=28633，父进程：28626
mark<built-in function id>执行时间：0.32秒
mark<built-in function id>运行中，pid=28634，父进程：28626
mark<built-in function id>执行时间：0.18秒
mark<built-in function id>执行时间：0.55秒
mark<built-in function id>执行时间：1.78秒
mark<built-in function id>执行时间：1.92秒
mark<built-in function id>执行时间：2.71秒
--end1--
******************************
mark<built-in function id>运行中，pid=28647，父进程：28626
mark<built-in function id>执行时间：0.70秒
mark<built-in function id>运行中，pid=28648，父进程：28626
mark<built-in function id>执行时间：1.66秒
mark<built-in function id>运行中，pid=28649，父进程：28626
mark<built-in function id>执行时间：2.87秒
mark<built-in function id>运行中，pid=28650，父进程：28626
mark<built-in function id>执行时间：2.68秒
mark<built-in function id>运行中，pid=28651，父进程：28626
mark<built-in function id>执行时间：1.42秒
mark<built-in function id>运行中，pid=28647，父进程：28626
mark<built-in function id>执行时间：1.20秒
mark<built-in function id>运行中，pid=28648，父进程：28626
mark<built-in function id>执行时间：2.01秒
--start2--
--end2--
```

* ##### 实例2：利用进程池遍历目录

> 查看下面实例前，先来熟悉一下我们需要用到的一些知识。
>
> os.walk\(top,topdown=true,onerrorNone,followlinks=false\):遍历目录地址，返回一个三元组（root，dirs，files）
>
> top:想要遍历的目录
>
> root:当前正在遍历的文件夹地址。
>
> dirs:是一个list：当前文件夹下所有目录的名字（不包括子目录）
>
> files:也是一个list，当前文件夹下所有文件的名字（不包括子目录文件）
>
> topdown:默认为True：优先遍历top目录，为false会优先遍历top的子目录。
>
> onerror:指定一个当方法执行异常时需要调用的方法。
>
> followlinks:默认为True：会遍历目录环境下的快捷方式实际指向目录。

代码：

```
#遍历目录文件并求取SHA512的摘要值
import os
import multiprocessing
import hashlib

#定义进程大小
POOLSIZE=2 #工作进程的数量
#可以读取的缓冲区大小
BUFSIZE=8196

def mark(filename):
    try:
        f=open(filename,"rb")
    except IOError:
        return None
    digest=hashlib.sha512()
    while True:
        chunk=f.read(BUFSIZE)
        if not chunk:break
        digest.update(chunk)
    f.close()
    return filename,digest.digest()



def build_map(dir):
    #定义进程
    pool=multiprocessing.Pool(POOLSIZE)


    #os.path.join:拼接文件路径
    #根据文件目录和名称拼接
    all_files=(os.path.join(root,name)
               #循环遍历当前目录
               for root,dirs,files in os.walk(dir)
                   #取出当前目录下文件名
                   for name in files)

    #dict方法用于将结果转化成字典
    map=dict(pool.imap_unordered(mark,all_files,20))
    pool.close()
    return map


if __name__=="__main__":
    digest_map=build_map("/Users/zhaolixiang/Desktop/python/test1/进程")
    for item in  digest_map:
        print("文件目录：",item)
        print("SHA512摘要值：",digest_map[item])
    #个数
    print(len(digest_map))
```

结果

    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/1、fork创建子线程.py
    SHA512摘要值： b'\x9eYLS\xe5\xb6\xd2\xec\xd4&\xa9\xff~m?\x87\xd2N\xea39G\xe1\x8f\x9cd\x83@\x06/B\n\xdd\x1e\xb5\xe0j\x10\xd1\xc9=\xfd;y]\x8dnR)\xbd\t\xb8\xc8\xb46rE\xf8\xd7t.\xae\xbb\xe9'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/8、JoinableQueue的生产者与消费者.py
    SHA512摘要值： b'\xfe4\x18\xee8\xd1\x97\xe7v\x86}\xe6q\n\x13\xf9D\xf1X\xe3\xab\x94\xeb\x96\xfb\xedW\x0bO\n$\x14d/+\r\x9b\x0b\xc1\xd4\x03\xad\xcbb\xf7\x8d\xd5Cc\x03y\xd1\xb4\x801,?,rIX\xa28\xd7'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/10、管道：返回数据.py
    SHA512摘要值： b'\x85\xb1C\xe56\x80\x1ds\x84\t\xf6\x95\xcb\x1d[\xda\xe4}n)Y<\xd5\x10=\x94\x88\xd8\xaf\xccr@eC\xc4!\xc1\xb70\x9fz\xc6\xb1\xb9\xf6\xc4\xce\xd7\x02\x86<\xc8U\x0f\x8bq\xda\x18yd\xa3K\x8b\xd6'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/11、进程池.py
    SHA512摘要值： b'bB\xf4l\xcc\xd5\xae\tr\xc1\x816\xe5\xfc;\t\x85\x86\xe5\xd3\x9e~SH]\xe6\xcbd\xc9\xfe\xe9\xfb\xfc\xeb)A\xd2o\x8cO\xbc\x1e\t\xe9\xe92^\xb5\x10\xe1\xf4\xc9\\_\x0c\x8c\xa7q\xf4(\x13M\xbe7'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/2、multiprocessing.py
    SHA512摘要值： b"m\xb2\xb5\x00M1=q\x86\xb9G\xf9\x02\xb1\x18\xfa\x08\xe7,.\xef\xff5\xeb\t,\n\x17\xbfB-\xc6\xc2\x9dV\xf9\x88\xa7\xd8\x1e\x9b)\x86N\xd5\xab \x89\xa0J}\xa9\xdc\xbc\x06m\x03\xa7\x87.\x17\xcb'\x93"
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/6、验证：put方法会阻塞.py
    SHA512摘要值： b'\xa3\x80\x86\xf6\xc9\xb8\x0e`OB 5\xd0\x949\xfd\xc2y\x9a\xc7\xca\x9d\xbc%\xa1\xe6`_\xfa\x84\xb6\x02$\xf7\x10\xbb\xb9N\xfb\xde\xc8\xbe6F \xd6\x87\xac\xb2\xf5\x94\x0c\xed\xe0.\xf9T\xda\x91`;\xd7\x90\x86'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/7、closse方法、get方法、put方法简单使用.py
    SHA512摘要值： b'l\x98w\x8fT\x15\xdfT\x19\x91^:\xc5\xa3\x8d\xf4\x1e\x9c\x91E\xe4\xe7\xbf\xecV\x1e\\\x1b\x19\xa0i\x96L\xc6\xc4r.\x9b\xec\x88\xe9r\xfeO\x8b\xdfA\x90\x7f6?\xe7\x1d8\xa6N\x07\xde\x8d\xb6\xe7#\x02p'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/5、进程池.py
    SHA512摘要值： b'\xc2\xf75\x04\xba\xa5X\xcc\x81\x88\xff\xbaz\xd81@\x0bQJ%\xbb\x15\xf3`Q\x9a4}\xc0\x07\xa2&a\xc0\x00\x0c%\xb4[\xd2e\x18\x04\x14y\t\x0c\xb0\xac\x1d\xf5\xfb\xe0\\\xc7\xb6$\xd2\xf1\xd9seP\x08'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/3、multiprocessing.py
    SHA512摘要值： b'\x04N4D\xd5\xab}\xcf\x03\xe6\x0fV\x0f\x910\x91\xe4\x81,\xbb-\xdf\xb36I\xf9!\x84A\xdf.\xf3HV\xfd\x86:\x0b\x81<+e\x00\xd1\x17u\xf5h\xb34F\xfe\xebF\xf5\x1b\xc3\x8d`A\xa0A\x02\x10'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/9、管道.py
    SHA512摘要值： b'`\xdb\x8d\x90V\x04=\x0c\xf9\x9c\xf7{\x8f\xca\x9f\xcc\xb8D\x97\xec\x82(\xd4\x9a\x84\xdb<\xb8+\x16\xd7\xec\x057\xe0\x07\xe9\xc3\xdcc\xe3 t#v\xec\xb5\xe3np\xc7~\x95\xd0J-c\xb1\xef\x03.?x\x12'
    文件目录： /Users/zhaolixiang/Desktop/python/test1/进程/4、继承Process创建进程.py
    SHA512摘要值： b'2\xc1\xa0\x1f\xd7\xd6\xb2\x1c}\x14\xded\x8f\xdb\xed\xd0\x91m\xc1,\xb9\xdd?TbX\x04#2\xfc\xb8\xbfu\r\xab\xfcFc\x17\x18\xc7)sY\x82\x0e\xea{5\x87\xf3\x8fc\xbaP\x91r0\xef\xabL\xa8\x1e\x15'
    11



