[TOC]

## Keys

​	Redis key 值是二进制安全的，这意味着可以用任何二进制序列作为 key 值，从形如 ”foo” 的简单字符串到一个 JPEG 文件的内容都可以，空字符串也是有效 key 值。

关于key的几条规则：

- 太长的键值不是个好主意，例如 1024 字节的键值就不是个好主意，不仅因为消耗内存，而且在数据中查找这类键值的计算成本很高。

- 太短的键值通常也不是好主意，如果你要用 "u:1000:pwd" 来代替 "user:1000:password"，这没有什么问题，但后者更易阅读，并且由此增加的空间消耗相对于 key object 和 value object 本身来说很小。当然，没人阻止您一定要用更短的键值节省一丁点儿空间。
- 最好坚持一种模式。例如："object-type:id:field" 就是个不错的注意，像这样 "user:1000:password"。还可以对多单词的字段名中加上一个点，就像这样："comment:1234:reply.to"。

#### 常用操作

| Operation                    | Meaning                                                      |
| ---------------------------- | ------------------------------------------------------------ |
| keys *                       | 查看当前库的所有键                                           |
| exists <key>                 | 判断 key 是否存在                                            |
| del key [key ...]            | 删除指定的 key（一个或多个）                                 |
| expire <key> <seconds>       | 设置 key 的过期时间,单位为 *秒*                              |
| ttl <key>                    | 查看还有多少秒过期，-1 表示永不过期，-2 表示已过期           |
| flushdb                      | 清空当前库                                                   |
| type key                     | 查看 key 的存储类型                                          |
| rename key newkey            | 将一个 key 重命名                                            |
| renamenx key newkey          | 重命名一个 key，新的 key 必须是不存在的 key                  |
| sort key [ASC\|DESC] [ALPHA] | 对列表、集合、有序集合排序。默认按照数值类型排序，当元素是字符串值并且需要按照字典顺序排序时，可以使用 ALPHA，<a href="http://www.redis.cn/commands/sort.html">详细介绍</a> |



## 数据类型

### 字符串（Strings）

​	字符串是一种最基本的 Redis 值类型。Redis 字符串是二进制安全的，这意味着一个 Redis 字符串能包含任意类型的数据，例如： 一张 JPEG 格式的图片或者一个序列化的 Ruby 对象。

​	一个字符串类型的值最多能存储 `512M` 字节的内容。

你可以用 Redis 字符串做许多有趣的事，例如你可以：

- 利用 INCR 命令簇（[INCR](http://www.redis.cn/commands/incr.html), [DECR](http://www.redis.cn/commands/decr.html), [INCRBY](http://www.redis.cn/commands/incrby)）来把字符串当作原子计数器使用。
- 使用 [APPEND](http://www.redis.cn/commands/append.html) 命令在字符串后添加内容。
- 将字符串作为 [GETRANGE](http://www.redis.cn/commands/getrange.html) 和 [SETRANGE](http://www.redis.cn/commands/setrange.html) 的随机访问向量。
- 在小空间里编码大量数据，或者使用 [GETBIT](http://www.redis.cn/commands/getbit.html) 和 [SETBIT](http://www.redis.cn/commands/setbit.html) 创建一个 Redis 支持的 Bloom 过滤器。

#### 常用操作

| Operation                                 | Meaning                                                      |
| ----------------------------------------- | ------------------------------------------------------------ |
| set <key> <value>                         | 设置一个 key 的 value，当 key 不存在时，添加 key             |
| get <key>                                 | 返回 key 的 value                                            |
| append  <key>  <value>                    | 追加一个值到 key                                             |
| strlen <key>                              | 获得 key 的值的长度                                          |
| incr key                                  | 将 key 中储存的数字值加 1，只能对数字值操作，如果为空，新增值为 1 |
| decr key                                  | 将 key 中储存的数字值减 1，只能对数字值操作，如果为空，新增值为 -1 |
| incrby  <key>  <increment>                | 执行原子增加一个整数                                         |
| decrby <key>  <decrement>                 | 原子减指定整数                                               |
| getrange  <key> <start> <end>             | 获取存储在 key 上的值的子字符串，0 到 -1 表示全部            |
| setrange  <key> <offset> <value>          | 用 value 覆写 key 所储存的字符串值，从 offset 开始           |
| setex <key> <seconds> <value>             | 设置 key-value 并设置过期时间（秒），setex：set with expire  |
| setnx <key> <value>                       | 只有在 key 不存在时设置 key 的值，setnx：set if not exist    |
| mset <key> <value> [<key>  <value> ...]   | 设置多个 key-value                                           |
| msetnx <key> <value> [<key>  <value> ...] | 设置多个 key-value，当且仅当所有 key 都不存在时才执行        |
| mget <key> [<key> ...]                    | 获取多个 key 的值                                            |
| getset <key> <value>                      | 先 get 再 set，设置一个 key 的 value，并获取设置前的值       |



### 列表（Lists）

​	Redis 列表是简单的字符串列表，按照插入顺序排序。 你可以添加一个元素到列表的头部（左边）或者尾部（右边）。[LPUSH](http://www.redis.cn/commands/lpush.html) 命令插入一个新元素到列表头部，而 [RPUSH](http://www.redis.cn/commands/rpush.html) 命令插入一个新元素到列表的尾部。当对一个空 key 执行其中某个命令时，将会创建一个新表。 类似的，如果一个操作要清空列表，那么 key 会从对应的 key 空间删除。

​	一个列表最多可以包含 2^32^ - 1 个元素（4294967295，每个表超过 40 亿个元素）。

​	从时间复杂度的角度来看，Redis 列表主要的特性就是支持时间常数的插入和靠近头尾部元素的删除，即使是需要插入上百万的条目。访问列表两端的元素是非常快的，因为那是一个时间复杂度为 **O(N)** 的操作。

#### 常用操作

> *注意* ：Redis 的索引范围是左右都包含的，不是左闭右开。

| Operation                                        | Meaning                                                      |
| ------------------------------------------------ | ------------------------------------------------------------ |
| lpush <key> <value> [<value> ...]                | 从队列的左边（头部）入队一个或多个元素。若 key 不存在，在 push 前会创建一个空列表 |
| lpushx <key> <value>                             | 只有当 key 已经存在并且存着一个 list 的时候，在 list 头部插入 value。若 key 不存在，不会进行任何操作 |
| rpush <key> <value> [<value> ...]                | 从队列的右边（尾部）入队一个或多个元素，若 key 不存在，在 push 前会创建一个空列表 |
| rpushx <key> <value>                             | 将值 value 插入到列表 key 的表尾，当且仅当 key 存在并且是一个列表。 若 key 不存在，不会进行任何操作 |
| lpop <key>                                       | 移除并返回 key 对应 list 的第一个元素，当 key 不存在时返回 nil |
| rpop <key>                                       | 移除并返回 key 对应 list 的最后一个元素，当 key 不存在时返回 nil |
| lrange <key> <start> <stop>                      | 按照索引下标获得元素（从左到右），-1 表示列表的最后一个元素，-2 是倒数第二个 |
| lindex <key> <index>                             | 返回索引对应的值（从左到右），当 index 超过范围的时候返回 nil |
| llen <key>                                       | 获取队列的长度                                               |
| lrem <key> <count> <value>                       | 从存于 key 的列表里移除前 count 次出现的值为 value 的元素。count > 0 时从头往尾移除，count < 0 时从尾往头移除，count = 0 时移除所有值为 value 的元素 |
| ltrim <key> <start> <stop>                       | 修剪(trim)一个已存在的 list，使得 list 只包含指定范围的元素  |
| rpoplpush <source> <destination>                 | 删除列表最后一个元素，并将其追加到另一个列表头部。若 source 不存在，会返回 nil ，并且不会执行任何操作；若 source 和 destination 相同，等同于移除列表最后一个元素并把该元素放在列表头部 |
| lset <key> <index> <value>                       | 设置 index 位置的 list 元素的值为 value，当 index 超出范围时会返回一个 error |
| linsert <key>  before\|after <pivot>  <newvalue> | 把 value 插入存于 key 的列表中在基准值 pivot 的前面或后面    |



### 集合（Sets）

​	Redis 集合是一个无序的字符串合集。你可以以 **O(1)** 的时间复杂度（无论集合中有多少元素时间复杂度都为常量）完成添加、删除以及测试元素是否存在的操作。

​	Redis 集合有着不允许相同成员存在的优秀特性。向集合中多次添加同一元素，在集合中最终只会存在 `1` 个此元素。

​	一个集合最多可以包含 2^32^ - 1 个元素（4294967295，每个集合超过 40 亿个元素）。

#### 常用操作

| Operation                             | Meaning                                                      |
| ------------------------------------- | ------------------------------------------------------------ |
| sadd <key> <member> [<member> ...]    | 添加一个或多个元素到集合里，已存在的元素将被忽略             |
| smembers <key>                        | 获取集合里所有的元素                                         |
| sismember <key> <member>              | 判断 member 是否是集合 key 的成员，是返回 1，不是返回 0      |
| scard <key>                           | 获取集合里面的元素个数                                       |
| srem <key> <member> [<member> ...]    | 从集合删除一个或多个元素                                     |
| srandmember <key> [<count>]           | 不提供 count，随机返回 key 集合中的一个元素。若 count 是整数且小于元素的个数，返回含有 count 个不同的元素的数组；若 count 是整数且大于集合中元素的个数，返回集合的所有元素；若 count 是负数，返回一个包含 count 绝对值个数元素的数组；若 count 的绝对值大于元素的个数，则返回的结果集里会出现一个元素出现多次的情况。仅返回结果，不做任何操作 |
| spop <key> [<count>]                  | 从存储在 key 的集合中移除并返回一个或多个随机元素，不提供 count 是一个 |
| smove <source> <destination> <member> | 将 member 元素从 source 集合移动到 destination 集合          |
| sdiff <key> [<key> ...]               | 返回集合 key 与给定集合的差集                                |
| sinter <key> [<key> ...]              | 返回指定所有的集合的成员的交集                               |
| sunion <key> [<key> ...]              | 返回给定的多个集合的并集                                     |



### 哈希（Hashes）

​	Redis Hashes 是字符串字段和字符串值之间的映射，所以它们是完美的表示对象（eg : 一个有名、姓、年龄等属性的用户）的数据类型。

​	一个 hash 最多可以包含 2^32^ - 1 个 key - value 键值对（超过 40 亿）。

#### 常用操作

| Operation                                         | Meaning                                                      |
| ------------------------------------------------- | ------------------------------------------------------------ |
| hset <key> <field> <value>                        | 设置 key 指定的哈希集中指定字段的值。如果 key 指定的哈希集不存在，会创建一个新的哈希集并与 key 关联；如果字段在哈希集中存在，它将被重写。 |
| hget <key>  <field>                               | 返回 key 指定的哈希集中该字段所关联的值，当字段不存在或者 key 不存在时返回 nil |
| hmset <key> <field> <value> [<field> <value> ...] | 批量设置 key 指定的哈希集中指定字段的值。该命令将重写所有在哈希集中存在的字段；如果 key 指定的哈希集不存在，会创建一个新的哈希集并与 key 关联 |
| hmget <key>  <field> [<field> ...]                | 返回 key 指定的哈希集中指定字段的值                          |
| hgetall <key>                                     | 返回 key 指定的哈希集中所有的字段和值。返回值中，每个字段名的下一个是它的值，所以返回值的长度是哈希集大小的两倍 |
| hdel <key> <field> [<field> ...]                  | 从 key 指定的哈希集中移除指定的字段。在哈希集中不存在的域将被忽略；如果 key 指定的哈希集不存在，它将被认为是一个空的哈希集，该命令将返回 0 |
| hlen <key>                                        | 返回 key 指定的哈希集包含的字段的数量                        |
| hexists key <field>                               | 查看哈希表 key 中，给定域 field 是否存在。存在返回 1，否则返回 0 |
| hkeys <key>                                       | 返回 key 指定的哈希集中所有字段的名字                        |
| hvals <key>                                       | 返回 key 指定的哈希集中所有字段的值                          |
| hincrby <key> <field> <increment>                 | 增加指定的哈希集 key 中指定字段的数值。如果字段不存在，则字段的值在该操作执行前被设置为 0；HINCRBY 支持的值的范围限定在 64 位有符号整数 |
| hincrbyfloat <key> <field> <increment>            | 为指定 key 的 hash 的 field 字段值执行 float 类型的 increment 加 |
| hsetnx <key> <field> <value>                      | 只有在字段 field 不存在时，设置哈希表字段的值                |



### 有序集合（Sorted sets）

​	Redis 有序集合和 Redis 集合类似，是不包含相同字符串的合集。它们的差别是，每个有序集合的成员都关联着一个评分，这个评分用于把有序集合中的成员 *按最低分到最高分排列*。

​	使用有序集合，可以非常快地 **O(log(N))** 完成添加、删除和更新元素的操作。因为元素是在插入时就排好序的，所以很快地通过评分（score）或位次（position）获得一个范围的元素。

#### 常用操作

| Operation                                                    | Meaning                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| zadd  <key> <score> <member> [<score> <member> ...]          | 将一个或多个 member 及其 score 加入到有序集 key 中，score 是一个双精度的浮点型数字字符串 |
| zrange <key> <start> <stop> [WITHSCORES]                     | 返回存储在有序集合 key 中的指定范围的元素。返回的元素按得分从低到高排列；如果得分相同，按字典排序；如果指定了 WITHSCORES 选项，将同时返回它们的得分 |
| zrevrange <key> <start> <stop> [WITHSCORES]                  | 返回存储在有序集合 key 中的指定范围的元素。返回的元素按得分从高到低排列；如果得分相同，按字典排序；如果指定了 WITHSCORES 选项，将同时返回它们的得分 |
| zrangebyscore key min max [withscores] [limit offset count]  | 返回 key 的有序集合中分数在 min 和 max 之间的所有元素（包括分数等于 max 或 min 的元素）。元素从低分到高分排序，具有相同分数的元素按字典序排列，可选 LIMIT 参数指定返回结果的数量及区间（类似SQL中SELECT LIMIT offset, count） |
| zrevrangebyscore key max min [withscores] [limit offset count] | 返回有序集合中指定分数区间内的成员，分数由高到低排序         |
| zrem  <key>  <member> [<member> ...]                         | 删除 key 集合下，指定的 member 及其 score                    |
| zcount <key>  <min>  <max>                                   | 返回有序集 key 中，score 值在 min 和 max 之间（包括 min 和 max）的成员 |
| zcard <key>                                                  | 获取有序集合的元素个数                                       |
| zrank <key> <member>                                         | 返回有序集 key 中成员 member 的排名。其中有序集成员按 score 递增（从小到大）排列，排名以 0 为底 |
| zrevrank <key> <member>                                      | 返回有序集 key 中成员 member 的排名。其中有序集成员按 score 递减（从大到小）排列，排名以 0 为底 |





**参考文档：**<a href="http://www.redis.cn/documentation.html">Redis 官方文档</a>

