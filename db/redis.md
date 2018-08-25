[TOC]

﻿### 基本命令

#### 本地连接

启动redis

`redis-server.exe`

连接

`redis-cli` 后面加--raw可以避免中文乱码

#### 远程连接

```
redis-cli -h host -p port -a password
```

#### 键命令

| 序号 | 命令及描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | [DEL key](http://www.runoob.com/redis/keys-del.html)该命令用于在 key 存在时删除 key。 |
| 2    | [DUMP key](http://www.runoob.com/redis/keys-dump.html)  如果 key 不存在，那么返回 nil 。 否则，返回序列化之后的值。 |
| 3    | [EXISTS key](http://www.runoob.com/redis/keys-exists.html) 检查给定 key 是否存在。 |
| 4    | [EXPIRE key seconds](http://www.runoob.com/redis/keys-expire.html) seconds 为给定 key 设置过期时间。(秒) |
| 5    | [EXPIREAT key timestamp](http://www.runoob.com/redis/keys-expireat.html) EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| 6    | [PEXPIRE key milliseconds](http://www.runoob.com/redis/keys-pexpire.html) 设置 key 的过期时间以毫秒计。 |
| 7    | [PEXPIREAT key milliseconds-timestamp](http://www.runoob.com/redis/keys-pexpireat.html) 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计 |
| 8    | [KEYS pattern](http://www.runoob.com/redis/keys-keys.html) 查找所有符合给定模式( pattern)的 key 。 |
| 9    | [MOVE key db](http://www.runoob.com/redis/keys-move.html) 将当前数据库的 key 移动到给定的数据库 db 当中。 |
| 10   | [PERSIST key](http://www.runoob.com/redis/keys-persist.html) 移除 key 的过期时间，key 将持久保持。 |
| 11   | [PTTL key](http://www.runoob.com/redis/keys-pttl.html) 以毫秒为单位返回 key 的剩余的过期时间。 |
| 12   | [TTL key](http://www.runoob.com/redis/keys-ttl.html) 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。 |
| 13   | [RANDOMKEY](http://www.runoob.com/redis/keys-randomkey.html) 从当前数据库中随机返回一个 key 。 |
| 14   | [RENAME key newkey](http://www.runoob.com/redis/keys-rename.html) 修改 key 的名称 |
| 15   | [RENAMENX key newkey](http://www.runoob.com/redis/keys-renamenx.html) 仅当 newkey 不存在时，将 key 改名为 newkey 。 |
| 16   | [TYPE key](http://www.runoob.com/redis/keys-type.html) 返回 key 所储存的值的类型。 |

## 数据类型

### String

#### set get

##### 赋值取值

**set** **get**

```sql
SET key value
GET key
```

##### 返回旧值

**Getset** key  value

设置一个值,并返回旧值

```sql
getset key a	//返回nil
getset key b	//返回"a"
```

##### 设定指定位数的bit

**SETBIT key offset value**

对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。

##### 获取指定位数的bit

**Getbit** key offset value

返回:字符串值指定偏移量上的位(bit)。(二进制)

参考https://www.zhihu.com/question/27672245

##### 设置多个key value

**mset** key1 [key2]

同时设定一个或多个key value

`mset key1 'value1' key2 'value2'`

##### 获取多个key value

**mget** key [key2]

获取所有(一个或多个)给定 key 的值。

##### 当key不存在设置值

**setnx** key value

只有在 key 不存在时设置 key 的值。

##### 设置多个key value (key不存在)

**msetnx** key value [key value]

同时设定多个key value 所有给定的key必须不存在

#### 有效期

##### 给值和有效期(秒)

**setex** key seconds value

为指定的 key 设置值及其过期时间。(如果key已存在会被替换)

```sql
SETEX key 秒数 值
```

##### 给值和有效期(毫秒)

**psetex** key milliseconds value 

#### 字符串操作

##### 截取字符串

**GETRANGE key start end**

```sql
SET mykey "This is my test key"
GETRANGE mykey 0 3	//"This"
```

##### 覆盖字符串到指定位置

**SETRANGE** key offset value

覆盖给定key所储存的字符串值,覆盖位置从offset开始

```sql
SET key1 "Hello World"
SETRANGE key1 6 "Redis"	//"Hello Redis"
```

##### 拼接字符串到末尾

**append** key value

```sql
append key 'hello'
append key 'redis'
get key //'hello redis'
```

##### 字符串长度

**strlen** key

返回字符串长度

#### 加减

##### 自增

**lncr** key

key不存在默认为0

```sql
set key 1
lncr key	//2
```

##### 增加给定量(整型)

**incrby** key

key不存在默认为0

```sql
set key 50
incrby key 20
get key 	//70
```

##### 增加给定量(浮点数)

**incrbyfloat**(同上)

##### 自减

**decr**(同上自增)

##### 减去给定量(整型)

**decrby**(同上)



### 哈希

#### get set

##### 获取指定字段的值

**hget** key field

##### 获取key中所有字段和值

**hgetall** key

##### 获取key中的所有字段

**hkeys**  key 

##### 获取key中所有的值

**hvals** key

##### 获取key中字段数量

**hlen** key

##### 获取给定字段的值

**hmget** key fleld1 field2

##### 设置字段的值

**hset** key field value

##### 为key同时设置多个字段和值

**hmset** key field1 value1 field2 value2

##### 设置字段的值(字段须不存在)

**hsetnx** key field value



 #### 其他

##### 删除字段

**hdel** key field

```sql
hdel key 字段1 字段2
```

##### 字段是否存在

**hexists** key field

##### 迭代哈希表中的键值对

HSCAN key cursor [MATCH pattern] .[COUNT count]   (还没看懂)



#### 加减

##### 加(整型)

**hincrby** key field 5(增量)

##### 加(浮点型)

hincrbyfloat key field 增量(可以为复数,相当于减法)

### 列表(list)

#### get

##### 移出获取第一个元素

**Lpop** key

移除并获取第一个元素,如果为空,则返回nil

##### 移出获取第一个元素(阻塞)

**blpop** key timeout

如果没有元素,会在timeout (秒) 之后返回nil , 设置为0秒则一直阻塞

##### 移出获取最后一个元素

**Rpop** key

Rpop 命令用于移除并返回列表的最后一个元素 , 当列表不存在时返回nil

##### 移出获取最后一个元素(阻塞)

**brpop** key timeout

移除并返回最后一个元素,如果没有,同上

##### 获取元素(根据索引)

**Lindex** key index

##### 获取元素(根据指定区间)

**Lrange** key strat stop

-1表示最后一个元素, -2表示最后第二个元素, 0表示第一个元素

##### 获取列表长度

**LLen** key



#### add

##### 插入元素到头部 (列表须存在)

**LpushX** key value

如果列表不存在,则操作无效

##### 插入元素到尾部 (列表须存在)

**RpushX** key value

将一个值插入到已存在的列表尾部。如果列表不存在，操作无效。

##### 插入多个元素到头部

**Lpush** key value1 value2

##### 插入个多元素到尾部

**Rpush** key value1 value2

##### 插入元素(根据已有元素)

**Linsert** key before|after  '已有元素'  '插入元素'

```sql
Rpush key 'hello'
Rpush key 'world'
Linsert key before 'world' 'my'
//'hello' 'my' 'world'
```

##### 设置元素的值(根据索引)

**Lset** key index value



#### del

##### 弹出一个元素插入另一个list (阻塞)

**brpoplpush** key(读取)  key2(写入)  timeout(阻塞多少秒)

返回值:一个含两个元素的列表 , 第一个元素为 "弹出元素的值"(如果没有则为nil) , 第二个元素为阻塞的时间

##### 移除元素 (指定个数)

**Lrem** key 个数 value

移除指定个数的value , 个数为0时, 移除所有与参数value相等的元素

个数为正数时 , 从表头向表尾检索 

个数为负数时 , 从表尾向表头检索

##### 截取指定区间的元素

**Ltrim** key strat stop	(返回值为该区间的元素)

不在指定区间内的元素将会被删除

##### 移出最后一个元素到另一个列表

**Rpoplpush**  key otherkey	(返回值为最后一个元素的值)



### 无序集合(Set)

Set是string类型的无序集合 (不允许重复)

#### get

##### 获取集合长度

**Scard** key

##### 获取给定集合的差集

**Sdiff** key otherkey1 otherkey2

```js
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SDIFF key1 key2 key3 = {b,d}
```

##### 获取给定集合的差集并存储

**SdiffStore** dbkey key ohterkey1 otherkey2

##### 获取给定集的交集

**Sinter**  key  otherkey

##### 获取给定集合的交集并存储

**SinterStore** dbkey key ohterkey1 otherkey2

##### 获取给定集的并集

**Sunion** key otherkey1 otherkey2

##### 获取给定集合的并集并存储

**SunionStore** dbkey key ohterkey1 otherkey2

##### 判断该元素是否存在

**SisMember** key value

##### 获取所有元素

**Smembers** key

##### 获取一个或多个随机元素

**SrandMember** key [count]

##### 获取匹配通配符的元素

**Sscan** key 光标 match 通配符



#### add

##### 添加一/多个成员

**Sadd** key value1 value2



#### move

##### 元素移动到另一集合

**Smove** key(原始集合)  key2(另一集合)  value

##### 移除并返回随机元素

**Spop** key

##### 移除一个或多个成员

**Srem** key member1 [member2]



### 有序集合(zset)

zset是string类型的有序集合 (不允许重复)

每个元素都要关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。(分数可以重复)

#### get

##### 获取元素个数

**Zcard** key

##### 获取指定分数区间的元素个数

**Zcount** key min max

##### 获取交集并存储在新的集合中

**ZinterStore** dbkey numkeys(有几个key就写几)  key1  key2.. 

新的集合中元素的分数值是给定集下该元素分数值之和

##### 获取并集并存储在新的集合中

**ZunionStore**  dbkey numkeys(有几个key就写几)  key1  key2.. 

##### 获取指定区间元素数量(根据字典)

**ZlexCount** key min max

```sql
ZADD myzset 0 a 0 b 0 c 0 d 0 e
ZADD myzset 0 f 0 g
ZLEXCOUNT myzset - +	//结果为7
ZLEXCOUNT myzset [b [f	//结果为5
```

##### 获取指定区间元素(根据字典)

**ZrangeByLex** key min max  [限制偏移个数]

##### 获取指定区间元素(根据分数)

**ZrangeByScore**  key  min  max  [withScore].[Limit]	

//第一个可选参数为挑选带有分数的元素(并返回所带分数) , 第二个参数限制偏移个数

```sql
ZRANGEBYSCORE key (5 10	//符合条件 5 < score <= 10 的成员
//默认闭区间 , 可选开区间 '('
```

##### 获取指定区间元素 (根据索引)

**Zrange** key strat stop [withscores]	//带上可选参数,则只返回有分数的元素

下标参数 start 和 stop 以 0 表示有序集第一个成员，以 -1 表示有序集最后一个成员

##### 获取指定区间元素(根据索引)

根据分数排序

**ZRange** key strat stop [withScore]		//递增排列

**ZrevRange** key strat stop [withScore]	//递减排列

##### 获取元素索引

**Zrank** key member

##### 获取元素分数值

**Zscore** key member

##### 获取指定区间元素排名

**ZrevRank** key member	//递减

**ZRank** key member		//递增

##### 获取匹配通配符的元素(迭代)

 ZSCAN key cursor [MATCH pattern] . [COUNT count]



#### add

##### 添加一个或多个元素,或者更新元素分数

**Zadd** key 分数 value

##### 给指定元素增加分数

**Zincrby** key 增加几分 value



#### remove

##### 移除一个或多个元素

**Zrem** key member [member]

##### 移除给定区间的元素(根据字典)

**ZremRangeByLex**  key  min  max

##### 移除给定区间的元素(根据下标)

**ZremRangeByRank** key strat stop

##### 移除给定区间的元素(根据分数)

**ZremRangeByScore** key min max [withScore]



### 应用场景

各个数据类型应用场景：

| 类型                 | 简介                                                   | 特性                                                         | 场景                                                         |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| String(字符串)       | 二进制安全                                             | 可以包含任何数据,比如jpg图片或者序列化的对象,一个键最大能存储512M | ---                                                          |
| Hash(字典)           | 键值对集合,即编程语言中的Map类型                       | 适合存储对象,并且可以像数据库中update一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去) | 存储、读取、修改用户属性                                     |
| List(列表)           | 链表(双向链表)                                         | 增删快,提供了操作某一段元素的API                             | 1,最新消息排行等功能(比如朋友圈的时间线) 2,消息队列          |
| Set(集合)            | 哈希表实现,元素不重复                                  | 1,添加、删除,查找的复杂度都是O(1) 2,为集合提供了求交集、并集、差集等操作 | 1,共同好友 2,利用唯一性,统计访问网站的所有独立ip 3,好用推荐时,根据tag求交集,大于某个阈值就可以推荐 |
| Sorted Set(有序集合) | 将Set中的元素增加一个权重参数score,元素按score有序排列 | 数据插入集合时,已经进行天然排序                              | 1,排行榜 2,带权重的消息队列                                  |

[TOC]

