##### 数据类型

###### 字符串string

赋值：set key value

取值：get key

取值后赋值：getset key value

数值增减：

  自增：incrby key numbers;若key不存在，则创建该key并赋值为0，然后自增numbers，若存在则value自增numbers；

  自增1：incr key; 若key不存在，则创建该key并赋值为0，然后自增1成为1，若存在则value自增1；

  自减：decrby key numbers;若key不存在，则创建该key并赋值为0，然后自减numbers，若存在则value自减numbers；

  自减1：decr key;若key不存在，则创建该key并赋值为0，然后自减1成为-1，若存在则value自减1；

删除：del key

扩展：append key string;给value追加内容

###### 哈希 hash

赋值：hset key field value；给单个field赋值；hmset key field value [field value ...]；给多个field赋值。

取值：hget key field；获取单个field值；hmget key field [field ...]；获取多个field值；hgetall key；获取同一个key下所有的field和value。

增加数字：hincrby key field number；给指定key下的field增加number。

删除：hdel key field [field ...]；删除指定key下field[可删除多个field]；del key；删除整个key。

自学命令：hexists key field；查看key中的field是否存在，存在返回1，不存在返回0；

hlen key；查看key中有多少个field；

hkeys key；获取key中所有的field；

hvals key；获取key中所有的value；

###### 字符串列表 list

list数据类型：

- arraylist使用数据的方式：根据索引查询速度非常快；新增和删除操作涉及到位移操作，比较慢。
- linklist使用双向连接方式：每个元素都记录了前后元素的指针，删除和新增只需修改前后指针，操作较快。

常用命令：

- 两端添加：

  lpush key value [value ...]；尾插

  rpush key value [value ...]；头插

- 查看列表：

  lrange key start stop;

- 指定位置push：

  lset key index value；在第index位置插入value，位置从0开始。

- 指定value插入值：

  linsert key before|after pivot value；在列表从左到右，第一个等于pivot这个值的前面或者后面，插入value。

- 两端弹出：

  lpop key；删除列表最左边的value

  rpop key；删除列表最右边的value

  rpoplpush source distination；从source列表右边删除一个value，并把这个value存储进入distination列表中，适用于消息发布过程中的备份操作。

- 获取列表元素个数：

  llen key；

- 扩展命令：

  lpushx key value；若列表存在，则从左端插入push values进入列表中，否则返回0。

  rpushx key value；若列表存在，则从右端插入push valuse进入列表中，否则返回0。

  lrem key count value；count>0，从列表左端开始删除，值等于value，共删除count个；count<0，从列表右端开始删除，值等于value，共删除count个；count=0，删除整个列表中所有值等于value的。
###### 字符串集合set

 常用命令：

- 添加删除元素：

  sadd key member [member ...]；可以直接新建集合并添加元素。

  srem key member [member ...]；删除集合中的某几个元素。

  del key；删除整个集合。

- 获取集合中的元素

  smembers key；

- 差集运算

  sdiff key1 [key ...]；求集合key1与其他集合的差集。

  sdiffstore destination key1 [key ...]；求集合key1与其他集合的差集，并把结果存储在destination集合中。

- 交集运算

  sinter key [key ...]；求多个集合的交集。

  sinterstore destionation key [key ...]；求多个集合的交集，并把结果存储在destination集合中。

- 并集运算

  sunion key [key ...]；求多个集合的并集。

  sunionstore destination key [key...]；求多个集合的并集，并把结果存储在destination集合中。

- 扩展命令

  sismember key member；查看member在key中是否存在。

  srandmember key [count]；集合key中随机返回count个元素。

  scard key；查看集合成员个数。

###### 有序字符串集合sorted set

常用命令：

- 获取元素：

  zscore key member；获取有序集合中的某个元素的score值。

  zrange key start stop [withscores]；查看集合中start到stop中的中的元素，[withscores]是否显示score。

  zrangebyscore key mim max [withscores] [limit offset count]；

- 添加元素

  zadd key score member [score member ...]；

  ```redis
  zadd myzset 1433 ma 3306 yu 1522 ha 3330 la
  ```

- 删除元素

  zrem key member [member ...]；删除key有序集合中的member元素。

  zremrangebyscire key min max；删除key有序集合中min到max之间的元素。

- 扩展查询

  zincrby key increment member；key中的元素自增。

  zscore key member；查看key中member中的score值。

  zcount key min max；查看key中score值从min到max中有多少个。

###### 通用操作

keys *：查看所有keys。

del key [key...]：删除多个key。

exists key：判断某个key是否存在，存在返回1，不存在返回0。

rename key newkey：重命名某个key。

expire key seconds：设置某个key的生命周期，过了这个时间为过期数据(过期数据会被清理)。

ttl key：查看该key的生命周期还剩多少秒。(pttl：以毫秒形式返回)。

type key：查看key类型。

##### redis特性

###### 多数据库

redis最多支持16个数据库，下标0-15表示第几个数据库，默认在0号数据库，切换数据库可使用 select dbnumber来切换，也可以通过move来移动key从当前数据移动到指定的数据库。

###### 事务

事务的指令：muli、exec、discard。redis中，如果某个命令执行失败，后面的命令还会继续执行。muli，开启事务，这个指令后的指令默认在同一个事务内，exec等同于提交，discard等同于回滚。

##### redis的持久化

RDB持久化：默认支持，在指定时间内，把内存的数据写入磁盘。

AOF持久化：以日志的形式记录每一个操作，启动的时候重新执行所有log。

无持久化：不进行持久化，则认为redis的作用为缓存，无需持久化数据

RDB和AOF同时使用

##### RDB持久化

1. 优势

   - redis数据库仅包含一个文件，对文件备份是非常方便的，若系统出现灾难比较容易回复。

   - 灾难恢复时，备份文件可以单端转移到其他存储介质。

   - 数据量很大的时候，启动速度快。

2. 劣势

   - 不能保证数据无丢失，数据丢失时间 = 当前时间 - 最近备份时间。

   - 子进程完成持久化工作，如果数据集很大的时候，可能会造成短时间redis所在服务器停止对外服务。

3. 配置

   - RDB默认配置文件中包含，可以查看redis.conf文件中关于save的设置。(windows为redis.windows.conf)

     ![redis.windows.conf](C:\Users\mayu.colin\AppData\Roaming\Typora\typora-user-images\image-20201125153639912.png)

     save 900 1：900秒内至少有一个数据发生变，则进行持久化。
     
     ![savefilename](C:\Users\mayu.colin\AppData\Roaming\Typora\typora-user-images\image-20201125154132001.png)
     
     dbfilename是是命名当前持久文件的名字，dir是定义当前持久化文件的存放路径。
     
##### AOF持久化

1. 优势

   - 更高的数据安全性：每秒同步，最高丢失1s数据；每操作数同步，每次发生数据的变化都会立即记录到磁盘中，性能最低。

   - append追加文件备份：备份过程中出现问题，不会破坏之前的日志备份；如果写入了一半数据，然后出现系统崩溃的问题，在redis的启动之前，可以通过redis_check_aof工具解决数据一致性问题。

   - 如果日志备份过大：redis会自动启动日志重写机制，append过程中，会把备份数据写入到老的备份文件中，并且用一个新文件，记录此期间修改数据语句。

   - AOF包含一个格式清晰的数据修改操作语句的日志文件。

2. 劣势

   - 相同数量的数据集文件，比RDB的要大。

   - AOF效率低于RDB。

   - 需要人员配置，非默认配置。

3. 配置

   - 在redis.conf文件中配置如下内容：

     - appendonly yes ：启动appendonly，开启AOF备份。

     - appendfilename "appendonly.aof" ：AOF备份的文件名。

     - appendfsync always ：每个修改操作同步备份一次。

     - appendfsync no：不同步。

   - 测试配置
     - 简单测试案例：redis中配置AOF，选择每操作一次就备份的机制，增删改数据后，执行flushall，然后通过备份文件来恢复数据到flushall之前。
     - 步骤
       - 启动AOF，选择每选择一次就备份：appendonly yes；appendfsync always。
       - 重启redis：/usr/local/redis/bin/ redis-cli shutdown；/usr/local/redis/bin/redis-server /etc/redis.conf
       - 造数据：set name mayu;set age 1000;set num 999;
       - 执行fulshall
       - 处理备份文件：vim /usr/loacl/redis/appendonly.aof；删除fiushall的操作记录。
       - 重启数据库：/usr/local/redis/bin/redis-cli shutdown；/usr/local/redis/bin/redis-server /etc/redis.conf
       - 检查数据

##### windows连接redis

```shell
redis-cli.exe -h 127.0.0.1 -p 6379
```





