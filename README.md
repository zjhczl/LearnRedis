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
127.0.0.1:6379> SMEMBERS zj
1) "2"
2) "3"
3) "5"
4) "6"
5) "7"
127.0.0.1:6379> SMEMBERS zj2
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> SMOVE zj zj2 2 #把2从集合zj移动到zj2
(integer) 1
127.0.0.1:6379> SMEMBERS zj
1) "3"
2) "5"
3) "6"
4) "7"
127.0.0.1:6379> SMEMBERS zj2
1) "2"
2) "c"
3) "b"
4) "a"
```

```bash
127.0.0.1:6379> SMEMBERS zj
1) "d"
2) "a"
3) "e"
4) "f"
5) "b"
6) "c"
127.0.0.1:6379> SMEMBERS zj2
1) "h"
2) "g"
3) "e"
4) "k"
5) "f"
6) "j"
127.0.0.1:6379> SDIFF zj zj2 #set1相比与set2有哪些不同
1) "c"
2) "b"
3) "a"
4) "d"
127.0.0.1:6379> SDIFF zj2 zj
1) "k"
2) "j"
3) "h"
4) "g"
127.0.0.1:6379> SINTER zj zj2 #交集
1) "e"
2) "f"
127.0.0.1:6379> SUNION zj zj2 #并集
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
