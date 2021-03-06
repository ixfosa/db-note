## Redis简介

+ Redis是当前互联网世界最为流行的 NoSQL（Not Only SQL）数据库。

+ Redis 具备一定持久层的功能，也可以作为一种缓存工具。



> 对于 NoSQL 数据库而言，作为持久层，它存储的数据是半结构化的，这就意味着计算机在读入内存中有更少的规则，读入速度更快。
>
> NoSQL 的数据主要存储在内存中（部分可以持久化到磁盘），而数据库主要是磁盘。
>
> NoSQL 并不完全安全稳定，由于它基于内存，一旦停电或者机器故障数据就很容易丢失数据，其持久化能力也是有限的，而基于磁盘的数据库则不会出现这样的问题。其数据完整性、事务能力、安全性、可靠性及可扩展性都远不及数据库。



## 优点

### 响应快速

Redis 响应非常快，每秒可以执行大约 110 000 个写入操作，或者 81 000 个读操作，其速度远超数据库。如果存入一些常用的数据，就能有效提高系统的性能。

### 支持 6 种数据类型

它们是`字符串`、`哈希结构`、`列表`、`集合`、`可排序集合`和`基数`。比如对于字符串可以存入一些 Java基础数据类型，哈希可以存储对象，列表可以存储 List 对象等。这使得在应用中很容易根据自己的需要选择存储的数据类型，方便开发。

对于 Redis 而言，虽然只有 6 种数据类型，但是有两大好处：一方面可以满足存储各种[数据结构体的需要；另外一方面数据类型少，使得规则就少，需要的判断和逻辑就少，这样读/写的速度就更快。

### 操作都是原子的

所有 Redis 的操作都是`原子`的，从而确保当两个客户同时访问 Redis 服务器时，得到的是更新后的值（最新值）。在需要高并发的场合可以考虑使用 Redis 的事务，处理一些需要锁的业务。

### MultiUtility 工具

Redis 可以在如缓存、消息传递队列中使用（Redis 支持 `发布+订阅` 的消息模式），在应用程序如 Web 应用程序会话、网站页面点击数等任何短暂的数据中使用。

一方面，使用 NoSQL 从数据库中读取数据进行缓存，就可以从内存中读取数据了，而不像数据库一样读磁盘。现实是读操作远比写操作要多得多，所以缓存很多常用的数据，提高其命中率有助于整体性能的提高，并且能减缓数据库的压力，对互联网系统架构是十分有利的。

另一方面，它也能满足互联网高并发需要高速处理数据的场合，比如抢红包、商品秒杀等场景，这些场合需要高速处理，并保证并发数据安全和一致性。



## 基本安装和使用

 https://github.com/MSOpenTech/redis/releases

在Redis根目录下新建一个文件 `startup.cmd`，用记事本或者其他文本编辑工具打开（要是打不开，可以先写入内容在重命名），然后写入以下内容。

```
redis-server redis.windows.conf
```

+  `redis-server.exe` 的命令读取` redis.window.conf` 的内容，用来启动 redis，保存好了 startup.cmd 文件，双击它就可以看到 Redis 启动的信息了

+ ` redis-cli.exe`，它是一个 Redis 自带的客户端工具，这样就可以连接到 Redis 服务器了



## Redis的6种数据类型

Redis是一种基于内存的数据库，并且提供一定的持久化功能，它是一种`键值`（key-value）数据库，使用 key 作为索引找到当前缓存的数据，并且返回给程序调用者。

当前的 Redis 支持 6 种数据类型，它们分别是

+ `字符串`（String）
+ `列表`（List）
+ `集合`（set）
+ `哈希结构`（hash）
+ `有序集合`（zset）
+ `基数`（HyperLogLog）。

| 数据类型            | 数据类型存储的值                                             | 说 明                                                        |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| string（字符串）    | 可以是保存字符串、整数和浮点数                               | 可以对字符串进行操作，比如增加字符或者求子串：如果是整数或者浮点数，可以实现计算，比如自增等 |
| list（列表）        | 它是一个链表，它的每一个节点都包含一个字符串                 | Redis 支持从链表的两端插入或者弹出节点，或者通过偏移对它进行裁剪；还可以读取一个或者多个节点，根据条件删除或者查找节点等 |
| set（集合）         | 它是一个收集器，但是是无序的，在它里而每一个元素都是一个字符串，而且是独一无二，各不相同的 | 可以新增、读取、删除单个元素：检测一个元素是否在集合中；计算它和其他集合的交集、并集和差集等；随机从集合中读取元素 |
| hash（哈希散列表）  | 它类似于 Java 语言中的 Map，是一个键值对应的无序列表         | 可以増、删、査、改单个键值对，也可以获取所有的键值对         |
| zset（有序集合）    | 它是一个有序的集合，可以包含字符 串、整数、浮点数、分值（score），元素 的排序是依据分值的大小来决定的 | 可以增、删、査、改元素，根据分值的范围或者成员 來获取对应的元索 |
| HyperLogLog（基数） | 它的作用是计算重复的值，以确定存储的数量                     | 只提供基数的运算，不提供返回的功能                           |



## 字符串

字符串是 Redis 最基本的数据结构，它将以一个键和一个值存储于 Redis 内部，它犹如 Java)的 Map 结构，让 Redis 通过键去找到值。Redis 字符串的数据结构如图 1 所示。

![Redis 字符串数据结构](../_media/Redis字符串数据结构.png)



| 命 令                  | 说 明                                             | 备 注                                                        |
| ---------------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| set key value          | 设置键值对                                        | 最常用的写入命令                                             |
| get key                | 通过键获取值                                      | 最常用的读取命令                                             |
| del key                | 通过 key，删除键值对                              | 删除命令，返冋删除数，注意，它是个通用的命令，换句话说在其他数据结构中，也可以使用它 |
| strlen key             | 求 key 指向字符串的长度                           | 返回长度                                                     |
| getset key value       | 修改原来 key 的对应值，并将旧值返回               | 如果原来值为空，则返回为空，并设置新值                       |
| getrange key start end | 获取子串                                          | 记字符串的长度为 len，把字符串看作一个数组，而 Redis 是以 0 开始计数的，所以 start 和 end 的取值范围 为 0 到 len-1 |
| append key value       | 将新的字符串 value，加入到原来 key 指向的字符串末 | 返回 key 指向新字符串的长度                                  |

如果字符串是数字（整数或者浮点数），那么 Redis 还能支持简单的运算。不过它的运算能力比较弱

| 命  令                   | 说  明                            | 备  注                 |
| ------------------------ | --------------------------------- | ---------------------- |
| incr key                 | 在原字段上加 1                    | 只能对整数操作         |
| incrby key increment     | 在原字段上加上整数（increment）   | 只能对整数操作         |
| decr key                 | 在原字段上减 1                    | 只能对整数操作         |
| decrby key decrement     | 在原字段上减去整数（decrement）   | 只能对整数操作         |
| incrbyfloat keyincrement | 在原字段上加上浮点数（increment） | 可以操作浮点数或者整数 |



## 哈希

Redis 中哈希结构就如同 Java 的 map 一样，一个对象里面有许多键值对，它是特别适合存储对象的，如果内存足够大，那么一个 Redis 的 hash 结构可以存储 2 的 32 次方减 1 个键值对（40 多亿）。

一般而言，不会使用到那么大的一个键值对，所以我们认为 Redis 可以存储很多的键值对。在 Redis 中，hash 是一个 String 类型的 field 和 value 的映射表，因此我们存储的数据实际在 Redis 内存中都是一个个字符串而已。

假设角色有 3 个字段：编号（id）、角色名称（ame）和备注（age），这样就可以使用一个 hash 结构保存它，它的内存结果如表 1 所示。

| role  |        |
| ----- | ------ |
| field | value  |
| id    | 21     |
| name  | ixfosa |
| age   | 22     |



Redis hash结构命令

| 命  令                                        | 说  明                                     | 备  注                   |
| --------------------------------------------- | ------------------------------------------ | ------------------------ |
| hdel key field1[field2......]                 | 删除 hash 结构中的某个（些）字段           | 可以进行多个字段的删除   |
| hexists key field                             | 判断 hash 结构中是否存在 field 字段        | 存在返回 1，否则返回 0   |
| hgetall key                                   | 获取所有 hash 结构中的键值                 | 返回键和值               |
| hincrby key field increment                   | 指定给 hash 结构中的某一字段加上一个整数   | 要求该字段也是整数字符串 |
| hincrbyfloat key field increment              | 指定给 hash 结构中的某一字段加上一个浮点数 | 要求该字段是数字型字符串 |
| hkeys key                                     | 返回 hash 中所有的键                       | ——                       |
| hlen key                                      | 返回 hash 中键值对的数量                   | ——                       |
| hmget key field1[field2......]                | 返回 hash 中指定的键的值，可以是多个       | 依次返回值               |
| hmset key field1 value1 [field2 field2......] | hash 结构设置多个键值对                    | ——                       |
| hset key filed value                          | 在 hash 结构中设置键值对                   | 单个设值                 |
| hsetnx key field value                        | 当 hash 结构中不存在对应的键，才设置值     | ——                       |
| hvals key                                     | 获取 hash 结构中所有的值                   | ——                       |

在 Redis 中的哈希结构和字符串有着比较明显的不同。

首先，命令都是以` h` 开头，代表操作的是 hash 结构。其次，大多数命令多了一个层级 field，这是 hash 结构的一个内部键，也就是说 Redis 需要通过 key 索引到对应的 hash 结构，再通过 field 来确定使用 hash 结构的哪个键值对。



对于数字的操作命令 hincrby 而言，要求存储的也是整数型的字符串，对于 hincrbyfloat 而言，则要求使用浮点数或者整数，否则命令会失败。



## 链表

链表结构是 Redis中一个常用的结构，它可以存储多个字符串，而且它是有序的，能够存储 2 的 32 次方减 1 个节点（超过 40 亿个节点）。

Redis 链表是双向的，因此即可以从左到右，也可以从右到左遍历它存储的节点，链表结构如图所示。

![链表结构](../_media/链表结构.jpg)



由于是双向链表，所以只能够从左到右，或者从右到左地访问和操作链表里面的数据节点。但是使用链表结构就意味着读性能的丧失，所以要在大量数据中找到一个节点的操作性能是不佳的，因为链表只能从一个方向中去遍历所要节点。

比如从查找节点 10 000 开始查询，它需要按照节点 1、节点 2、节点 3……直至节点 10 000，这样的顺序查找，然后把一个个节点和你给出的值比对，才能确定节点所在。如果这个链表很大，如有上百万个节点，可能需要遍历几十万次才能找到所需要的节点，显然查找性能是不佳的。

而链表结构的优势在于插入和删除的便利，因为链表的数据节点是分配在不同的内存区域的，并不连续，只是根据上一个节点保存下一个节点的顺序来索引而已，无需移动元素。其新增和删除的操作如图 所示。   阿拉伯数字代表新增的步骤，而汉字数字代表删除步骤。

![链表的新增和删除操作](images\链表的新增和删除操作.png)

1. 新增节点

   对插入图中的节点 4 而言，先看从左到右的指向，先让节点 4 指向节点 1 原来的下一个节点，也就是节点 2，然后让节点 1 指向节点 4，这样就完成了从右到左的指向修改。再看从右到左，先让节点 4 指向节点 1，然后节点 2 指向节点 4，这个时候就完成了从右到左的指向，那么节点 1 和节点 2 之间的原有关联关系都已经失效，这样就完成了在链表中新增节点4的功能。

2. 删除节点

   对删除图中的节点 3 而言，首先让节点 2 从左到右指向后续节点，然后让后续节点指向节点 2，这样节点 3 就脱离了链表，也就是断绝了与节点 2 和后继节点的关联关系，然后对节点 3 进行内存回收，无须移动任何节点，就完成了删除。

由此可见，链表结构的使用是需要注意场景的，对于那些经常需要对数据进行插入和删除的列表数据使用它是十分方便的，因为它可以在不移动其他节点的情况下完成插入和删除。而对于需要经常查找的，使用它性能并不佳，它只能从左到右或者从右到左的查找和比对。

因为是`双向链表结构`，所以 Redis 链表命令分为`左操作`和`右操作`两种命令，左操作就意味着是从左到右，右操作就意味着是从右到左。Redis 关于链表的命令如表 1 所示。

| 命  令                               | 说  明                                                       | 备  注                                                       |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| lpush key node1 [node2.].....        | 把节点 node1 加入到链表最左边                                | 如果是 node1、node2 ...noden 这样加入， 那么链表开头从左到右的顺序是 noden...node2、node1 |
| rpush key node1[node2]......         | 把节点 node1 加入到链表的最右边                              | 如果是 node1、node2....noden 这样加 入，那么链表结尾从左到右的顺序是 node1、node2,node3...noden |
| lindex key index                     | 读取下标为 index 的节点                                      | 返回节点字符串，从 0 开始算                                  |
| llen key                             | 求链表的长度                                                 | 返回链表节点数                                               |
| lpop key                             | 删除左边第一个节点，并将其返回                               | ——                                                           |
| rpop key                             | 删除右边第一个节点，并将其返回                               | ——                                                           |
| linsert key before\|after pivot node | 插入一个节点 node，并且可以指定在值为pivot 的节点的前面（before）或者后面（after)） | 如果 list 不存在，则报错；如果没有值为对应 pivot 的，也会插入失败返回 -1 |
| lpushx list node                     | 如果存在 key 为 list 的链表，则插入节点 node, 并且作为从左到右的第一个节点 | 如果 list 不存在，则失败                                     |
| rpushx list node                     | 如果存在 key 为 list 的链表，则插入节点 node，并且作为从左到右的最后个节点 | 如果 list 不存在，则失败                                     |
| lrange list start end                | 获取链表 list 从 start 下标到 end 下标的节点值               | 包含 start 和 end 下标的值                                   |
| lrem list count value                | 如果 count 为 0，则删除所有值等于 value 的节 点：如果 count 不是 0，则先对 count 取绝对值，假设记为 abs，然后从左到右删除不大于 abs 个等于 value 的节点 | 注意，count 为整数，如果是负数，则 Redis 会先求取其绝对值，然后传递到后台操作 |
| lset key index node                  | 设置列表下标为 index 的节点的值为 node                       | ——                                                           |
| ltrim key start stop                 | 修剪链表，只保留从 start 到 stop 的区间的节点，其余的都删除掉 | 包含 start 和 end 的下标的节点会保留                         |

其中以“`l`”开头的代表左操作，以“`r`”开头的代表右操作。

对于很多个节点同时操作的，需要考虑其花费的时间，链表数据结构对于查找而言并不适合于大数据

只是对于大量数据操作的时候，我们需要考虑插入和删除内容的大小，因为这将是十分消耗性能的命令，会导致 Redis 服务器的卡顿。对于不允许卡顿的一些服务器，可以进行分批次操作，以避免出现卡顿。

这些操作链表的命令都是`进程不安全的`，因为当我们操作这些命令的时候，其他 Redis 的客户端也可能操作同一个链表，这样就会造成并发数据安全和一致性的问题，尤其是当你操作一个数据量不小的链表结构时，常常会遇到这样的问题。

为了克服这些问题，Redis 提供了链表的阻塞命令，它们在运行的时候，会给链表加锁，以保证操作链表的命令安全性

| 命  令                          | 说  明                                                       | 备  注                                 |
| ------------------------------- | ------------------------------------------------------------ | -------------------------------------- |
| blpop key timeout               | 移出并获取列表的第一个元索，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元索为止 | 相对于 lpop 命令，它的操作是进程安全的 |
| brpop key timeout               | 移出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止 | 相对于 rpop 命令，它的操作是进程安全的 |
| rpoplpush key sre dest          | 按从左到右的顺序，将一个链表的最后一个元素移除，并插入到目标链表最左边 | 不能设置超时时间                       |
| brpoplpush key src dest timeout | 按从左到右的顺序，将一个链表的最后一个元素移除，并插入到目标链表最左边，并可以设置超时时间 | 可设置超时时间                         |

当使用这些命令时，Redis 就会对对应的链表加锁，加锁的结果就是其他的进程不能再读取或者写入该链表，只能等待命令结束。加锁的好处可以保证在多线程并发环境中数据的一致性，保证一些重要数据的一致性，比如账户的金额、商品的数量。

在实际的项目中，虽然阻塞可以有效保证了数据的一致性，但是阻塞就意味着其他进程的等待，CPU 需要给其他线程挂起、恢复等操作，更多的时候我们希望的并不是阻塞的处理请求，所以这些命令在实际中使用得并不多



## 集合

Redis的集合不是一个线性结构，而是一个哈希表结构，它的内部会根据 hash 分子来存储和查找数据，理论上一个集合可以存储 2 的 32 次方减 1 个节点（大约 42 亿）个元素，因为采用哈希表结构，所以对于 Redis 集合的插入、删除和查找的复杂度都是 0（1），只是我们需要注意 3 点。

- 对于集合而言，它的每一个元素都是`不能重复`的，当插入相同记录的时候都会失败。
- 集合是`无序`的。
- 集合的每一个元素都是 `String` 数据结构类型。


Redis 的集合可以对于不同的集合进行操作，比如求出两个或者以上集合的交集、差集和并集等。

| 命  令                                   | 说  明                                                       | 备  注                                                       |
| ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| sadd key member1 [member2 member3......] | 给键为 key 的集合増加成员                                    | 可以同时増加多个                                             |
| scard key                                | 统计键为 key 的集合成员数                                    | —                                                            |
| sdiffkey1 [key2]                         | 找出两个集合的差集                                           | 参数如果是单key，那么 Redis 就返回这个 key 的所有元素        |
| sdiftstore des key1 [key2]               | 先按 sdiff 命令的规则，找出 key1 和 key2 两 个集合的差集，然后将其保存到 des 集合中。 | —                                                            |
| sinter key1 [key2]                       | 求 key1 和 key2 两个集合的交集。                             | 参数如果是单 key，那么 Redis 就返冋这个 key 的所有元素       |
| sinterstore des key1 key2                | 先按 sinter 命令的规则，找出 key1 和 key2 两个集合的交集，然后保存到 des 中 | —                                                            |
| sismember key member                     | 判断 member 是否键为 key 的集合的成员                        | 如果是返回 1，否则返回 0                                     |
| smembers key                             | 返回集合所有成员                                             | 如果数据量大，需要考虑迭代遍历的问题                         |
| smove src des member                     | 将成员 member 从集合 src 迁移到集合 des 中                   | —                                                            |
| spop key                                 | 随机弹出集合的一个元素                                       | 注意其随机性，因为集合是无序的                               |
| srandmember key [count]                  | 随机返回集合中一个或者多个元素，count 为限制返回总数，如果 count 为负数，则先求其绝对值 | count 为整数，如果不填默认为 1，如果 count 大于等于集合总数，则返回整个集合 |
| srem key member1[member2......]          | 移除集合中的元素，可以是多个元素                             | 对于很大的集合可以通过它删除部分元素，避免删除大量数据引发 Redis 停顿 |
| sunion key1 [key2]                       | 求两个集合的并集                                             | 参数如果是单 key，那么 Redis 就返回这个 key 的所有元素       |
| sunionstore des key1 key2                | 先执行 sunion 命令求出并集，然后保存到键为 des 的集合中      | —                                                            |

集合命令的前缀都包含了一个 s，用来表达这是集合的命令，集合是无序的，并且支持并集、交集和差集的运算

## 有序集合

有序集合和集合类似，只是说它是有序的，和无序集合的主要区别在于每一个元素除了值之外，它还会多一个分数。分数是一个浮点数，在 Java中是使用双精度表示的，根据分数，Redis就可以支持对分数从小到大或者从大到小的排序。

这里和无序集合一样，对于每一个元素都是`唯一`的，但是对于不同元素而言，它的分数可以一样。元素也是 String 数据类型，也是一种基于 hash 的存储结构。

集合是通过哈希表实现的，所以添加、删除、查找的复杂度都是 0（1）。集合中最大的成员数为 2 的 32 次方减 1（40 多亿个成员），有序集合的数据结构如图 所示。

![有序集合的数据结构](images\有序集合的数据结构.png)

有序集合是依赖 key 标示它是属于哪个集合，依赖分数进行排序，所以值和分数是必须的，而实际上不仅可以对分数进行排序，在满足一定的条件下，也可以对值进行排序。



有序集合和无序集合的命令是接近的，只是在这些命令的基础上，会增加对于排序的操作，这些是我们在使用的时候需要注意的细节。

有时 Redis 借助数据区间的表示方法来表示包含或者不包含，比如在数学的区间表示中，[2,5] 表示包含 2，但是不包含 5 的区间。

| 命  令                                                      | 说  明                                                       | 备  注                                                       |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| zadd key score1 value1 [score2 value2......]                | 向有序集合的 key，增加一个或者多个成员                       | 如果不存在对应的 key，则创建键为 key 的有序集合              |
| zcard key                                                   | 获取有序集合的成员数                                         | —                                                            |
| zcount key min max                                          | 根据分数返回对应的成员列表                                   | min 为最小值，max 为最大值，默认为包含 min 和 max 值，采用数学区间表示的方法，如果需要不包含，则在分数前面加入“(”，注意不支持“[”表示 |
| zincrby key increment member                                | 给有序集合成员值为 member 的分数增加 increment               | —                                                            |
| zinterstore desKey numkeys key1 [key2 key3......]           | 求多个有序集合的交集，并且将结果保存到 desKey 中             | numkeys 是一个整数，表示多少个有序集合                       |
| zlexcount key min max                                       | 求有序集合 key 成员值在 min 和 max 的范围                    | 这里范围为 key 的成员值，Redis 借助数据区间的表示方法，“[”表示包含该值，“(”表示不包含该值 |
| zrange key start stop [withscores]                          | 按照分值的大小（从小到大）返回成员，加入 start 和 stop 参数可以截取某一段返回。如果输入可选项 withscores，则连同分数一起返回 | 这里记集合最人长度为 len，则 Redis 会将集合排序后，形成一个从 0 到 len-1 的下标，然后根据 start 和 stop 控制的下标（包含 start 和 stop）返回 |
| zrank key member                                            | 按从小到大求有序集合的排行                                   | 排名第一的为 0，第二的为 1……                                 |
| zrangebylex key min max [limit offset count]                | 根据值的大小，从小到大排序，min 为最小值，max 为最大值；limit 选项可选，当 Redis 求出范围集合后，会生产下标 0 到 n，然后根据偏移量 offset 和限定返回数 count，返回对应的成员 | 这里范围为 key 的成员值，Redis 借助数学区间的表示方法，“[”表示包含该值，“(”表示不包含该值 |
| zrangebyscore key min max [withscores] [limit offset count] | 根据分数大小，从小到大求取范围，选项 withscores 和 limit 请参考 zrange 命令和 zrangebylex 说明 | 根据分析求取集合的范围。这里默认包含 min 和 max，如果不想包含，则在参数前加入“(”， 注意不支持“[”表示 |
| zremrangebyscore key start stop                             | 根据分数区间进行删除                                         | 按照 socre 进行排序，然后排除 0 到 len-1 的下标，然后根据 start 和 stop 进行删除，Redis 借助数学区间的表示方法，“[”表示包含该值，“(” 表示不包含该值 |
| zremrangebyrank key start stop                              | 按照分数排行从小到大的排序删除，从 0 开始计算                | —                                                            |
| zremrangebylex key min max                                  | 按照值的分布进行删除                                         | —                                                            |
| zrevrange key start stop [withscores]                       | 从大到小的按分数排序，参数请参见 zrange                      | 与 zrange 相同，只是排序是从大到小                           |
| zrevrangebyscore key max min [withscores]                   | 从大到小的按分数排序，参数请参见 zrangebyscore               | 与 zrangebyscore 相同，只是排序是从大到小                    |
| zrevrank key member                                         | 按从大到小的顺序，求元素的排行                               | 排名第一位 0，第二位 1......                                 |
| zscore key member                                           | 返回成员的分数值                                             | 返回成员的分数                                               |
| zunionstore desKey numKeys key1 [key2 key3 key4......]      | 求多个有序集合的并集，其中 numKeys 是有序集合的个数          | ——                                                           |

在对有序集合、下标、区间的表示方法进行操作的时候，需要十分小心命令，注意它是操作分数还是值，稍有不慎就会出现问题。



##  HyperLogLog

基数是一种算法。举个例子，一本英文著作由数百万个单词组成，你的内存却不足以存储它们，那么我们先分析一下业务。

英文单词本身是有限的，在这本书的几百万个单词中有许许多多重复单词，扣去重复的单词，这本书中也就是几千到一万多个单词而已，那么内存就足够存储它们了。

比如数字集合 {1,2,5,7,9,1,5,9} 的基数集合为 {1,2,5,7,9} 那么基数（不重复元素）就是 5，基数的作用是评估大约需要准备多少个存储单元去存储数据，但是基数的算法一般会存在一定的误差（一般是可控的）。Redis 对基数数据结构的支持是从版本 2.8.9 开始的。

基数并不是存储元素，存储元素消耗内存空间比较大，而是给某一个有重复元素的数据集合（一般是很大的数据集合）评估需要的空间单元数，所以它没有办法进行存储，

| 命  令                             | 说  明                                       | 备  注                                 |
| ---------------------------------- | -------------------------------------------- | -------------------------------------- |
| pfadd key element                  | 添加指定元素到 HyperLogLog 中                | 如果已经存储元索，则返回为 0，添加失败 |
| pfcount key                        | 返回 HyperLogLog 的基数值                    | —                                      |
| pfmerge desKey key1 [key2 key3...] | 合并多个 HyperLogLog，并将其保存在 desKey 中 | —                                      |

