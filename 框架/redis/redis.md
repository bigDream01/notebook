# redis6



## 简介

> ① 几乎覆盖了Memcached的绝大部分功能
>
> ② 数据都在内存中，支持持久化，主要用作备份恢复
>
> ③ 除了支持简单的key-value模式，还支持多种数据结构的存储，比如 list、set、hash、zset等。
>
> ④ 一般是作为缓存数据库辅助持久化的数据库
>
>  **单线程+多路IO复用(Redis)**



## redis的安装

一、**1.1.1.1.**     **准备工作：下载安装最新版的gcc编译器**

安装C 语言的编译环境

```shell
yum install centos-release-scl scl-utils-build

yum install -y devtoolset-8-toolchain

scl enable devtoolset-8 bash
```

**测试 gcc版本** 

**gcc --version**

​    <img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617200820385.png" alt="image-20210617200820385" style="zoom:150%;" />                 

二、 **下载redis-6.2.1.tar.gz放/opt目录**

三、**解压命令：tar -zxvf redis-6.2.1.tar.gz**

四、  **解压完成后进入目录：cd redis-6.2.1**

五、  **在redis-6.2.1目录下再次执行make命令（只是编译好）**

六、  **如果没有准备好C语言编译环境，make 会报错—Jemalloc/jemalloc.h：没有那个文件**

![image-20210617201106771](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617201106771.png)



**解决方案：运行make distclean**

 ![image-20210617201118213](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617201118213.png)

**在redis-6.2.1目录下再次执行make命令（只是编译好）**

 ![image-20210617201127585](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617201127585.png)

七、  **跳过make test 继续执行: make install**

##  后台启动和关闭

 

**一、**备份 **redis.conf**

拷贝一份redis.conf到其他目录

cp /opt/redis-3.2.5/redis.conf /myredis

**二、**后台启动设置**daemonize no改成yes**

修改redis.conf(128行)文件将里面的daemonize no 改成 yes，让服务在后台启动

**三、**Redis启动redis-server/myredis/redis.conf                        

**四、**用客户端访问：redis-cli

**五、**多个端口可以：redis-cli -p6379

六、测试验证： **ping**

七、**Redis****关闭**

> 单实例关闭：redis-cli shutdown
>
> 也可以进入终端后再关闭
>
> 多实例关闭，指定端口关闭：redis-cli -p 6379 shutdown

## 常用五大数据类型

#### key的操作

> **keys ***查看当前库所有key  (匹配：keys *1)
>
> **exists key**判断某个key是否存在
>
> **type key** 查看你的key是什么类型
>
> **del key**    删除指定的key数据
>
> unlink key  **根据value选择非阻塞删除**
>
> >  仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。
>
> **expire key 10**  10秒钟：为给定的key设置过期时间
>
> **ttl key** 查看还有多少秒过期，-1表示永不过期，-2表示已过期
>
>  **数据库的操作**
>
> **select**命令切换数据库
>
> **dbsize**查看当前数据库的key的数量
>
> **flushdb**清空当前库
>
> **flushall**通杀全部库

### string类型

**简介**

> **一个key对应一个value。** 类似于Java中的**string存储定对应**
>
> String类型是二进制安全的。意味着Redis的string可以包含任何数据。比如jpg图片或者序列化的对象。
>
> String类型是Redis最基本的数据类型，一个Redis中字符串value最多可以是512M

**常用命令**

> **set**  <key><value>添加键值对
>
> ​                               
>
> > *NX：当数据库中key不存在时，可以将key-value添加数据库
> >
> > *XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
> >
> > *EX：key的超时秒数
> >
> > *PX：key的超时毫秒数，与EX互斥
>
>  
>
> **get**  <key>查询对应键值
>
> **append** <key><value>将给定的<value> 追加到原值的末尾
>
> **strlen** <key>获得值的长度
>
> **setnx** <key><value>只有在 key 不存在时  设置 key 的值
>
>  
>
> **incr** <key>
>
> > 将 key 中储存的数字值增1
> >
> > 只能对数字值操作，如果为空，新增值为1
>
> **decr** <key>
>
> > 将 key 中储存的数字值减1
> >
> > 只能对数字值操作，如果为空，新增值为-1
>
> **incrby** / decrby <key><步长>将 key 中储存的数字值增减。自定义步长，并且是原子性的。
>
> >   **原子性**  
> >
> > > ​       所谓**原子**操作是指不会被线程调度机制打断的操作；  
> >
> > 这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。  
> >
> > （1）在单线程中， 能够在单条指令中完成的操作都可以认为是"原子操作"，因为中断只能发生于指令之间。  
> >
> > （2）在多线程中，不能被其它进程（线程）打断的操作就叫原子操作。  Redis单命令的原子性主要得益于Redis的单线程。  
> >
> > **案例：**  java中的i++是否是原子操作？**不是**
> >
> >   i=0;两个线程分别对i进行++100次,值是多少？ **2~200**            
> >
> > ![image-20210617202648094](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617202648094.png)        
>
> **mset** <key1><value1><key2><value2> ..... 
>
> >  同时设置一个或多个 key-value对 
>
> **mget** <key1><key2><key3> .....
>
> > 同时获取一个或多个 value 
>
> **msetnx** <key1><value1><key2><value2> ..... 
>
> > 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。
>
> **原子性，有一个失败则都失败**
>
>  
>
> **getrange** <key><起始位置><结束位置>
>
> > 获得值的范围，类似java中的substring，**前包，后包**
>
> **setrange** <key><起始位置><value>
>
> > 用 <value> 覆写<key>所储存的字符串值，从<起始位置>开始(**索引从0****开始**)。
>
> **setex <key><****过期时间****><value>**
>
> > 设置键值的同时，设置过期时间，单位秒。
>
> **getset** <key><value>
>
> > 以新换旧，设置了新值同时获得旧值。

**数据结构**

> String的数据结构为简单动态字符串(Simple Dynamic String,缩写SDS)。是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配.
>
> ![image-20210617202848603](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617202848603.png)
>
> 如图中所示，内部为当前字符串实际分配的空间capacity一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。需要注意的是字符串最大长度为512M。

### List类型

**简介**

> **单键多值**  ===> java中的**list集合**
>
> **单key多个value**
>
> Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
>
> 它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。
>
> ​                               ![image-20210617203039189](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617203039189.png)

**常见操作**

> **lpush/rpush** <key><value1><value2><value3> .... 从左边/右边插入一个或多个值。
>
> **lpop/rpop** <key>从左边/右边吐出一个值。值在键在，值光键亡。
>
>  
>
> **rpoplpush** <key1><key2>从<key1>列表右边吐出一个值，插到<key2>列表左边。
>
>  
>
> **lrange** <key><start><stop>
>
> 按照索引下标获得元素(从左到右)
>
> **lrange** mylist 0 -1  0左边第一个，-1右边第一个，（0-1表示获取所有）
>
> **lindex** <key><index>按照索引下标获得元素(从左到右)
>
> **llen** <key>获得列表长度 
>
>  
>
> **linsert** <key> before <value><newvalue>在<value>的后面插入<newvalue>插入值
>
> **lrem** <key><n><value>从左边删除n个value(从左到右)
>
> **lset**<key><index><value>将列表key下标为index的值替换成value

**数据结构**

> **List的数据结构为快速链表quickList。**
>
> 首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。
>
> 它将所有的元素紧挨着一起存储，分配的是一块连续的内存。
>
> 当数据量比较多的时候才会改成quicklist。
>
> 因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。
>
> ​                             ![image-20210617205504164](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617205504164.png)  
>
> Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。

### set类型

**简介**

> **单键多值**  ===> java中的**set集合** （其是不包含重复元素的类似于set集合）
>
> Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。
>
> Redis的Set是string类型的无序集合。它底层其实是一个value为null的hash表，所以添加，删除，查找的**复杂度都是O(1)**。
>
> 一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变

**常用命令**

> **sadd** <key><value1><value2> ..... 
>
> 将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略
>
> **smembers** <key>取出该集合的所有值。
>
> **sismember** <key><value>判断集合<key>是否为含有该<value>值，有1，没有0
>
> **scard**<key>返回该集合的元素个数。
>
> **srem** <key><value1><value2> .... 删除集合中的某个元素。
>
> **spop** <key>**随机从该集合中吐出一个值。**
>
> **srandmember** <key><n>随机从该集合中取出n个值。不会从集合中删除 。
>
> **smove** <source><destination>value把集合中一个值从一个集合移动到另一个集合
>
> **sinter** <key1><key2>返回两个集合的交集元素。
>
> **sunion** <key1><key2>返回两个集合的并集元素。
>
> **sdiff** <key1><key2>返回两个集合的**差集**元素(key1中的，不包含key2中的)

**数据结构**

> Set数据结构是dict字典，字典是用哈希表实现的。
>
> Java中HashSet的内部实现使用的是**HashMap，**只不过所有的value都指向同一个对象。Redis的set结构也是一样，它的内部也使用**hash结构，所有的value都指向同一个内部值。**

### hash类型

**简介**

> Redis hash 是一个键值对集合。**对应的数据存储模式是java中的对象**
>
> Redis hash是一个string类型的**field和value的映射表**，hash特别适合用于存储对象。
>
> **key存储对象名**
>
> **field存储属性名**
>
> **value存储属性值**
>
> 类似Java里面的Map<String,Object>
>
> 用户ID为查找的key，存储的value用户对象包含姓名，年龄，生日等信息，如果用普通的key/value结构来存储
>
> ![image-20210617210052500](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617210052500.png)

**常见操作**

> **hset** <key><field><value>给<key>集合中的 <field>键赋值<value>
>
> **hget** <key1><field>从<key1>集合<field>取出 value 
>
> **hmset** <key1><field1><value1><field2><value2>... 批量设置hash的值
>
> **hexists**<key1><field>查看哈希表 key 中，给定域 field 是否存在。 
>
> **hkeys** <key>列出该hash集合的所有field
>
> **hvals** <key>列出该hash集合的所有value
>
> **hincrby** <key><field><increment>为哈希表 key 中的域 field 的值加上增量 1  -1
>
> **hsetnx** <key><field><value>将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 .

**数据结构**

> sh类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable。



### SSet**类型**

**简介**

> Redis有序集合zset与普通集合set非常相似，是一个没有重复元素的字符串集合。
>
> 不同之处是有序集合的每个成员都关联了一个**评分（score）**,这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了 。
>
> 因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。
>
> 访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

**常用命令**

> **zadd** <key><score1><value1><score2><value2>…
>
> >  将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
>
> **zrange <key><start><stop> [WITHSCORES]**  
>
> > 返回有序集 key 中，下标在<start><stop>之间的元素
> >
> > 带WITHSCORES，可以让分数一起和值返回到结果集。
>
> **zrangebyscore** key minmax [withscores] [limit offset count]
>
> > 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 
>
> **zrevrangebyscore** key maxmin [withscores] [limit offset count]        
>
> 同上，改为从大到小排列。 
>
> **zincrby** <key><increment><value>   为元素的score加上增量
>
> **zrem** **<key><value>删除该集合下，指定值的元素**
>
> **zcount** <key><min><max>统计该集合，分数区间内的元素个数 
>
> **zrank** <key><value>返回该值在集合中的排名，从0开始。

**数据结构**

> SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构Map<String, Double>，可以给每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。
>
> zset底层使用了两个数据结构
>
> （1）hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。
>
> （2）跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。
>
>  **跳跃表**
>
> > **1、简介**
> >
> >   有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家排名等。对于有序集合的底层实现，可以用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所有效率低。Redis采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。
> >
> > **2、实例**
> >
> >   对比有序链表和跳跃表，从链表中查询出51
> >
> > （1）  有序链表
> >
> > ​                               
> >
> > 要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。
> >
> > （2）  跳跃表
> >
> >  ![image-20210617210807860](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617210807860.png)
> >
> > 从第2层开始，1节点比51节点小，向后比较。
> >
> > 21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层
> >
> > 在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下
> >
> > 在第0层，51节点为要查找的节点，节点被找到，共查找4次。
> >
> >  ![image-20210617210815673](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210617210815673.png)
> >
> > 从此可以看出跳跃表比有序链表效率要高



## redis的配置文件

 13、redis的配置文件：在redis根目录下提供redis.conf配置文件；
                     可以配置一些redis服务端运行时的一些参数；
		     		如果不使用配置文件，那么redis会按照默认的参数运行；
		   		  如果使用配置文件，在启动redis服务时必须指定所使用的配置文件。 
  		  1)、redis配置文件中关于网络的配置：
       			 port：指定redis服务所使用的端口，默认使用6379。
					bind: 配置客户端连接redis服务时，所能使用的ip地址，默认可以使用redis服务所在主机上任何一个ip都可以;一般情况下，都							会配置一个ip，而且通常是一个真实。 
	      						如果配置了port和bind，则客户端连接redis服务时，必须指定端口和ip	

```shell
				redis-cli -h 192.168.11.128 -p 6380
	 			redis-cli -h 192.168.11.128 -p 6380 shutdown
```

​				tcp-keepalive:连接保活策略。
​			 2)、常规配置：
   					 loglevel:配置日志级别,开发阶段配置debug,上线阶段配置notice或者warning.
​						logfile：指定日志文件。redis在运行过程中，会输出一些日志信息；默认情况下，这些日志信息会输出到控制台；我们可									以使用								

​								logfile配置日志文件，使redis把日志信息输出到指定文件中。
   					 databases：配置redis服务默认创建的数据库实例个数，默认值是16。
​			 3)、安全配置：（一般不使用）
  					  requirepass：设置访问redis服务时所使用的密码；默认不使用。
​             									此参数必须在protected-mode=yes时才起作用。
​                 								一旦设置了密码验证，客户端连接redis服务时，必须使用密码连接：redis-cli -h ip -p port -a pwd

## redis和java的整合



导入依赖

```xml
<dependency>
     <groupId>redis.clients</groupId>
     <artifactId>jedis</artifactId>
     <version>3.2.0</version>
 </dependency>


```

**注意**

> 禁用Linux的防火墙：Linux(CentOS7)里执行命令
>
> ```shell
> systemctl stop/disable firewalld.service
> ```
>
> redis.conf中注释掉bind 127.0.0.1 ,然后
>
>  **protected-mode no**(在redis.conf文件中)

## **redis和springboot整合**

> ```xml
> <!-- redis --> 
> <dependency>
> 	<groupId>org.springframework.boot</groupId> 
> 	<artifactId>spring-boot-starter-data-redis</artifactId> 
> </dependency> 
> <!-- spring2.X集成redis所需common-pool2-->
> <dependency> 
> 	<groupId>org.apache.commons</groupId>
> 	<artifactId>commons-pool2</artifactId> 
> <version>2.6.0</version> </dependency>
> ```
>
> **application.properties配置redis配置**
>
> ```yaml
> #Redis服务器地址
> spring.redis.host=192.168.140.136
> #Redis服务器连接端口
> spring.redis.port=6379
> #Redis数据库索引（默认为0）
> spring.redis.database= 0
> #连接超时时间（毫秒）
> spring.redis.timeout=1800000
> #连接池最大连接数（使用负值表示没有限制）
> spring.redis.lettuce.pool.max-active=20
> #最大阻塞等待时间(负数表示没限制)
> spring.redis.lettuce.pool.max-wait=-1
> #连接池中的最大空闲连接
> spring.redis.lettuce.pool.max-idle=5
> #连接池中的最小空闲连接
> spring.redis.lettuce.pool.min-idle=0
> ```
>
> **添加配置类**
>
> ```java
> @EnableCaching
> @Configuration
> public class RedisConfig extends CachingConfigurerSupport {
> 
>     @Bean
>     public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
>         RedisTemplate<String, Object> template = new RedisTemplate<>();
>         RedisSerializer<String> redisSerializer = new StringRedisSerializer();
>         Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
>         ObjectMapper om = new ObjectMapper();
>         om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
>         om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
>         jackson2JsonRedisSerializer.setObjectMapper(om);
>         template.setConnectionFactory(factory);
> //key序列化方式
>         template.setKeySerializer(redisSerializer);
> //value序列化
>         template.setValueSerializer(jackson2JsonRedisSerializer);
> //value hashmap序列化
>         template.setHashValueSerializer(jackson2JsonRedisSerializer);
>         return template;
>     }
> 
>     @Bean
>     public CacheManager cacheManager(RedisConnectionFactory factory) {
>         RedisSerializer<String> redisSerializer = new StringRedisSerializer();
>         Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
> //解决查询缓存转换异常的问题
>         ObjectMapper om = new ObjectMapper();
>         om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
>         om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
>         jackson2JsonRedisSerializer.setObjectMapper(om);
> // 配置序列化（解决乱码的问题）,过期时间600秒
>         RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
>                 .entryTtl(Duration.ofSeconds(600))
>                 .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
>                 .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
>                 .disableCachingNullValues();
>         RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
>                 .cacheDefaults(config)
>                 .build();
>         return cacheManager;
>     }
> }
> 
> ```

## **事务**

**简介**

> Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
>
> Redis事务的主要作用就是串联多个命令防止别的命令插队。

**Multi**、**Exec**、**discard**

> 从输入Multi命令开始，输入的命令都会依次进入命令队列中，但不会执行，直到输入Exec后，Redis会将之前的命令队列中的命令依次执行。
>
> **组队的过程中可以通过discard来放弃组队。** 
>
> **注意点**
>
> > redis 的事务是不具备原子性的，其在执行的过程中出现执行错误，其的语句是能够正常执行的
> >
> > redis的事务在语句添加到队列中的时候是具备原子性的，一个语句添加错误，其他的语句不能执行（一般是语法错误）

**事务中的冲突问题**

> **悲观锁**
>
> > 类似于synchronized，直接对这个数据进行单线程化。执行完这个线程其他的线程才能拿到这个数据
>
> **乐观锁**
>
> > 就是给这个数据加上一个版本号，操作完数据后将版本号更新。其他的线程在发现数据版本号与初始拿到的版本不一致的时，放弃对数据的更新。

 **WATCH** **key** **[key ...]**

> 在执行multi之前，先执行watch key1 [key2],可以监视一个(或多个) key ，如果在事务**执行之前这个**(**或这些) key** **被其他命令所改动，那么事务将被打断。**
>
> 类似于个这数据添加了一个乐观锁
>
> **unwatch** key取消对数据的监视

