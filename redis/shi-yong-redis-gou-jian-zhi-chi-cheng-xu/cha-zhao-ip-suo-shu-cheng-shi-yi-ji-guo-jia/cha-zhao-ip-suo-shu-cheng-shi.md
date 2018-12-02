#查找IP所属城市
为了实现IP地址查找功能，我们在上一个小节已经将代表城市ID所属IP地址段起始端的整数分值添加到了有序集合里面。要根据给定IP地址来查找所属城市，程序首先会使用ip\__to_\_score\(\)函数将给定的IP地址转换为分值，然后在所有分值小于或等于给定IP地址里面，找出分值最大的那个IP地址所对应的城市ID。这个查找城市ID的操作可以通过调用zrevrangebyscore命令并将选项start和num的参数分别设为0和1来完成。在找到城市ID之后，程序就可以在存储着城市ID与城市信息映射的散列里面获取ID对应城市的信息了。

下面清单展示了IP地址所属地查找程序的具体实现方法：；

```
def find_city_by_ip(conn,ip_address):
    if isinstance(ip_address,str):
        #将IP地址转换为分值以便执行zrevrangebyscore命令
        ip_address=ip_to_score(ip_address)

    #查找唯一城市ID
    city_id=conn.zrevrangebyscore('ip2cityed:',ip_address,0,start=0,num=1)

    if not city_id:
        return None
    #partition() 方法用来根据指定的分隔符将字符串进行分割。
    # 如果字符串包含指定的分隔符，则返回一个3元的元组，
    # 第一个为分隔符左边的子串，第二个为分隔符本身，第三个为分隔符右边的子串。
    #将唯一城市ID转换为普通城市ID
    city_id=city_id[0].partition('_')[0]
    #从散列里面取出城市信息
    return json.loads(conn.hget('cityid2city:',city_id))
```

通过上面函数，我们现在可以基于IP地址来查找相应的城市信息并对用户的来源地进行分析了。

本节介绍的【将数据转换为整数并搭配有序集合进行操作】的做法非常有用，它可以极大简化对特定元素或特定范围的查找工作。

