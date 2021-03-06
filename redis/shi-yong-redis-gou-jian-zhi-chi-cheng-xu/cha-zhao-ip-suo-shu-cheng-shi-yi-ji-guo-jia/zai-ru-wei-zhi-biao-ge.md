#载入城市表格
为了开发IP所属地查找程序，我们将使用一个IP所属城市数据库作为测试数据。这个数据库包含两个非常重要的文件：

* 一个是GeoLiteCity-Blocks.csv，它记录了多个IP地址段以及这些地址段所属城市的ID
* 另一个是GeoLiteCity-Location.csv它记录了城市ID与城市名、地区名、州县名以及我们不会用到的其他信息之间的映射。

实现IP所属地查找程序会用到两个查找表：

* 第一个查找表需要根据输入的IP地址来查找IP所属城市的ID
* 第二个查找表则需要根据输入的城市ID来查找ID对应城市的实际信息（这个城市信息中还会包括城市所在地区的其他信息）

根据IP地址来查找城市ID的查找表由有序集合实现，这个有序集合的成员为具体的城市ID，而分值则是一个根据IP地址计算出来的整数值。为了创建IP地址与城市ID之间的映射，程序需要将点分十进制格式的IP地址转换为一个整数分值，下面的ip\__to_\_score\(\)函数定义了整个转化过程：IP地址中的每8个二进制会被看做是无符号整数中的1字节，其中IP地址最开头的8个二进制位最高位。

```
def ip_ti_score(ip_address):
    score=0
    for v in ip_address.split('.'):
        score=score*256+int(v,10)
    return score

if __name__ == '__main__':
    y=ip_ti_score('117.61.12.128')
    print(y)

    x=117
    x=x*256+61
    x=x*256+12
    x=x*256+128
    print(x)
```

运行结果：

```
1966935168
1966935168
```

在将IP地址转换为整数分值之后，程序就可以创建IP地址与城市ID之间的映射了。因为多个IP地址范围可能会被映射到同一个城市ID，所以程序会在普通的城市ID后面，加上一个\_字符以及有序集合目前已知的城市ID的数量，以此来构建一个独一无二的唯一城市ID。下面代码展示了程序时如何创建IP地址与城市ID之前的映射的。

```
import csv


#这个函数在执行时需要输入GeoLiteCity-Blocks.csv文件所在的路径
def import_ips_to_redis(conn,filename):
    csv_file=csv.reader(open(filename,'rb'))
    #enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，
    # 同时列出数据和数据下标，一般用在 for 循环当中。
    for count,row in enumerate(csv_file):
        start_ip=row[0] if row else ''
        #按照需要将IP地址转换为分值
        if 'i' in start_ip.lower():
            continue
        if '.' in start_ip:
            start_ip=ip_to_score(start_ip)
        elif start_ip.isdigit():
            start_ip=int(start_ip,10)
        else:
            #略过文件的第一行以及格式不正确的条目
            continue
        #构建唯一的城市ID
        city_id=row[2]+'_'+str(count)
        #将城市ID及其对应的IP地址分值添加到有序集合里面。
        conn_zadd('ip2cityid:',city_id,start_ip)
```

在调用import\_ips\_to\_redis\(\)函数并将所有IP地址都载入Redis之后，我们会像下面代码展示的那样，创建一个城市ID映射至城市信息的散列。因为所有城市信息的格式都是固定的，并且不会随着时间而发生变化，所有我们会将这些信息编码为JSON列表然后再进行存储。

```
def import_cities_to_redis(conn,filename):
    for row in csv.reader(open(filename,'rb')):
        if len(row)<4 or not row[0].isdigit():
            continue
        row=[i.decode('latin-1') for i in row]
        city_id=row[0]
        country=row[1]
        region=row[2]
        city=row[3]
        conn.hset('cityid2city:',city_id,json.dumps([city,region,country]))
```

在将所需的信息全部存储到Redis里面之后，接下来要考虑的就是如何实现IP地址查找功能了。

