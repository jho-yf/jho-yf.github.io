# Redis

## NoSQL概述

- 关系型数据：表格、行、列

- 传统的RDBMS
  - 结构化组织
  - SQL
  - 数据和关系都存在单独的表中
  - 操作数据的语句，数据定义语言
  - 严格的一致性
  - 基础的事务等
- NoSQL
  - 没有固定的查询语言
  - 键值对存储、列存储、文档存储、图形数据库
  - 最终一致性
  - CAP定理和BASE（异地多活）
  - 高性能、高可用、高可扩

### 什么是NoSQL

> NoSQL = Not Only SQL

泛指非关系型数据库，很多数据，如：个人信息、社交网络、地理位置，这些数据的存储不需要一个固定的格式 

### NoSQL的特点

- 方便扩展：数据之间没有关系，很好扩展
- 大数据量高性能：Redis每秒写8万次，读11万次
- 数据类型是多样型的，而不需要设计数据库，随取随用 

### NoSQL的四大分类

#### KV键值对

- redis
- Tair
- memecache

#### 文档型数据库（bjson格式）

- MongoDB
  - 基于分布式文件存储的数据库，C++编写，主要用于处理大量的文档
  - 介于关系型数据库和非关系型数据库中间的产品
  - 是非关系型数据中功能最丰富，最像关系型数据库的
- CouchDB

#### 列存储数据库

- HBase
- 分布式文件系统

#### 图关系数据库

- 存放关系，如：社交网络、广告推荐
- Neo4J
- InfoGrid

#### 对比

| 分类                      | 数据模型                                 | 优点                                                         | 缺点                                                         | 典型应用场景                                                 |
| ------------------------- | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 键值(Key-Value)存储数据库 | Key指向Value的键值对，通常用hash表来实现 | 查找速度快                                                   | 数据无结构化(通常只被当作字符串或者二进制数据)               | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等 |
| 列存储数据库              | 以列簇式存储，将同一列数据存在一起       | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 | 分布式的文件系统                                             |
| 文档型数据库              | Key-Value对应的键值对，Value为结构化数据 | 数据结构要求不严格，表结构可变(不需要像关系型数据库一样需预先定义表结构) | 查询性能不高，而且缺乏统一的查询语法                         | Web应用                                                      |
| 图形(Graph)数据库         | 图结构                                   | 利用图结构相关算法(如最短路径寻址，N度关系查找等)            | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群方案 | 社交网络，推荐系统、公共交通网络，地图及网络拓谱等           |



### NoSQL适用场景

- 对数据高并发的读写
- 海量数据的读写
- 对数据高可扩展性



## Redis

### 概述

> Redis（Remote Dictionary Server），即远程字典服务

- 一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持续化的日志型、Key-Value数据库，并提供多种语言的API
- redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现`master-slave（主从）同步`

> 官网：https://redis.io/
>
> 中文网：http://www.redis.cn/
>
> Github： https://github.com/redis/redis
>
> redis命令：http://www.redis.cn/commands.html



#### 基础知识

- redis是单线程的，所有的数据都放在内存中
- redis是基于内存操作，CPU不是redis的性能瓶颈，redis的瓶颈是机器的内存和网络带宽
- 作用：
  - 数据库
  - 缓存
  - 消息中间件MQ



#### Redis的使用场景

- 内存存储、持久化，内存中是断电即失，所以持久化很重要（rdb、aof）
- 效率高，可用于高速缓存
- 发布订阅系统
- 地图信息分析
- 计数器（浏览量）、计时器
- 等



#### 特性

- 多样化的数据类型

- 持久化

- 集群

- 事务

- 原子性

  - 所谓原子操作是指不会被线程调度机制打断的操作，这种操作一旦开始，就一直运行到结束，中间不会有任何其他线程切换
    - Redis是单线程的，能够在单条指令中完成的操作都可以认为是“原子操作”，以为中断智能发生于指令间
    - 在多线程中，不能再被其他进程（线程）打断的操作就叫原子操作

  

#### 安装目录

- redis-benchmark：性能测试工具
- redis-check-aof：aof文件修复工具
- redis-check-rdb：rdb文件修复工具
- redis-sentinel：redis集群使用
- redis-server：redis服务器启动命令
- redis-cli：redis客户端操作入口



#### 性能测试

- redis-benchmark

```sh
root@99f244b2ed7f:/data# redis-benchmark --help
Usage: redis-benchmark [-h <host>] [-p <port>] [-c <clients>] [-n <requests>] [-k <boolean>]

 -h <hostname>      Server hostname (default 127.0.0.1)
 -p <port>          Server port (default 6379)
 -s <socket>        Server socket (overrides host and port)
 -a <password>      Password for Redis Auth
 --user <username>  Used to send ACL style 'AUTH username pass'. Needs -a.
 -c <clients>       Number of parallel connections (default 50)
 -n <requests>      Total number of requests (default 100000)
 -d <size>          Data size of SET/GET value in bytes (default 3)
 --dbnum <db>       SELECT the specified db number (default 0)
 --threads <num>    Enable multi-thread mode.
 --cluster          Enable cluster mode.
 --enable-tracking  Send CLIENT TRACKING on before starting benchmark.
 -k <boolean>       1=keep alive 0=reconnect (default 1)
 -r <keyspacelen>   Use random keys for SET/GET/INCR, random values for SADD,
                    random members and scores for ZADD.
  Using this option the benchmark will expand the string __rand_int__
  inside an argument with a 12 digits number in the specified range
  from 0 to keyspacelen-1. The substitution changes every time a command
  is executed. Default tests use this to hit random keys in the
  specified range.
 -P <numreq>        Pipeline <numreq> requests. Default 1 (no pipeline).
 -e                 If server replies with errors, show them on stdout.
                    (no more than 1 error per second is displayed)
 -q                 Quiet. Just show query/sec values
 --precision        Number of decimal places to display in latency output (default 0)
 --csv              Output in CSV format
 -l                 Loop. Run the tests forever
 -t <tests>         Only run the comma separated list of tests. The test
                    names are the same as the ones produced as output.
 -I                 Idle mode. Just open N idle connections and wait.

Examples:

 Run the benchmark with the default configuration against 127.0.0.1:6379:
   $ redis-benchmark

 Use 20 parallel clients, for a total of 100k requests, against 192.168.1.1:
   $ redis-benchmark -h 192.168.1.1 -p 6379 -n 100000 -c 20

 Fill 127.0.0.1:6379 with about 1 million keys only using the SET test:
   $ redis-benchmark -t set -n 1000000 -r 100000000

 Benchmark 127.0.0.1:6379 for a few commands producing CSV output:
   $ redis-benchmark -t ping,set,get -n 100000 --csv

 Benchmark a specific command line:
   $ redis-benchmark -r 10000 -n 10000 eval 'return redis.call("ping")' 0

 Fill a list with 10000 random elements:
   $ redis-benchmark -r 10000 -n 10000 lpush mylist __rand_int__

 On user specified command lines __rand_int__ is replaced with a random integer
 with a range of values selected by the -r option.
```



### key的命名规范

- 数据库中的数据key命名惯例

> 表名 : 主键名 : 主键值 : 字段名
>
> order: id:2016035144149:price
>
> people: id:2016035144149:name
>
> news: id:2016035144149:title



 ```shell
set user:id:2016035144149:fans 1231312
set user:id:2016035144149:blogs 4324
set user:id:2016035144149:focuss 45

set user:id:2016035144149 {id:2016035144149, name:jho, fans:1231312, blogs:4324, focuss:45}
 ```



### key操作常用命令

redis中的key操作

```bash
keys *				# 查看当前库所有key
exists key			# 片段某个key是否存在
type key			# 查看key的类型
del key				# 删除指定的key
unlink key			# 根据value选择非阻塞删除，仅将key从keyspace元数据中删除，真正的删除在后续的异步操作中
expire key 10		# 为给定key设置过期时间（秒）
ttl key				# 查看key的过期时间（秒）	-1代表永不过期，-2代表已过期
```

数据库操作

```bash
select index		# 切换数据库
select 0			# 切换到索引为0的数据库

dbsize				# 查看DB大小，查看key的数量

flushdb				# 清空当前数据库
flushall			# 清空所有数据库

move key db			# 移动某个key到其他数据库
move name 1			# 将name移动到索引为1的数据库
```





### Redis五大数据类型

String、List、Set、Hash、Zset

#### String 字符串

- String类型是**二进制安全**的。意味着Redis的String可以包含任何数据，比如JPG图片或者序列化对象

- 字符串最大长度512M

  ![String数据结构](https://yf-pic-repo.oss-cn-guangzhou.aliyuncs.com/yf-pic-repo/202204041610971.png)

- 如图，字符串在内存中分配的实际空间**capacity**一般会高于实际字符长度**len**。当字符串长度小于1M时，扩容都是加倍现有空间。如果超过1M，扩容时会多扩容1M的空间。

##### 基本命令

```shell
# 追加往对应key的值字符串
append key value
append name ",xujihong"

# 获取字符串长度
strlen key

# 自增
incr key	# 若对应的值是不是数字类型，提示“(error) ERR value is not an integer or out of range”
# 自增increment的值
incrby key increment
incrby age 10

# 自减
decr key
# 自减decrement的值
decrby key decrement

# 截取字符串 [start, end]
getrange key start end

# 替换下标为offset开始的字符串
setrange key offset value

# setex (set with expire) 设置key的过期时间以及值
setex key seconds value

# setnx (set if not exist) 不存在则设置
setnx key value

# 批量添加
mset key value [key value ...]

# 批量添加 保持原子性，要么全部成功，要么全部失败
msetnx key value [key value ...]

# 批量查询
mget key [key ...]

# 先get再set 如果不存在值，返回nil，如果存在值，则返回值再set
getset key value
```

##### 数据结构

String的数据结构为简单动态字符串（Simple Dynamic String，SDS），是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配

##### 使用场景

- 计算器
- 统计多单位的数量
- 粉丝数
- 对象缓存存储



#### List 列表

- 在redis里面，list可以当作栈、队列、阻塞队列
- redis的链表实际上是一个双向链表，在两边插入或者修改值，效率最高，通过索引操作中间的节点性能比较差

##### 基本命令

```shell
# 将一个或多个值插入到列表头部（左）
lpush key element [element ...]

# 将一个或多个值插入到列表尾部（右）
rpush key element [element ...]

# 查询列表的元素
lrange key start stop
# 查询所有元素
lrange jho 0 -1
# 查询指定范围元素
lrange jho 0 1

# 获取并移除列表头部（左）
lpop key
# 获取并移除列表尾部（右）
rpop key

# 通过下标获取list中的某一个值
lindex key index

# 获取列表长度
llen key

# 移除列表中指定个数的value
lrem key count element

# 截取掉指定范围的元素
ltrim key start stop

# 移除列表尾部元素，并添加到到新的列表的头部
rpoplpush source destination

# 更新指定下标的值，如果列表不存在则会报错
lset key index element

# 将某个元素插入到列表中某个元素的前面或后面
linsert key before|after pivot element

# 阻塞获取列表尾部（右）元素，timeout设置为0，则一直阻塞直到取到元素
brpop key [key ...] timout

# 阻塞获取列表头部（左）元素，timeout设置为0，则一直阻塞直到取到元素
blpop key [key ...] timeout

# 按从左到右的顺序，用下标获取元素
lindex key index	
```

##### 数据结构

Redis List的数据结构为quickList 

![带头结点的双向循环链表](https://yf-pic-repo.oss-cn-guangzhou.aliyuncs.com/yf-pic-repo/202204041610002.png)

##### 使用场景

- 消息排队
- 消息队列
- 栈



#### Set 集合

- Set集合里面的元素无序且不重复
- 自动排重

##### 基本命令

```shell
# 往set中添加元素
sadd key member [member ...]

# 获取set的元素个数
scard key

# 移除set中的元素
srem key member [member ...]

# 获取key中所有member
smembers key

# 从set里随机获取count个元素,count不填默认1
srandmember key [count]

# 随机删除并取出set中的count个元素
spop key [count]

# 将set里面的元素移动到另一个set中
smove source destination member

# 求集合间的差集
sdiff key [key ...]

# 求集合间的交集
sinter key [key ...]

# 求集合间的并集
sunion key [key ...]
```

##### 数据结构

`redis`的`set`是`String`类型的无序集合，底层是value为null的Hash表，所有添加、删除、查找的时间复杂度都是O(1)

##### 使用场景

并集：微博共同关注、共同好友、共同爱好



#### Hash 哈希

- 相当于Java的Map集合
- Hash是一个String类型的field和value的映射表，适合对象的存储

##### 基本命令

```shell
# 添加一个或多个k-v
hset key field value [field value ...]

# 获取一个字段值
hget key field

# 获取多个字段值
hmget key field [field ...]

# 获取全部的数据
hgetall key

# 删除hash指定key字段
hdel key field [field ...]

# 获取hash的field长度
hlen key

# 判断hash的field是否存在
hexists key field

# 列出一个hash的所有field
hkeys key

# 列出一个hash的所有value
hvals key

# 为哈希表 key 中的指定字段的整数值加上增量 increment
hincrby key field increment
# 为哈希表 key 中的指定字段的浮点数值加上增量 increment
hincrbyfloat key field increment

# 如果存在则不set，不存在则set
hsetnx key field value
```

##### 数据结构

Hash类型对应的数据结构有两种：
- ziplist 压缩列表
- hashtable 哈希表

当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable

##### 使用场景

- 用户信息、经常变动的信息的保存



#### Zset 有序集合

- 在set的基础上添加根据score排序
- set的命令zset都可以用
- 集合中的成员是唯一的，但score是可以重复的

##### 基本命令

```shell
# 往set里添加一个元素
zadd key [NX|XX] [CH] [INCR] score member [score member ...]

zrange key min max [BYSCORE|BYLEX] [REV] [LIMIT offset count] [WITHSCORES]

# 排序查询
# min 最小值（-inf表负无穷）
# max 最大值（+inf表正无穷）
# with 
zrangebyscore key min max [withscores] [limit offset count]
# 从小到大查询所有元素并显示score
zrangebyscore salary -inf +inf withscores 
# 从小到大查询score小于2500的元素
zrangebyscore salary -inf 2500 withscores 

# 从大到小排序查询
zrevrange key start stop [withscores]
# 从大到小查询所有元素并显示score
zrevrange salary 0 -1 withscores

# 获取指定区间的元素数量
zcount key min max
```

##### 数据结构

- zset底层使用了两个数据结构
  - hash：用于关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值
  - 跳跃表：用于给元素value进行排序，根据score的范围获取元素列表 

##### 使用场景

- 成绩、工资排序
- 加权判断
- 排行榜
- 取Top N（前n个）



### 三种特殊数据类型

- geospatial
- hyperloglog
- bitmaps

#### geospatial 地理位置

- 推送地理位置信息，计算两地之间的距离，方圆几里的人
- GEO底层是基于zset实现的，所以可以通过zset命令来操作geo

##### 基本命令

- 有效经度：-180度到180度
- 有效纬度：-85.05112878度到85.05112878度 

```shell
# 添加地理位置
geoadd key longitude latitude member [longitude latitude member ...]
# 示例
geoadd china:city 114.05 22.52 shenzhen

# 获取坐标值（经度和纬度）
geopos key member [member ...]
# 示例
geopos china:city shenzhen

# 获取两个位置的直线距离
# m 表示单位米 - 默认
# km 表示单位千米
# ft 表示单位英里
# mi 表示单位英尺
geodist key member1 member2 [m|km|ft|mi]
# 示例
geodist china:city shenzhen suibian km

# 获取某个经纬度附近的其他位置
# withcoord 显示其他位置的经纬度
# withdist 显示该位置到其他位置的直线距离
# count 显示多少个
georadius key longitude latitude radius m|km|ft|mi [withcoord] [withdist] [withhash] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
# 示例
georadius china:city 114.03 22.3 100 km

# 获取某个位置附近的其他位置
georadiusbymember key longitude latitude radius m|km|ft|mi [withcoord] [withdist] [withhash] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
# 示例
geeradiusbymember china:city shenzhen 100 km

# 返回11个字符的geohash字符串
# 即将二维的经纬度转换为一维的字符串，如果两个字符串越接近则两个位置距离越近
geohash key member [member ...]
# 示例
geohash china:city shenzhen
> 1) "ws10578st80"

# 查看所有元素
zrange china:city 0 -1
# 移除指定元素
zrem china:city shenzhen
```

##### 使用场景

- 朋友定位
- 附近的人
- 打车距离计算



#### Hyperloglog 基数统计

> 基数：不重复的元素的个数

- 存放一组元素，且里面的元素不会重复
- 与set相比较：
  - 优点：占用的内存固定，2^64不同的元素，只需要12KB内存
  - 缺点：存在0.81%的错误率
  - 如果允许容错，那么一定使用Hyperloglog。如果不允许容错，就是用set或其他数据结构

##### 基本命令

```shell
# 添加元素到组里，成功返回1，否则返回0
pfadd key element [element ...]

# 统计组里元素的基数数量
pfcount key [key ...]

# 合并多个组
pfmerge destkey sourcekey [sourcekey ...]
```

##### 使用场景

- 网站访问量统计（允许容错）



#### Bitmap 位图

- bitmap位图，数据结构，通过操作二进制位来记录，只有0或1两个状态
- 常用于统计两个状态的数量

##### 基本命令

```shell
# 设置一个bitmap
setbit key offset value

# 获取一个bitmap
getbit key offset

# 统计个数 
# start和end参数中，-1表示最后一个位，-2表示倒数第二个位
bitcount key [start end]

# 对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上
# operation: and or xor not
bitop operation destkey key [key ...]
```

##### 数据结构

- bitmaps本身不是一种数据类型，实际上它就是字符串，但是它可以对字符串的位进行操作

##### 使用场景

- 统计用户：活跃，不活跃。登录，未登录。365天打卡状态



### 事务

- Redis的命令是保证原子性的，但事务不保证原子性
- redis事务的本质是**一组命令的集合**，在事务的执行过程中，会按照顺序执行，可以理解为一个打包的批量执行脚本
- redis事务具有一次性、顺序性、排他性

#### redis事务的特性

- 单独的隔离操作：事务在执行过程中，不会被其他客户端发送来的命令请求打断
- 没有隔离级别的概念：开启事务后，所有在事务中的命令并不会被直接执行，只有发起执行命令`exec`之后才会执行
- 不保证原子性：事务中如果有一条命令执行失败（非语法错误），其后的命令仍会执行，没有回滚



#### 事务中的错误

使用事务时可能会遇上以下两种错误：
- 事务在执行`EXEC`之前，入队的命令可能会出错：命令可能会产生语法错误（参数数量错误，参数名错误，等等），或者其他更严重的错误，比如内存不足（如果服务器使用 `maxmemory` 设置了最大内存限制的话）
- 命令可能在`EXEC`调用之后失败：事务中的命令可能处理了错误类型的键

对于发生在`EXEC`执行之前的错误，redis拒绝执行并自动放弃这个事务。

`EXEC`命令执行之后所产生的错误即使事务中有某个/某些命令在执行时产生了错误， 事务中的其他命令仍然会继续执行。

#### 使用redis事务

- 开启事务（multi）
- 命令入队
- 执行事务（exec）



### 事务锁

- **悲观锁**：悲观锁假设每次线程获取数据都会去修改数据。所以每当有线程拿数据的时候都会进行上锁，当其他线程需要访问该数据的时候，都需要阻塞挂起，直到解锁之后才能访问得到。
  - 适用于写多的场景
- **乐观锁**：乐观锁假设每次线程获取数据都不会去修改数据，所以不进行上锁。但在更新时会判断其他线程在这之前有没有对数据进行修改，一般会使用`版本号机制`或`CAS(compare and swap)算法`实现。如果之前有其他线程对数据进行修改，则操作失败后事务回滚、提示。
  - 适用于读多写少的场景

#### 常用命令

redis的`watch`就是乐观锁操作

```shell
# 开启事务
multi

# 取消事务
discard

# 执行事务
exec

# watch可以当作redis的乐观锁操作，监控某个key
# 当前线程watch一个key的时候，若其他线程去修改当前key，则当前线程事务命令执行失败
watch key [key ...]

# 取消监视
unwatch
```



### Jedis

- Redis官方推荐的Java连接工具，Java操作Redis的中间件
- Jedis和Lettuce
  - Jedis：采用直连，多个线程操作是不安全的，如果想要避免不安全，需要使用jedis pool连接池
  - Lettuce：采用Netty，实例可以在多个线程中共享，不存在线程不安全的情况，可以减少线程数量

#### 常用API

- String
- List
- Set
- Hash
- Zset



### SpringBoot整合

#### 整合步骤

在`pom.xml`中引入`spring-boot-starter-data-redis`依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

在`application.yaml`中配置redis

```yaml
spring:
  redis:
    host: localhost			# redis服务器地址
    port: 6379				# redis服务器连接端口
    password: 666666		# redis服务器密码
    database: 0				# redis数据库索引（默认为0）
    timeout: 1800000		# 连接超时时间（毫秒）
    lettuce:
    	pool: 
    		max-active: 20	# 连接池最大连接数（使用负值表示没有限制）
    		max-wait: -1	# 最大阻塞等待时间（使用负值表示没有限制）
    		max-idle: 5		# 连接池中的最大空闲数
    		min-idle: 0		# 连接池中的最小空闲数
```

添加redis配置类

```java
@Configuration
public class RedisAutoConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);

        // 设置具体的序列化方式
        Jackson2JsonRedisSerializer<Object> serializer = new Jackson2JsonRedisSerializer<Object>(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.setDefaultTyping(ObjectMapper.DefaultTypeResolverBuilder.noTypeInfoBuilder());
        serializer.setObjectMapper(om);

        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // key采用String方式序列化
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String方式序列化
        template.setHashKeySerializer(stringRedisSerializer);
        // value采用jackson方式序列化
        template.setValueSerializer(serializer);
        // hash的value采用jackson方式序列化
        template.setHashValueSerializer(serializer);
        template.afterPropertiesSet();

        return template;
    }

}
```

使用`RedisTemplate`

```java
@SpringBootTest
@Slf4j
public class RedisSpringBootApplicationTests {

    @Autowired
    @Qualifier("redisTemplate")
    private RedisTemplate redisTemplate;

    @Test
    void testRedisTemplate() {
        String key = "name";
        redisTemplate.opsForValue().set(key, "乙方小弟");
        log.info("value={}", redisTemplate.opsForValue().get(key));
    }
```



#### 源码

```java
// RedisAutoConfiguration
@Bean
@ConditionalOnMissingBean(
    name = {"redisTemplate"}	// 可以定义一个redisTemplate来替换
)
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    RedisTemplate<Object, Object> template = new RedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}

@Bean
@ConditionalOnMissingBean
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
    StringRedisTemplate template = new StringRedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
```



### redis.conf配置文件

- 配置文件unit单位大小写不敏感

#### INCLUDES

- 包含一个或者多个配置文件



#### NETWORK

```conf
bind 192.168.1.100 10.0.0.1		# 绑定的ip

protected-mode yes|no			# 是否开启保护模式

port 6379						# 端口绑定
```



#### GENERAL

 ```conf
daemonize yes|no				# 是否以守护进程的方式运行，默认是no

pidfile /var/run/redis_6379.pid	# 如果以守护进程的方式启动，则需要指定pid文件

# 日志
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice					# notice适用于生产环境

logfile ""						# 设置日志文件位置

databases 16					# 数据库的数量，默认16

always-show-logo yes			# 是否显示redis logo
 ```



#### SNAPSHOTTING

- redis是个内存数据库，不做持久化就造成数据丢失
- SNAPSHOTTING，快照。即持久化，在规定时间内执行了多少次操作则会持久化到文件
  - `.rdb`
  - `.aof`

```conf
# 持久化策略
save 900 1			# 如果900秒内，至少有1个key进行修改则进行持久化
save 300 10			# 如果300秒内，至少有10个key进行修改则进行持久化
save 60 10000		# 如果60秒内，至少有10000个key进行修改则进行持久化

stop-writes-on-bgsave-error yes|no		# 持久化出错，是否继续进行工作

rdbcompression yes|no					# 是否压缩rdb文件，此操作会消耗cpu资源

rdbchecksum yes							# 保存rdb文件时，是否进行错误的检查校验

dir ./									# rdb文件保存的目录
```



#### REPLICATION

- 主从复制



#### SECURITY

```conf
requirepass foobared			# 设置redis密码
```



#### CLIENTS

- 限制客户端

```conf
maxclients 10000			# 限制客户端最大连接数
```



#### MEMORY MANAGEMENT

- 内存管理

```conf
maxmemory <bytes>				# 配置最大的内存容量

maxmemory-policy noeviction		# 内存达到上限之后的处理策略
	# noeviction: 不删除策略, 达到最大内存限制时, 如果需要更多内存, 直接返回错误信息。（默认值）
    # allkeys-lru: 所有key通用; 优先删除最近最少使用(less recently used ,LRU) 的 key。
	# volatile-lru: 只限于设置了 expire 的部分; 优先删除最近最少使用(less recently used ,LRU) 的 key。
	# allkeys-random: 所有key通用; 随机删除一部分 key。
	# volatile-random: 只限于设置了 expire 的部分; 随机删除一部分 key。
	# volatile-ttl: 只限于设置了 expire 的部分; 优先删除剩余时间(time to live,TTL) 短的key。
```



#### APPEND ONLY MODE 

- aof配置

```conf
appendonly no			# 默认不开启aof模式，默认使用rdb模式持久化

appendfilename "appendonly.aof"			# 持久化文件的名字

appendfsync everysec	# 每秒执行一次sync，可能会丢失这一秒的数据
appendfsync always		# 每次修改都会sync，小号性能
appendfsync no			# 不执行sync，操作系统自己同步数据，速度最快
```





### Redis持久化

- RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储。
- Redis持久化是最简单的高可用方法，主要作用是数据备份，即将数据存储在硬盘，保证数据不会因进程退出而丢失。

#### RDB（Redis DataBase）

- RDB 方式，是将 redis 某一时刻的数据持久化到磁盘中，是一种快照式的持久化方法。
- Redis默认使用RDB方式做持久化
- 大部分时候使用默认配置即可

##### 相关配置

```conf
################################ SNAPSHOTTING  ################################

dbfilename dump.rdb 			# 持久化文件名字，在生产环境常常需对该文件进行备份

# 持久化策略
save 900 1			# 如果900秒内，至少有1个key进行修改则进行持久化
save 300 10			# 如果300秒内，至少有10个key进行修改则进行持久化
save 60 10000		# 如果60秒内，至少有10000个key进行修改则进行持久化
```

##### 触发机制

- save规则满足
- 执行flushall命令
- 正常退出redis

##### 优缺点

- 优点：适用于大规模数据的恢复
- 缺点：
  - 需要一定的时候间隔进行进程操作，如果redis意外宕机，最后一次修改的数据就丢失了（所以rdb适用于对数据完整性要求不高的场景）
  - fork进程的时候，会占用一定的内存空间



#### AOF（Append Only File）

- 以日志的形式将所有写命令都记录下来，恢复的时候将这个文件所有命令执行一遍
- 默认是不开启的

##### 相关配置

```bash
appendonly no		# 是否开启aof模式
```

##### 工具

- 如果aof文件损坏，redis无法正常启动，此时可以使用`redis-check-aof --fix`对aof文件进行修复

##### 优缺点：

- 优点：
  - 每次修改都同步，保证数据完整性
  - 每秒同步一次
- 缺点：
  - aof数据文件远远大于rdb文件，修复速度也比rdb慢
  - aof运行效率比rdb慢
  - 可能会丢失一秒的数据



### Redis发布订阅（pub/sub）

- redis客户端可以订阅任意数量的频道

#### 命令

```bash
# 订阅一个或多个符合给定模式的频道
PSUBSCRIBE pattern [pattern ...]

# 查看订阅与发布系统状态
PUBSUB subcommand [argument [argument ...]]

# 将信息发送到指定的频道
PUBLISH channel message

# 退订所有给定模式的频道
PUNSUBSCRIBE [pattern [pattern ...]]

# 订阅给定的一个或多个频道的消息
SUBSCRIBE channel [channel ...]

# 推定指定频道
UNSUBSCRIBE [channel [channel ...]]
```



### Redis主从复制

- 主从复制是指将主节点（master/leader）Redis服务器的数据复制到从节点（slave/follower）服务器
- 数据的复制是单向的，只能由主节点到从节点，Master以写为主，Slave以读为主
- 主节点只能有一个，一个主节点可以对应零个或多个从节点

#### 使用多节点

- 结构上，单个redis服务器会发生单点故障，一台服务器需要处理的请求负载过多，压力过大
- 容量上，单台redis服务器内存容量有限，单台redis最大使用内容不应该超过20G

#### 主从复制的作用

- 数据冗余：主从复制可以实现数据热备份，是持久化之外的另外一种数据冗余方式
- 故障恢复：当主节点出现故障时，其他节点可以提供服务，实现故障恢复
- 负载均衡：在主从复制的基础上，配合读写分离，有主节点提供写服务，由从节点提供读服务，负担服务器负载
- 高可用

#### 基本命令

```bash
# 查看当前redis服务的信息
info relication

    # Replication
    role:master						# master节点
    connected_slaves:0				# 没有从节点
    master_failover_state:no-failover
    master_replid:75072170e3007900c9548975ad40b2c53d4286f6
    master_replid2:0000000000000000000000000000000000000000
    master_repl_offset:0
    second_repl_offset:-1
    repl_backlog_active:0
    repl_backlog_size:1048576
    repl_backlog_first_byte_offset:0
    repl_backlog_histlen:0
    
# 认领master节点
slaveof host port
```

#### 环境配置

- 默认情况下，每台redis服务器都是主节点，只需要配置从节点，不需要配置主节点

#### 主从复制原理



### 哨兵模式 

- 一主二从三哨兵