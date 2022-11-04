[toc]

# Redis

## 简介

redis 远程字典服务，当下最热门的 nosql 技术之一。内存存储，持久化（adb，aof），高速缓存，支持集群。

官网：https://redis.io
中文网：www.redis.cn
github:https://github.com/redis/redis

### 安装 redis

#### windows 安装 redis

pass

#### linux 安装 redis

下载安装包
安装包地址：http://www.redis.cn/

解压 redis，在解压目录下运行 make。

```
sudo apt-get install gcc
sudo apt-get install g++
make
```

默认安装在/usr/local/bin/

### 启动和连接 redis

#### 启动 redis

通过配置文件启动服务

```
redis-server zjconf/redis.conf
```

#### 连接 redis

```
redis-cli -p 6379
```

### redis 常用命令

#### set

存值

```
set name value
```

#### get

取值

```
get name
```

#### keys \*

查看所有值

```
keys *
```

#### ps -ef|grep redis

查看 redis 相关进程信息

#### shutdown

关闭 redis

#### select

redis 默认有 16 个数据库，用 select 切换数据库

```
select 3
```

#### dbsize

查看当前数据库的大小

#### flushdb

清空当前数据库

#### flushall

清空全部数据库

#### exists key

判断 key 是否存在，如果存在则返回 1

#### append key value

给 key 追加数据

## redis 数据基本类型

### String

```bash
127.0.0.1:6379> set views 0 #设置views为0
OK
127.0.0.1:6379> incr views #值加一
(integer) 1
127.0.0.1:6379> decr views #值减一
(integer) 0
127.0.0.1:6379> incrby views 10 #值加十
(integer) 10
127.0.0.1:6379> decrby views 5 #值减十
(integer) 5
```

```bash
127.0.0.1:6379> set zj "hello world"
OK
127.0.0.1:6379> get zj
"hello world"
127.0.0.1:6379> getrange  zj 1 3 #查看1～3下标字符
"ell"
127.0.0.1:6379> setrange zj 1 xxx #从下标1，往后一直替换
(integer) 11
127.0.0.1:6379> get zj
"hxxxo world"
127.0.0.1:6379>
```

```bash
127.0.0.1:6379> setex zj2 15 test #设置15秒后过期
OK
127.0.0.1:6379> ttl zj2 #查看过期时间
(integer) 13
127.0.0.1:6379> setnx zj2 "ssss" #如果不存在就设置为ssss
(integer) 1
```

```bash
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> keys *
1) "zj"
2) "k1"
3) "k3"
4) "zj2"
5) "k2"
127.0.0.1:6379> mget k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
```

### List

大多数 list 命令都是 l 开头。list 相当于是一个链表。

```bash
127.0.0.1:6379> lpush zj 1 #插入到list头部
(integer) 1
127.0.0.1:6379> lpush zj 2
(integer) 2
127.0.0.1:6379> lpush zj 3
(integer) 3
127.0.0.1:6379> lrange zj 0 -1
1) "3"
2) "2"
3) "1"
127.0.0.1:6379> lrange zj 0 0 #第0个值是3！！！
1) "3"
```

```bash
127.0.0.1:6379> rpush zj 4 #从右边插入值
(integer) 4
127.0.0.1:6379> lrange zj 0 -1
1) "3"
2) "2"
3) "1"
4) "4"
```

```bash
127.0.0.1:6379> lpop zj  #移除list开头元素
"3"
127.0.0.1:6379> lrange zj 0 -1
1) "2"
2) "1"
3) "4"
127.0.0.1:6379> rpop zj #移除list右边边元素
"4"
127.0.0.1:6379> lrange zj 0 -1
1) "2"
2) "1"
127.0.0.1:6379>
```

```bash
127.0.0.1:6379> lrange zj 0 -1
1) "2"
2) "1"
127.0.0.1:6379> llen zj #list长度
(integer) 2
127.0.0.1:6379> lindex zj 0 #取出标号为0的list元素
"2"
```

```bash
127.0.0.1:6379> lrange zj 0 -1
1) "44"
2) "33"
3) "22"
4) "44"
5) "33"
6) "22"
7) "2"
8) "1"
127.0.0.1:6379> lrem zj 1 44 #移除值44一次
(integer) 1
127.0.0.1:6379> lrange zj 0 -1
1) "33"
2) "22"
3) "44"
4) "33"
5) "22"
6) "2"
7) "1"
127.0.0.1:6379> lrem zj 2 22 #移除值22两次
(integer) 2
127.0.0.1:6379> lrange zj 0 -1
1) "33"
2) "44"
3) "33"
4) "2"
5) "1"
```

```bash
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "world"
3) "hello"
4) "test"
5) "test"
6) "test"
127.0.0.1:6379> ltrim list 0 2 #修改了原来的list
OK
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "world"
3) "hello"
```

```bash
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "world"
3) "hello"
127.0.0.1:6379> rpoplpush list mylist #将元素从原来的list中移除并且添加的新的列表
"hello"
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "world"
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
```

```bash
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "world"
127.0.0.1:6379> lset list 1 1111 #设置list某个位置的值
OK
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "1111"
```

```bash
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "1111"
127.0.0.1:6379> linsert list after yeah~ ssss #在yeah～ 后面插入ssss
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "yeah~"
2) "ssss"
3) "1111"
```

### Set

一般都三以 s 开头

```bash
127.0.0.1:6379> sadd zj 1 2 3 4 5 5 6 7 #往集合里加入值
(integer) 7
127.0.0.1:6379> smembers zj #查看set所以值
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
127.0.0.1:6379> sismember zj 7 #查看集合里是否存在该元素，存在返回1，不存在返回0
(integer) 1
127.0.0.1:6379> sismember zj 8
(integer) 0
```

```bash
127.0.0.1:6379> scard zj #查看元素个数
(integer) 7
127.0.0.1:6379> srem zj 5 #移除元素5
(integer) 1
127.0.0.1:6379> scard zj
(integer) 6
127.0.0.1:6379> smembers zj
1) "1"
2) "2"
3) "3"
4) "4"
5) "6"
6) "7"
```

```bash
127.0.0.1:6379> smembers zj
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
127.0.0.1:6379> srandmember zj 2 #随机出两个元素
1) "2"
2) "6"
```

```bash
127.0.0.1:6379> smembers zj
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
127.0.0.1:6379> spop zj 2 #随机删除set中的两个元素
1) "1"
2) "4"
127.0.0.1:6379> smembers zj
1) "2"
2) "3"
3) "5"
4) "6"
5) "7"
127.0.0.1:6379>
```

```bash
127.0.0.1:6379> smembers zj
1) "2"
2) "3"
3) "5"
4) "6"
5) "7"
127.0.0.1:6379> smembers zj2
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> smove zj zj2 2 #把2从集合zj移动到zj2
(integer) 1
127.0.0.1:6379> smembers zj
1) "3"
2) "5"
3) "6"
4) "7"
127.0.0.1:6379> smembers zj2
1) "2"
2) "c"
3) "b"
4) "a"
```

```bash
127.0.0.1:6379> smembers zj
1) "d"
2) "a"
3) "e"
4) "f"
5) "b"
6) "c"
127.0.0.1:6379> smembers zj2
1) "h"
2) "g"
3) "e"
4) "k"
5) "f"
6) "j"
127.0.0.1:6379> sdiff zj zj2 #set1相比与set2有哪些不同
1) "c"
2) "b"
3) "a"
4) "d"
127.0.0.1:6379> sdiff zj2 zj
1) "k"
2) "j"
3) "h"
4) "g"
127.0.0.1:6379> sinter zj zj2 #交集
1) "e"
2) "f"
127.0.0.1:6379> sunion zj zj2 #并集
 1) "f"
 2) "c"
 3) "b"
 4) "k"
 5) "e"
 6) "g"
 7) "h"
 8) "d"
 9) "a"
10) "j"
```

### Hash

key->map,一般以 h 开头

```bash
127.0.0.1:6379> hset myhash f1 test #设置一个hash
(integer) 1
127.0.0.1:6379> hget myhash f1 #取得值
"test"
127.0.0.1:6379> hmset myhash f1 test2 f2 test f3 test #设置多个hash
OK
127.0.0.1:6379> hmget myhash f1 f2 f3 #取得多个值
1) "test2"
2) "test"
3) "test"
127.0.0.1:6379> hgetall myhash #获取所有值
1) "f1"
2) "test2"
3) "f2"
4) "test"
5) "f3"
6) "test"
```

```bash
127.0.0.1:6379> hdel myhash f1 #删除一个值
(integer) 1
127.0.0.1:6379> hexists myhash f1 #判断一个hash值是否存在，存在为1，不存在为0
(integer) 0
127.0.0.1:6379> hlen myhash #获取hash的字段数量
(integer) 2
127.0.0.1:6379> hkeys myhash #获取所有的key
1) "f2"
2) "f3"
127.0.0.1:6379> hvals myhash #获取所有的值
1) "test"
2) "test"
127.0.0.1:6379> hgetall myhash #获取所有的键值对
1) "f2"
2) "test"
3) "f3"
4) "test"
```

```bash
127.0.0.1:6379> hset myhash f1 1
(integer) 1
127.0.0.1:6379> HINCRBY myhash f1 5 #值加5
(integer) 6
```

### Zset

有序集合，一般以 z 开头

```bash
127.0.0.1:6379> zadd myset 1 a 2 b 3 c #创建一个zset
(integer) 3
127.0.0.1:6379> ZRANGE myset 0 -1 #显示所有的值
1) "a"
2) "b"
3) "c"
127.0.0.1:6379> zadd myset 5 e
(integer) 1
127.0.0.1:6379> ZRANGE myset 0 -1
1) "a"
2) "b"
3) "c"
4) "e"
127.0.0.1:6379> zadd myset 4 d
(integer) 1
127.0.0.1:6379> ZRANGE myset 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"
```

```bash
127.0.0.1:6379> zadd salary 2500 xiaohong 3000 xiaoming 2000 xiaoliu
(integer) 3
127.0.0.1:6379> ZRANGE salary 0 -1
1) "xiaoliu"
2) "xiaohong"
3) "xiaoming"
127.0.0.1:6379> ZREVRANGE salary 0 -1 #倒序
1) "xiaoming"
2) "xiaohong"
3) "xiaoliu"
127.0.0.1:6379> zadd salary 2000 xiaodu
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1
1) "xiaodu"
2) "xiaoliu"
3) "xiaohong"
4) "xiaoming"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf #从负无穷到正无穷排序
1) "xiaodu"
2) "xiaoliu"
3) "xiaohong"
4) "xiaoming"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 withscores
1) "xiaodu"
2) "2000"
3) "xiaoliu"
4) "2000"
5) "xiaohong"
6) "2500"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 #从负无穷到2500（包括）排序
1) "xiaodu"
2) "xiaoliu"
3) "xiaohong"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf (2500  #从负无穷到2500（不包括）排序
1) "xiaodu"
2) "xiaoliu"
127.0.0.1:6379> zrem salary xiaodu #移除xiaodu
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1
1) "xiaoliu"
2) "xiaohong"
3) "xiaoming"
127.0.0.1:6379> zcard salary #集合中元素个数
(integer) 3
127.0.0.1:6379> ZCOUNT salary 2400 2600 #所给区间的值的个数
(integer) 1
```

## 三种特殊数据类型

### geospatial 地理位置

#### geoaddd

该命令以采用标准格式的参数 x,y,所以经度必须在纬度之前。这些坐标的限制是可以被编入索引的，区域面积可以很接近极点但是不能索引。具体的限制，由 EPSG:900913 / EPSG:3785 / OSGEO:41001 规定如下：

有效的经度从-180 度到 180 度。
有效的纬度从-85.05112878 度到 85.05112878 度。
当坐标位置超出上述指定范围时，该命令将会返回一个错误。

```bash
127.0.0.1:6379> geoadd china 116.405285 39.904989 beijing
(integer) 1
127.0.0.1:6379> geoadd china 121.472644 31.231706 shanghai
(integer) 1
127.0.0.1:6379> geoadd china 106.504962 29.533155 chongqing
(integer) 1
127.0.0.1:6379> geoadd china 120.153576 30.287459 hangzhou
(integer) 1
127.0.0.1:6379> geoadd china 125.14904 42.927 xian
(integer) 1
127.0.0.1:6379> geoadd china 114.085947 22.547 shenzhen
(integer) 1
```

#### geopos

从 key 里返回所有给定位置元素的位置（经度和纬度）。

```bash
127.0.0.1:6379> geopos china beijing
1) 1) "116.40528291463851929"
   2) "39.9049884229125027"
127.0.0.1:6379> geopos china shanghai
1) 1) "121.47264629602432251"
   2) "31.23170490709807012"
```

#### geodist

返回两个给定位置之间的距离。

如果两个位置之间的其中一个不存在， 那么命令返回空值。

指定单位的参数 unit 必须是以下单位的其中一个：

m 表示单位为米。
km 表示单位为千米。
mi 表示单位为英里。
ft 表示单位为英尺。
如果用户没有显式地指定单位参数， 那么 GEODIST 默认使用米作为单位。

GEODIST 命令在计算距离时会假设地球为完美的球形， 在极限情况下， 这一假设最大会造成 0.5% 的误差。

```bash
127.0.0.1:6379> geodist china beijing shanghai #单位是m
"1067597.9668"

127.0.0.1:6379> geodist china beijing shenzhen km #单位是km
"1943.0240"
```

#### georadius

以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

范围可以使用以下其中一个单位：

m 表示单位为米。
km 表示单位为千米。
mi 表示单位为英里。
ft 表示单位为英尺。
在给定以下可选项时， 命令会返回额外的信息：

WITHDIST: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。
WITHCOORD: 将位置元素的经度和维度也一并返回。
WITHHASH: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。
命令默认返回未排序的位置元素。 通过以下两个参数， 用户可以指定被返回位置元素的排序方式：

ASC: 根据中心的位置， 按照从近到远的方式返回位置元素。
DESC: 根据中心的位置， 按照从远到近的方式返回位置元素。
在默认情况下， GEORADIUS 命令会返回所有匹配的位置元素。 虽然用户可以使用 COUNT <count> 选项去获取前 N 个匹配元素， 但是因为命令在内部可能会需要对所有被匹配的元素进行处理， 所以在对一个非常大的区域进行搜索时， 即使只使用 COUNT 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 COUNT 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

```bash
127.0.0.1:6379> GEORADIUS china 110 30 1000 km #以110,30为中心，1000km为半径范围内所有的城市
1) "chongqing"
2) "shenzhen"
3) "hangzhou"
127.0.0.1:6379> GEORADIUS china 121 31 500 km withcoord
1) 1) "hangzhou"
   2) 1) "120.15357345342636108"
      2) "30.28745790721532671"
2) 1) "shanghai"
   2) 1) "121.47264629602432251"
      2) "31.23170490709807012"
```

#### georadiusbymember

这个命令和 GEORADIUS 命令一样， 都可以找出位于指定范围内的元素， 但是 GEORADIUSBYMEMBER 的中心点是由给定的位置元素决定的， 而不是像 GEORADIUS 那样， 使用输入的经度和纬度来决定中心点

指定成员的位置被用作查询的中心。

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china shanghai 500 km
1) "hangzhou"
2) "shanghai"
```

#### geohash

该命令将返回 11 个字符的 Geohash 字符串
返回的 geohashes 具有以下特性：
他们可以缩短从右边的字符。它将失去精度，但仍将指向同一地区。
它可以在 geohash.org 网站使用，网址 http://geohash.org/<geohash-string>。查询例子：http://geohash.org/sqdtr74hyu0.

```bash
127.0.0.1:6379> geohash china shanghai
1) "wtw3sjt9vg0"
```
