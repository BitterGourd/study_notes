## 数组与集合

### 数组(Array)

##### 定长数组

1. 创建

   ```scala
   // 方式一
   val array = Array(1,2,3)
   
   // 方式二
   val array = new Array[Int](3)
   ```

2. 操作

   ```scala
   // (1) 赋值
   array(0) = 99
   
   // (2) 转换为变长数组
   val ab = array.toBuffer
   ```

##### 变长数组

1.  创建

   ```scala
   // 方式一
   import scala.collection.mutable.ArrayBuffer
   val array = new ArrayBuffer[Int]()
   
   // 方式二
   val array = ArrayBuffer(1,2,3)
   ```

2. 操作

   ```scala
   // (1) += 在尾端添加一个元素，多个元素用（）包起来
   array += 11
   array += (11,2,3,5)
   
   // (2) ++= 在尾端添加集合
   array ++= Array(4,5,6)
   
   // (3) trimStart(n)/trimEnd(n) 移除最 前/后 n个元素
   array.trimEnd(2)
   
   // (4) insert(n,x) 在下标为n的位置插入单个元素x
   array.insert(2,99)
   
   // (5) insert(n,x1,x2,x3,..) 在下标为n的位置插入多个元素x1,x2,x3,.. 
   array.insert(2,44,55,66)
   
   // (6) remove(i) 移除下标为i的位置的元素
   array.remove(1)
   
   // (7) remove(i,n) 移除下标为i的位置开始的n个元素(包括i) 
   array.remove(1,4)
   
   // (8) toArray() 转换为长度不可变数组
   val arr = array.toArray
   ```

##### 遍历数组

1. 使用下标遍历

   ```scala
   for (index <- 0 to array.length - 1) { println(array(index)) }
   ```

2. 不使用下标遍历

   ```scala
   for (item <- array) { println(item) }
   ```

##### yield推导新数组

```scala
val arr = for(item <- array) yield item * 2
```

##### 常用函数

```scala
// (1) sum
array.sum

// (2) max/min
array.max

// (3) sorted 排序
array.sorted

// (4) reverse 反转
array.reverse
```

##### 多维数组

```scala
val arr = Array.ofDim[Double](3,4)
// 说明：二维数组中有三个一维数组，每个一维数组中有四个元素
```

##### Scala数组与Java数组互转

1. Scala数组转Java数组(List)

   ```scala
   val arr = ArrayBuffer("1", "2", "3")
   
   // 下面的 import 引入了我们需要的隐式函数 【这里就是隐式函数的应用】
   // implicit def bufferAsJavaList[A](b : scala.collection.mutable.Buffer[A]) : java.util.List[A]
   import scala.collection.JavaConversions.bufferAsJavaList
   
   // 这里使用了我们的隐式函数 bufferAsJavaList 完成两个 ArrayBuffer -> List 转换
   val javaArr = new ProcessBuilder(arr)
   // 返回的是 List<String>
   val arrList = javaArr.command()
   // 输出 [1, 2, 3]
   println(arrList)
   ```

2. Java数组(List)转Scala数组

   ```scala
   import scala.collection.JavaConversions.asScalaBuffer
   import scala.collection.mutable
   
   // java.util.List ==> Buffer
   val scalaArr: mutable.Buffer[String] = arrList
   ```




### 元组(Tuple)

> 元组可以存入多个不同类型的值, 目前 Scala 支持的元组最大长度为 22

##### 创建

```scala
// 方式一
val t1 = (1,2.0,"lisi")

// 方式二	可以通过a,b,c直接访问对应元素
val t1,(a,b,c) = ("zhangsan","lisi","wangwu")

// 方式三	Tuple1,Tuple2,Tuple3...
val tuple = Tuple3(1, 2, 3)
```

##### 查询

```scala
// 方式一	通过顺序号(_1,_2,_3,...)
t1._1

// 方式二	通过索引(productElement(index))
t1.productElement(1)

// 方式三	通过别名
scala> val t1,(a,b,c) = ("zhangsan","lisi","wangwu")
scala> a
res0: String = zhangsan
```

##### 遍历

```scala
for (item <- t1.productIterator) { println("item = " + item) }
```



### 列表(List)

##### 不可变列表

1. 创建

   ```scala
   val list = List(1,2,4)
   // Nil -> 空List集合
   ```

2. 操作

   ```scala
   // (1) :: 将给定的头和尾链接起来，创建一个新的列表
   9 :: List(5, 2)
   9 :: 5 :: 2 :: Nil
   
   // (2) +: 将元素插入到集合前
   val list1 = 22 +: list
   
   // (3) :+ 将元素插入到集合后
   val list2 = list :+ 99
   
   // (4) ++ 将两个集合合并成一个新的集合
   val list3 = list1 ++ list2
   
   // (5) ++: 将集合lsit1插入list2前面
   val list4 = list1 ++: list2
   
   // (6) .:::() 将集合lsit1插入list2后面
   val list5 = list1.:::(list2)
   ```

##### 可变列表

1. 创建

   ```scala
   // 方式一
   import scala.collection.mutable.ListBuffer
   val list = ListBuffer(1, 2, 3)
   
   // 方式二
   import scala.collection.mutable.ListBuffer
   val list = new ListBuffer[Int]()
   ```

2. 操作

   ```scala
   // (1) += 将元素插入到集合后，多个元素用 () 包起来
   list += 4
   
   // (2) append 将元素插入到集合后
   list.append(5)
   
   // (3) ++= 将集合list2追加到list1中(没有生成新的集合)
   list1 ++= list2
   
   // (4) ++ 将list1和list2合并成一个新的集合
   val list3 = list1 ++ list2
   
   // (5) :+ 将元素插入到集合后，并生成新的集合
   val list4 = list :+ 88
   
   // (5) +: 将元素插入到集合前，并生成新的集合
   val list5 = 99 +: list
   ```



### 映射(Map)

##### 不可变映射

1. 创建

   ```scala
   // 方式一
   val map = Map("001" -> "lisi","002" -> "wangwu")
   
   // 方式二
   import scala.collection.immutable.HashMap
   val map = new HashMap()
   // --> map: scala.collection.immutable.HashMap[Nothing,Nothing] = Map()
   ```

2. 操作

   ```scala
   // (1) + 添加/修改 一个元素(key 无则添加，有则修改 value)
   map + ("003" -> "libai")
   
   // (2) - 删除一个元素
   map - "001"
   ```

##### 可变映射

1. 创建

   ```scala
   // 方式一
   import scala.collection.mutable.Map
   val map = Map("001" -> "zhangsan","002" -> "lisi")
   
   // 方式二
   import scala.collection.mutable.HashMap
   val map = new HashMap()
   // --> map: scala.collection.mutable.HashMap[Nothing,Nothing] = Map()
   ```

2. 操作

   ```scala
   // (1) 更新映射中的值
   map.put("002","nanjing")	// 方式一
   map("002") = "nanjing1"		// 方式二
   
   // (2) 添加数据
   map.put("003","dongjing")		// 方式一
   map("004") = "xijing"			// 方式二
   map += ("005" -> "tianjing")	// 方式三
   
   // (3) 删除数据
   map -= "001"
   ```

##### Map 取值

```scala
/** 方式一	map(key)
 * 1) 如果 key 存在，则返回对应的值
 * 2) 如果 key 不存在，则抛出异常
 * 3) 在 Java 中,如果 key 不存在则返回 null
 */
val value = map("001")

/** 方式二	map.get(key)
 * 返回一个Option对象，要么是Some，要么是None
 */
println(map.get("001"))

/** 方式三	map.getOrElse()
 * 1) 如果key存在，返回key对应的值
 * 2) 如果 key 不存在，返回默认值
 */
println(map.getOrElse("001","default_value"))

/** 方式四	contains 检查 key 是否存在
 * 1) 如果 key 存在，则返回 true
 * 2) 如果 key 不存在，则返回 false
 */
if (map.contains("001")) println("存在" + map("001"))
else println("key 不存在")
```

##### 遍历

```scala
// 方式一 for((x,y) <- map)
for((x,y) <- map) { println(x + " -> " + y) }

// 方式二 keySet
val keyset = map.keySet
for(i <- keyset) { println(i + " -> " + map(i)) }

// 方式三
for (k <- map.keys) print("key = " + k + "\t")
for (v <- map.values) print("value = " + v + "\t")
```



### 集合(Set)

##### 不可变集合

1. 创建

   ```scala
   val set = Set(1,2,3)
   ```

2. 操作

   ```scala
   // (1) + 在集合中添加元素(生成新集合)
   val set2 = set + 4
   
   // (2) ++ 在集合中添加集合(生成新集合)
   val set3 = set ++ set2
   ```

##### 可变集合

1. 创建

   ```scala
   import scala.collection.mutable.Set
   val set = Set(1,2,3)
   ```

2. 操作

   ```scala
   // (1) += 添加元素，多个元素用（）包起来
   set += 4
   
   // (2) add	返回值：Boolean
   set.add(5)
   set add 6
   
   // (3) ++= 追加一个Set集合(没有创建新集合)
   set ++= set1
   
   // (4) -= 删除元素，多个元素用（）包起来
   set -= 1
   
   // (5) remove(element) 删除一个元素
   set.remove(2)
   ```



### Iterable常用方法

##### foreach

```scala
list.foreach(x => println(x))
map.foreach(x => println(x._1 + " -> " + x._2))
```

##### filter

```scala
// 过滤不满足传入函数(false)的元素,返回新集合
val res = list.filter(x => x % 2 == 0)
```

##### flatten

> 扁平化处理

```scala
val list = List(list1,list2)
// list: List[List[Int]] = List(List(1, 2, 3), List(4, 5, 6))
val res = list.flatten
// List(1, 2, 3, 4, 5, 6)
```

##### diff/intersect/union

> 差集、交集和并集，并集不去重

```scala
val list1 = List(1,2,3,4,5,6)
val list2 = List(4,5,6,7,8,9)
val diff_res = list1 diff list2				// List(1, 2, 3)
val intersect_res = list1 intersect list2	// List(4, 5, 6)
val union_res = list1 union list2			// List(1, 2, 3, 4, 5, 6, 4, 5, 6, 7, 8, 9)
```

##### map

```scala
List(1, 2, 3, 4).map(x => x * 2)
// List[Int] = List(2, 4, 6, 8)
```

##### flatMap

> 等价先 map 再 flatten

```scala
List(a b c, a b, c d).flatMap(x => x.split(" "))
// List[String] = List(a, b, c, a, b, c, d)
```

##### zip

> 拉链操作

```scala
val arr1 = Array(1,2,3)
val arr2 = Array("zhangsan","lisi","wangwu")
val res = arr1 zip arr2
// Array[(String, Int)] = Array((zhangsan,1), (lisi,2), (wangwu,3))
```

##### forall

> 对整个集合进行条件检查，只要一个元素返回 false，则最后结果为 false

```scala
val arr = Array(1,2,3)
val res = arr.forall(x => x > 2)	// false
```

##### partition

> 对集合按条件分组

```scala
val arr = Array(1,2,3,5,6,2,5)
val res = arr.partition(x => x % 2 == 0)
// (Array[Int], Array[Int]) = (Array(2, 6, 2),Array(1, 3, 5, 5))
```

##### reduce/reduceLeft/reduceRight

```scala
val arr = Array(1,2,3)
// Int = -4
val res1 = arr.reduce((x,y) => x - y)
// Int = -4
val res2 = arr.reduceLeft((x,y) => x - y)
// Int = 2	-> (1,(2,3))
val res3 = arr.reduceRight((x,y) => x - y)
```

##### fold/foldLeft/foldRight

> 可以把reduceLeft看做简化版的foldLeft
>
> def reduceLeft[B >: A](@deprecatedName('f) op: (B, A) => B): B =
> ​	if (isEmpty) throw new UnsupportedOperationException("empty.reduceLeft")
> ​	else tail.foldLeft[B](head)(op)

```scala
val arr = List(1, 2, 3, 4)
// -5	->	(((5, 1), 2), 3, 4)
val res1 = arr.foldLeft(5)((x,y) => x - y)
// 3	->	(1, (2, (3, (4, 5))))
val res2 = arr.foldRight(5)((x,y) => x - y)
```

##### scan/scanLeft/scanRight

> 对某个集合的所有元素做 fold 操作，但是会把产生的所有中间结果放置于一个集合中保存

```scala
(1 to 5).scanLeft(5)((x,y) => x - y)	// Vector(5, 4, 2, -1, -5, -10)
(1 to 5).scanLeft(5)((x,y) => x + y)	// Vector(5, 6, 8, 11, 15, 20)
```

##### groupBy/grouped

```scala
/** groupBy	返回一个新的Map集合，按照key将元素进行分组 */
List("boy" -> "xiaobei", "boy" -> "xiaonan", "girl" -> "xiaotian").groupBy(x => x._1)
//	Map(girl -> List((girl,xiaotian)), boy -> List((boy,xiaobei), (boy,xiaonan)))

/** grouped 根据给定的长度n, 进行分组, 每n个元素分为一组 */
Array(1,2,3,4,5).grouped(2)	// 返回值是 Iterator[Array[Int]]
```

##### mapValues

> 在Map数据结构里，对value进行操作

```scala
// Map(dog -> 3, cat -> 2)
Map("dog" -> List(1, 2, 4), "cat" -> List(2, 3)).mapValues(x => x.size)
```

##### stream

> stream 是一个集合。这个集合，可以用于存放无穷多个元素，但是这无穷个元素并不会一次性生产出来，而是需要用到多大的区间，就会动态的生产，末尾元素遵循 lazy 规则(即：要使用结果才进行计算)

```scala
// 创建的集合的第一个元素是 n , 后续元素生成的规则是 n + 1
def numsForm(n: BigInt) : Stream[BigInt] = n #:: numsForm(n + 1)

val stream = numsForm(1)
println("stream = " + stream)
//希望再流集合数据
println(stream.tail) //(2,?)，tail 表示返回除了第一个以外剩余的元素
println("stream =" + stream) // (1,2,?)
println("stream.head=" + stream.head)
// println("stream.last" + stream.last)	// 死循环，last 表示返回集合最后一个元素
```

##### view

> view 方法产出一个总是被懒执行的集合

```scala
val view = (1 to 5).view.scanLeft(5)((x,y) => x - y)
// SeqViewC(...)
println(view)
// 5 4 2 -1 -5 -10 
for (item <- res) print(item + " ")
```

##### Tips : 上边界和下边界

```scala
// <: 上边界
规定泛型可以适用的在继承关系中的范围，“<:” 后的是上限，表示不超过XXX
// >: 下边界 [B >: A]
参数可以随意传，不过
1）和 A 直系的，是 A 父类的还是父类处理，是 A 子类的按照 A 处理
2）和 A 无关的，一律按照 Object 处理
```

