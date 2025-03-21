# 基本概念

redis是一个开源的基于内存的数据存储系统，
它可以用于 ==数据库== ==缓存== ==消息队列== 等各种场景。
它可是目前最热门的Nosql之一


## 数据类型
redis支持五种基本数据类型，五种高级数据类型。
字符串String
列表List
集合Set
有序集合SortedSet
哈希Hash

消息队列Stream
地理空间Geospatial
HyperLogLog
位图Bitmap
位域Bitfiled


## 使用方式
CLI（command line interface）
API (Application programming interface)
GUI (图形化操作界面)


## 优势
性能极高
数据类型丰富
简单易用，支持所有主流编程语言
支持数据持久化，主从复制，哨兵模式等高可用特性。


## 安装部署
直接安装在docker中
docker run --name  my-redis -d redis 
以交互进入redis
docker exec -it my-redis redis-cli（ --raw）


如果是安装在Linux系统中的
redis-server
通过命令行连接redis
redis-cli
操作redis
set k2 v2


# 字符串String
```bash
set k1 v1 
setnx k1 v1 不存在才设置
get k1
exists k1 
del k1 
keys *****
flushall 删除所有键
expire name 10 设置过期时间
ttl name 查看过期时间
```


# List列表

![[Pasted image 20250225202815.png]]

```bash
lpush letter a b c
lrange letter 0 -1
#查看列表的长度
llen letter 
ltrim letter start stop
```

# Set集合
```shell
sadd course Redis
smembers course
sismember course Redis 
srem course Redis
```

# SortedSet 有序集合
```shell
zadd result 680 清华 660 北大 650 复旦 640 浙大
zrange result 0 -1 
zrange result 0 -1 withscores
zscore result 清华
zrank result 清华
```


# Hash哈希
```bash
hset person name Bruce
hset person age 27
hgetall person
hget person name
hdel person age 
hexists person name 
hkeys person 
hlen person 
```


# 消息发布订阅
```bash
# 订阅一个频道
subscribe channel1 
# 发布一个消息
publish channel1 
```