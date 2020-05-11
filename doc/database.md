## 9. 数据库

### 9.1 数据库隔离级别有哪些，各自的含义是什么，MYSQL 默认的隔离级别是是什么。

1. **未提交读(Read Uncommitted)**：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据。
2. **提交读(Read Committed)**：只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别 (不重复读)。
3. **可重复读(Repeated Read)**：可重复读。在同一个事务内的查询都是事务开始时刻一致的，InnoDB默认级别。在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻象读。
4. **串行读(Serializable)**：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞。

MYSQL默认是RepeatedRead级别

### 9.2 MYSQL 有哪些存储引擎，各自优缺点。

1. **MyISAM**： 拥有较高的插入，查询速度，但不支持事务。
2. **InnoDB** ：5.5版本后Mysql的默认数据库，事务型数据库的首选引擎，支持ACID事务，支持行级锁定。
3. **BDB**： 源自Berkeley DB，事务型数据库的另一种选择，支持COMMIT和ROLLBACK等其他事务特性。
4. **Memory** ：所有数据置于内存的存储引擎，拥有极高的插入，更新和查询效率。但是会占用和数据量成正比的内存空间。并且其内容会在Mysql重新启动时丢失。
5. **Merge** ：将一定数量的MyISAM表联合而成一个整体，在超大规模数据存储时很有用。
6. **Archive** ：非常适合存储大量的独立的，作为历史记录的数据。因为它们不经常被读取。Archive拥有高效的插入速度，但其对查询的支持相对较差。
7. **Federated**： 将不同的Mysql服务器联合起来，逻辑上组成一个完整的数据库。非常适合分布式应用。
8. **Cluster/NDB** ：高冗余的存储引擎，用多台数据机器联合提供服务以提高整体性能和安全性。适合数据量大，安全和性能要求高的应用。
9. **CSV**： 逻辑上由逗号分割数据的存储引擎。它会在数据库子目录里为每个数据表创建一个.CSV文件。这是一种普通文本文件，每个数据行占用一个文本行。CSV存储引擎不支持索引。
10. **BlackHole** ：黑洞引擎，写入的任何数据都会消失，一般用于记录binlog做复制的中继。

[存储引擎]([https://baike.baidu.com/item/%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E](https://baike.baidu.com/item/存储引擎))

### 9.3 高并发下，如何做到安全的修改同一行数据。

* 使用悲观锁 悲观锁本质是当前只有一个线程执行操作，结束了唤醒其他线程进行处理。
* 也可以缓存队列中锁定主键。

### 9.4 乐观锁和悲观锁是什么，INNODB 的行级锁有哪 2 种，解释其含义。

* 乐观锁是设定每次修改都不会冲突，只在提交的时候去检查。
* 悲观锁设定每次修改都会冲突，持有排他锁。

行级锁分为 **共享锁** 和 **排他锁** 两种，**共享锁**又称读锁，**排他锁**又称写锁

### 9.5 SQL 优化的一般步骤是什么，怎么看执行计划，如何理解其中各个字段的含义。

查看慢日志（show [session|gobal] status ），定位慢查询，查看慢查询执行计划 根据执行计划确认优化方案

Explain sql

* select_type：表示select类型。常见的取值有SIMPLE（简单表，即不使用连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（union中的第二个或者后面的查询语句）、SUBQUERY（子查询中的第一个SELECT）等。
* talbe：输出结果集的表。
* type：表的连接类型。性能由高到底：system（表中仅有一行）、const（表中最多有一个匹配行）、eq_ref、ref、ref_null、index_merge、unique_subquery、index_subquery、range、idnex等
* possible_keys：查询时，可能使用的索引
* key：实际使用的索引
* key_len：索引字段的长度
* rows：扫描行的数量
* Extra：执行情况的说明和描述

 

**Oracle优化**

1. 对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引。

2. 应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如：

   select id from t where num is null
   可以在num上设置默认值0，确保表中num列没有null值，然后这样查询：
   select id from t where num=0

3. 应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。

4. 应尽量避免在 where 子句中使用 or 来连接条件，否则将导致引擎放弃使用索引而进行全表扫描，如：
   select id from t where num=10 or num=20
   可以这样查询：
   select id from t where num=10
   union all
   select id from t where num=20

5. in 和 not in 也要慎用，否则会导致全表扫描，如：
   select id from t where num in(1,2,3)
   对于连续的数值，能用 between 就不要用 in 了：
   select id from t where num between 1 and 3

6. 下面的查询也将导致全表扫描：
   select id from t where name like ‘%abc%’
   若要提高效率，可以考虑全文检索。

7. 如果在 where 子句中使用参数，也会导致全表扫描。因为SQL只有在运行时才会解析局部变量，但优化程序不能将访问计划的选择推迟到运行时；它必须在编译时进行选择。然而，如果在编译时建立访问计划，变量的值还是未知的，因而无法作为索引选择的输入项。如下面语句将进行全表扫描：
   select id from t where num=@num
   可以改为强制查询使用索引：
   select id from t with(index(索引名)) where num=@num

8. 应尽量避免在 where 子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描。如：
   select id from t where num/2=100
   应改为:
   select id from t where num=100*2

9. 应尽量避免在where子句中对字段进行函数操作，这将导致引擎放弃使用索引而进行全表扫描。如：
   select id from t where substring(name,1,3)=‘abc’ // oracle总有的是substr函数。
   select id from t where datediff(day,createdate,‘2005-11-30’)=0 //查过了确实没有datediff函数。
   应改为:
   select id from t where name like ‘abc%’
   select id from t where createdate>=‘2005-11-30’ and createdate<‘2005-12-1’ //
   oracle 中时间应该把char 转换成 date 如： createdate >= to_date(‘2005-11-30’,‘yyyy-mm-dd’)

10. 不要在 where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用索引。

11. 在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。

12. 不要写一些没有意义的查询，如需要生成一个空表结构：
    select col1,col2 into #t from t where 1=0
    这类代码不会返回任何结果集，但是会消耗系统资源的，应改成这样：
    create table #t(…)

13. 很多时候用 exists 代替 in 是一个好的选择：
    select num from a where num in(select num from b)
    用下面的语句替换：
    select num from a where exists(select 1 from b where num=a.num)

14. 并不是所有索引对查询都有效，SQL是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL查询可能不会去利用索引，如一表中有字段sex，male、female几乎各一半，那么即使在sex上建了索引也对查询效率起不了作用。

15. 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。

16. 应尽可能的避免更新 clustered 索引数据列，因为 clustered 索引数据列的顺序就是表记录的物理存储顺序，一旦该列值改变将导致整个表记录的顺序的调整，会耗费相当大的资源。若应用系统需要频繁更新 clustered 索引数据列，那么需要考虑是否应将该索引建为 clustered 索引。

17. 尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。

18. 尽可能的使用 varchar/nvarchar 代替 char/nchar ，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些。

19. 任何地方都不要使用 select * from t ，用具体的字段列表代替“*”，不要返回用不到的任何字段。

20. 尽量使用表变量来代替临时表。如果表变量包含大量数据，请注意索引非常有限（只有主键索引）。

21. 避免频繁创建和删除临时表，以减少系统表资源的消耗。

22. 临时表并不是不可使用，适当地使用它们可以使某些例程更有效，例如，当需要重复引用大型表或常用表中的某个数据集时。但是，对于一次性事件，最好使用导出表。

23. 在新建临时表时，如果一次性插入数据量很大，那么可以使用 select into 代替 create table，避免造成大量 log ，以提高速度；如果数据量不大，为了缓和系统表的资源，应先create table，然后insert。

24. 如果使用到了临时表，在存储过程的最后务必将所有的临时表显式删除，先 truncate table ，然后 drop table ，这样可以避免系统表的较长时间锁定。

25. 尽量避免使用游标，因为游标的效率较差，如果游标操作的数据超过1万行，那么就应该考虑改写。

26. 使用基于游标的方法或临时表方法之前，应先寻找基于集的解决方案来解决问题，基于集的方法通常更有效。

27. 与临时表一样，游标并不是不可使用。对小型数据集使用 FAST_FORWARD 游标通常要优于其他逐行处理方法，尤其是在必须引用几个表才能获得所需的数据时。在结果集中包括“合计”的例程通常要比使用游标执行的速度快。如果开发时间允许，基于游标的方法和基于集的方法都可以尝试一下，看哪一种方法的效果更好。

28. 在所有的存储过程和触发器的开始处设置 SET NOCOUNT ON ，在结束时设置 SET NOCOUNT OFF 。无需在执行存储过程和触发器的每个语句后向客户端发送 DONE_IN_PROC 消息。

29. 尽量避免大事务操作，提高系统并发能力。

30. 尽量避免向客户端返回大数据量，若数据量过大，应该考虑相应需求是否合理。


**MySQL优化**

[MySQL优化的步骤详解](https://blog.csdn.net/hsd2012/article/details/51106285)

### 9.6 数据库会死锁吗，举一个死锁的例子，mysql 怎么解决死锁。

产生死锁的原因主要是：

1. 系统资源不足。
2. 进程运行推进的顺序不合适。
3. 资源分配不当等。

如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则就会因争夺有限的资源而陷入死锁。其次，进程运行推进顺序与速度不同，也可能产生死锁。
产生死锁的四个必要条件：

1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。
   这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

这里提供两个解决数据库死锁的方法：

1. 重启数据库（谁用谁知道）
2. 杀掉抢资源的进程：
   先查哪些进程在抢资源：SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;
   杀掉它们：Kill trx_mysql_thread_id；

### 9.7 MYsql 的索引原理，索引的类型有哪些，如何创建合理的索引，索引如何优化。

索引是通过复杂的算法，提高数据查询性能的手段。从磁盘IO到内存IO的转变
普通索引，主键，唯一，单列/多列索引建索引的几大原则
>
1. 最左前缀匹配原则，非常重要的原则，mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配，比如a = 1 and b = 2 and c 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。
2. = 和 in 可以乱序，比如：a = 1 and b = 2 and c = 3 建立(a,b,c)索引可以任意顺序，mysql的查询优化器会帮你优化成索引可以识别的形式。
3. 尽量选择区分度高的列作为索引,区分度的公式是count(distinct col)/count(*)，表示字段不重复的比例，比例越大我们扫描的记录数越少，唯一键的区分度是1，而一些状态、性别字段可能在大数据面前区分度就是0，那可能有人会问，这个比例有什么经验值吗？使用场景不同，这个值也很难确定，一般需要join的字段我们都要求是0.1以上，即平均1条扫描10条记录。
4. 索引列不能参与计算，保持列“干净”，比如from_unixtime(create_time) = ’2014-05-29’就不能使用到索引，原因很简单，b+树中存的都是数据表中的字段值，但进行检索时，需要把所有元素都应用函数才能比较，显然成本太大。所以语句应该写成create_time = unix_timestamp(’2014-05-29’)。
5. 尽量的扩展索引，不要新建索引。比如表中已经有a的索引，现在要加(a,b)的索引，那么只需要修改原来的索引即可。

[mysql索引类型 normal, unique, full text](https://www.cnblogs.com/cq-home/p/3482101.html)

### 9.8 聚集索引和非聚集索引的区别。

**聚簇**：就是索引和记录紧密在一起。
**非聚簇索引**：索引文件和数据文件分开存放，索引文件的叶子页只保存了主键值，要定位记录还要去查找相应的数据块。

### 9.9 数据库中 BTREE 和 B+tree 区别。

B+树是btree的变种，本质都是btree，b+tree与B-Tree相比，B+Tree有以下不同点：

* 每个节点的指针上限为2d而不是2d+1。
* 内节点不存储data，只存储key。
* 叶子节点不存储指针。

### 9.10 Btree 怎么分裂的，什么时候分裂，为什么是平衡的。

Key 超过1024才分裂B树为甚会分裂？ 

因为随着数据的增多，一个结点的key满了，为了保持B树的特性，就会产生分裂，就像**红黑树**和**AVL树**为了保持树的性质需要进行旋转一样。

### 9.11 ACID 是什么。

* A（atomic）：原子性，要么都提交，要么都失败，不能一部分成功，一部分失败。
* C（consistent）：一致性，事物开始及结束后，数据的一致性约束没有被破坏
* I（isolation）：隔离性，并发事物间相互不影响，互不干扰。
* D（durability）：持久性，已经提交的事物对数据库所做的更新必须永久保存。即便发生崩溃，也不能被回滚或数据丢失。

### 9.12 Mysql 怎么优化 table scan 的。

* 避免在where子句中对字段进行is null判断
* 应尽量避免在where 子句中使用 != 或 <操作符，否则将引擎放弃使用索引而进行全表扫描。
* 避免在where 子句中使用or 来连接条件
* in 和 not in 也要慎用
* Like 查询（非左开头）
* 使用NUM=@num参数这种
* where 子句中对字段进行表达式操作 num / 2 = XX
* 在where子句中对字段进行函数操作

### 9.13 如何写 sql 能够有效的使用到复合索引。

由于复合索引的组合索引，类似多个木板拼接在一起，如果中间断了就无法用了，所以要能用到复合索引，首先开头(第一列)要用上，比如index(a,b) 这种，我们可以 select table tname where a=XX 用到第一列索引 如果想用第二列 可以 and b = XX 或者and b like ‘TTT%’

### 9.14 mysql 中 in 和 exists 区别。

mysql中的in语句是把外表和内表作hash 连接，而exists语句是对外表作loop循环，每次loop循环再对内表进行查询。一直大家都认为exists比in语句的效率要高，这种说法其实是不准确的。这个是要区分环境的。
* 如果查询的两个表大小相当，那么用in和exists差别不大。
* 如果两个表中一个较小，一个是大表，则子查询表大的用exists，子查询表小的用in
* not in 和 not exists 如果查询语句使用了not in 那么内外表都进行全表扫描，没有用到索引；而not extsts 的子查询依然能用到表上的索引。所以无论那个表大，用not exists都比not in要快。

1. **EXISTS** 只返回TRUE或FALSE，不会返回UNKNOWN。
2. **IN** 当遇到包含NULL的情况，那么就会返回UNKNOWN。

### 9.15 数据库自增主键可能的问题。

* 在分库分表时可能会生成重复主键
* 利用自增比例达到唯一
* 自增1，2，3 等

### 9.16. 用过哪些 MQ，和其他 mq 比较有什么优缺点，MQ 的连接是线程安全的吗，你们公司的MQ 服务架构怎样的。
根据实际情况说明
我们公司用 ActiveMQ 因为业务比较简单 只有转码功能，而 ActiveMQ 比较简单
如果是分布式的建议用kafka

[【消息队列MQ】各类MQ比较](https://blog.csdn.net/sunxinhere/article/details/7968886)

### 9.17 MQ 系统的数据如何保证不丢失。

基本都是对数据进行持久化，多盘存储

### 9.18 Rabbitmq 如何实现集群高可用。

集群是保证服务可靠性的一种方式，同时可以通过水平扩展以提升消息吞吐能力。RabbitMQ是用分布式程序设计语言erlang开发的，所以天生就支持集群。接下来，将介绍 RabbitMQ 分布式消息处理方式、集群模式、节点类型，并动手搭建一个高可用集群环境，最后通过java程序来验证集群的高可用性。

**三种分布式消息处理方式**
RabbitMQ分布式的消息处理方式有以下三种：

1. Clustering：不支持跨网段，各节点需运行同版本的Erlang和RabbitMQ, 应用于同网段局域网。
2. Federation：允许单台服务器上的Exchange或Queue接收发布到另一台服务器上Exchange或Queue的消息, 应用于广域网。
3. Shovel：与Federation类似，但工作在更低层次。
   RabbitMQ对网络延迟很敏感，在LAN环境建议使用clustering方式;在WAN环境中，则使用Federation或Shovel。我们平时说的RabbitMQ集群，说的就是clustering方式，它是RabbitMQ内嵌的一种消息处理方式，而Federation或Shovel则是以plugin形式存在。

[基于高可用配置的RabbitMQ集群实践](https://my.oschina.net/jiaoyanli/blog/822011)

[高可用 RabbitMQ 集群自动化部署解决方案](https://www.ibm.com/developerworks/cn/opensource/os-cn-RabbitMQ/)

### 9.19 redis 的 list 结构相关的操作。

* LPUSH
* LPUSHX
* RPUSH
* RPUSHX
* LPOP
* RPOP
* BLPOP
* BRPOP
* LLEN
* LRANGE

[Redis命令参考简体中文版 2.4.1 documentation](https://redis.readthedocs.io/en/2.4/list.html)

### 9.20 Redis 的数据结构都有哪些。

* **字符串(strings)**：存储整数（比如计数器）和字符串，有些公司也用来存储 json/pb 等序列化数据，并不推荐，浪费内存。
* **哈希表(hashes)**：存储配置，对象（比如用户、商品），优点是可以存取部分key，对于经常变化的或者部分key要求atom操作的适合。
* **列表(lists)**：可以用来存最新用户动态，时间轴，优点是有序，确定是元素可重复，不去重。
* **集合(sets)**：无序，唯一，对于要求严格唯一性的可以使用。
* **有序集合(sorted sets)**：集合的有序版，很好用，对于排名之类的复杂场景可以考虑。

### 9.21 Redis 的使用要注意什么，讲讲持久化方式，内存设置，集群的应用和优劣势，淘汰策略等。

**持久化方式**：RDB时间点快照 AOF记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。

**内存设置**：maxmemory used_memory

**虚拟内存**：vm-enabled yes


3.0采用Cluster方式，
Redis集群相对单机在功能上存在一些限制， 需要开发人员提前了解，
在使用时做好规避。 限制如下：

1. key 批量操作支持有限。 如 mset、 mget， 目前只支持具有相同slot值的key执行批量操作。 对于映射为不同slot值的key由于执行 mset、 mget 等操作可能存在于多个节点上因此不被支持。
2. key 事务操作支持有限。 同理只支持多key在同一节点上的事务操作， 当多个key分布在不同的节点上时无法使用事务功能。
3. key 作为数据分区的最小粒度， 因此不能将一个大的键值对象如 hash、 list等映射到不同的节点。
4. 不支持多数据库空间。 单机下的Redis可以支持16个数据库， 集群模式下只能使用一个数据库空间， 即db0。
5. 复制结构只支持一层， 从节点只能复制主节点， 不支持嵌套树状复制结构。

Redis Cluster是Redis的分布式解决方案， 在3.0版本正式推出， 有效地解
决了Redis分布式方面的需求。 当遇到单机内存、 并发、 流量等瓶颈时， 可
以采用Cluster架构方案达到负载均衡的目的。 之前， Redis分布式方案一般
有两种：

* **客户端分区方案**：优点是分区逻辑可控， 缺点是需要自己处理数据路由、 高可用、 故障转移等问题。
* **代理方案**：优点是简化客户端分布式逻辑和升级维护便利， 缺点是加重架构部署复杂度和性能损耗。


现在官方为我们提供了专有的集群方案： 

* Redis Cluster， 它非常优雅地解决了Redis集群方面的问题， 因此理解应用好 Redis Cluster 将极大地解放我们使用分布式Redis的工作量， 同时它也是学习分布式存储的绝佳案例。
* LRU（近期最少使用算法）
* TTL（超时算法） 去除ttl最大的键值

[Redis 数据淘汰机制](https://wiki.jikexueyuan.com/project/redis/data-elimination-mechanism.html)

[Redis 内存使用优化与存储](https://www.infoq.cn/article/tq-redis-memory-usage-optimization-storage/)

[Redis 集群教程](redis.cn/topics/cluster-tutorial.html)

### 9.22 redis2 和 redis3 的区别，redis3 内部通讯机制。

集群方式的区别，3采用Cluster，2采用客户端分区方案和代理方案
通信过程说明：

1. 集群中的每个节点都会单独开辟一个TCP通道， 用于节点之间彼此
   通信， 通信端口号在基础端口上加10000。
2. 每个节点在固定周期内通过特定规则选择几个节点发送ping消息。
3. 接收到ping消息的节点用pong消息作为响应。

### 9.23 当前 redis 集群有哪些玩法，各自优缺点，场景。
当缓存使用、持久化使用、消息队列

### 9.24 Memcache 的原理，哪些数据适合放在缓存中。

* 基于libevent的事件处理
* 内置内存存储方式SLab Allocation机制
* 并不单一的数据删除机制
* 基于客户端的分布式系统
* 变化频繁，具有不稳定性的数据,不需要实时入库, (比如用户在线状态、在线人数…)，门户网站的新闻等，觉得页面静态化仍不能满足要求，可以放入到memcache中.(配合jquey的ajax请求)

### 9.25 redis 和 memcached 的内存管理的区别。
Memcached默认使用Slab Allocation机制管理内存，其主要思想是按照预先规定的大小，将分配的内存分割成特定长度的块以存储相应长度的key-value数据记录，以完全解决内存碎片问题。
Redis的内存管理主要通过源码中zmalloc.h和zmalloc.c两个文件来实现的。
在Redis中，并不是所有的数据都一直存储在内存中的。这是和Memcached相比一个最大的区别。

### 9.26 Redis 的并发竞争问题如何解决，了解 Redis 事务的 CAS 操作吗。

Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，但是在 Jedis 客户端对 Redis 进行并发访问时会发生连接超时、数据转换错误、阻塞、客户端关闭连接等问题，这些问题均是由于客户端连接混乱造成。对此有2种解决方法：

1. 客户端角度，为保证每个客户端间正常有序与Redis进行通信，对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。
2. 服务器角度，利用setnx实现锁。
   MULTI，EXEC，DISCARD，WATCH 四个命令是 Redis 事务的四个基础命令。其中：
   * MULTI：告诉 Redis 服务器开启一个事务。注意，只是开启，而不是执行
   * EXEC：告诉 Redis 开始执行事务
   * DISCARD：告诉 Redis 取消事务
   * WATCH：监视某一个键值对，它的作用是在事务执行之前如果监视的键值被修改，事务会被取消。
     可以利用watch实现cas乐观锁

[Redis 事务机制](https://wiki.jikexueyuan.com/project/redis/transaction-mechanism.html)

[Redis实现CAS的乐观锁](https://www.jianshu.com/p/d777eb9f27df)

### 9.27 Redis 的选举算法和流程是怎样的

Raft 采用心跳机制触发 Leader 选举。系统启动后，全部节点初始化为 Follower，term 为0。节点如果收到了RequestVote或者AppendEntries，就会保持自己的Follower身份。如果一段时间内没收到AppendEntries消息直到选举超时，说明在该节点的超时时间内还没发现Leader，Follower就会转换成Candidate，自己开始竞选Leader。一旦转化为Candidate，该节点立即开始下面几件事情：

1. 增加自己的term。
2. 启动一个新的定时器。
3. 给自己投一票。
4. 向所有其他节点发送RequestVote，并等待其他节点的回复。
   如果在这过程中收到了其他节点发送的AppendEntries，就说明已经有Leader产生，自己就转换成Follower，选举结束。
   如果在计时器超时前，节点收到多数节点的同意投票，就转换成Leader。同时向所有其他节点发送AppendEntries，告知自己成为了Leader。
   每个节点在一个term内只能投一票，采取先到先得的策略，Candidate前面说到已经投给了自己，Follower会投给第一个收到RequestVote的节点。每个Follower有一个计时器，在计时器超时时仍然没有接受到来自Leader的心跳RPC, 则自己转换为Candidate, 开始请求投票，就是上面的的竞选Leader步骤。
   如果多个Candidate发起投票，每个Candidate都没拿到多数的投票（Split Vote），那么就会等到计时器超时后重新成为Candidate，重复前面竞选Leader步骤。
   Raft协议的定时器采取随机超时时间，这是选举Leader的关键。每个节点定时器的超时时间随机设置，随机选取配置时间的1倍到2倍之间。由于随机配置，所以各个Follower同时转成Candidate的时间一般不一样，在同一个term内，先转为Candidate的节点会先发起投票，从而获得多数票。多个节点同时转换为Candidate的可能性很小。即使几个Candidate同时发起投票，在该term内有几个节点获得一样高的票数，只是这个term无法选出Leader。由于各个节点定时器的超时时间随机生成，那么最先进入下一个term的节点，将更有机会成为Leader。连续多次发生在一个term内节点获得一样高票数在理论上几率很小，实际上可以认为完全不可能发生。一般1-2个term类，Leader就会被选出来。
   Sentinel的选举流程
   Sentinel集群正常运行的时候每个节点epoch相同，当需要故障转移的时候会在集群中选出Leader执行故障转移操作。Sentinel采用了Raft协议实现了Sentinel间选举Leader的算法，不过也不完全跟论文描述的步骤一致。Sentinel集群运行过程中故障转移完成，所有Sentinel又会恢复平等。Leader仅仅是故障转移操作出现的角色。


**选举流程**

1. 某个Sentinel认定master客观下线的节点后，该Sentinel会先看看自己有没有投过票，如果自己已经投过票给其他Sentinel了，在2倍故障转移的超时时间自己就不会成为Leader。相当于它是一个Follower。
2. 如果该Sentinel还没投过票，那么它就成为Candidate。
3. 和Raft协议描述的一样，成为Candidate，Sentinel需要完成几件事情
   * 更新故障转移状态为start
   * 当前epoch加1，相当于进入一个新term，在Sentinel中epoch就是Raft协议中的term。
   * 更新自己的超时时间为当前时间随机加上一段时间，随机时间为1s内的随机毫秒数。
   * 向其他节点发送is-master-down-by-addr命令请求投票。命令会带上自己的epoch。
   * 给自己投一票，在Sentinel中，投票的方式是把自己master结构体里的leader和leader_epoch改成投给的Sentinel和它的epoch。
4. 其他Sentinel会收到Candidate的is-master-down-by-addr命令。如果Sentinel当前epoch和Candidate传给他的epoch一样，说明他已经把自己master结构体里的leader和leader_epoch改成其他Candidate，相当于把票投给了其他Candidate。投过票给别的Sentinel后，在当前epoch内自己就只能成为Follower。
5. Candidate会不断的统计自己的票数，直到他发现认同他成为Leader的票数超过一半而且超过它配置的quorum（quorum可以参考《redis sentinel设计与实现》）。Sentinel比Raft协议增加了quorum，这样一个Sentinel能否当选Leader还取决于它配置的quorum。
6. 如果在一个选举时间内，Candidate没有获得超过一半且超过它配置的quorum的票数，自己的这次选举就失败了。
7. 如果在一个epoch内，没有一个Candidate获得更多的票数。那么等待超过2倍故障转移的超时时间后，Candidate增加epoch重新投票。
8. 如果某个Candidate获得超过一半且超过它配置的quorum的票数，那么它就成为了Leader。
9. 与Raft协议不同，Leader并不会把自己成为Leader的消息发给其他Sentinel。其他Sentinel等待Leader从slave选出master后，检测到新的master正常工作后，就会去掉客观下线的标识，从而不需要进入故障转移流程。

[Raft协议实战之Redis Sentinel的选举Leader源码解析](http://weizijun.cn/2015/04/30/Raft协议实战之Redis Sentinel的选举Leader源码解析/)

### 9.28 Redis 的持久化的机制，AOF 和 RDB 的区别。

* RDB 定时快照方式(snapshot)： 定时备份，可能会丢失数据
* AOF 基于语句追加方式，只追加写操作
* AOF 持久化和 RDB 持久化的最主要区别在于，前者记录了数据的变更，而后者是保存了数据本身

### 9.29 redis 的集群怎么同步的数据的。

redis、replication、redis-migrate-tool等方式

### 9.30 elasticsearch 了解多少，说说你们公司 es 的集群架构，索引数据大小，分片有多少，以及一些调优手段。elasticsearch 的倒排索引是什么。

ElasticSearch（简称ES）是一个分布式、Restful的搜索及分析服务器，设计用于分布式计算；能够达到实时搜索，稳定，可靠，快速。和Apache Solr一样，它也是基于Lucence的索引服务器，而ElasticSearch对比Solr的优点在于：
>
1. 轻量级：安装启动方便，下载文件之后一条命令就可以启动。
2. Schema free：可以向服务器提交任意结构的JSON对象，Solr中使用schema.xml指定了索引结构。
3. 多索引文件支持：使用不同的index参数就能创建另一个索引文件，Solr中需要另行配置。
4. 分布式：Solr Cloud的配置比较复杂。



倒排索引是实现“单词-文档矩阵”的一种具体存储形式，通过倒排索引，可以根据单词快速获取包含这个单词的文档列表。倒排索引主要由两个部分组成：**单词词典** 和 **倒排文件**。

### 9.31 elasticsearch 索引数据多了怎么办，如何调优，部署。

* 使用bulk API
* 初次索引的时候，把 replica 设置为 0
* 增大 threadpool.index.queue_size
* 增大 indices.memory.index_buffer_size
* 增大 index.translog.flush_threshold_ops
* 增大 index.translog.sync_interval
* 增大 index.engine.robin.refresh_interval

[如何提高ElasticSearch 索引速度](https://www.jianshu.com/p/5eeeeb4375d4)

### 9.32 lucence 内部结构是什么

* **索引(Index)**：
  在Lucene中一个索引是放在一个文件夹中的。
  如上图，同一文件夹中的所有的文件构成一个Lucene索引。
* **段(Segment)**：
  一个索引可以包含多个段，段与段之间是独立的，添加新文档可以生成新的段，不同的段可以合并。
  如上图，具有相同前缀文件的属同一个段，图中共三个段 “_0” 和 "_1"和“2”。
  segments.gen和segments_X是段的元数据文件，也即它们保存了段的属性信息。
* **文档(Document)**：
  文档是我们建索引的基本单位，不同的文档是保存在不同的段中的，一个段可以包含多篇文档。
  新添加的文档是单独保存在一个新生成的段中，随着段的合并，不同的文档合并到同一个段中。
* **域(Field)**：
  一篇文档包含不同类型的信息，可以分开索引，比如标题，时间，正文，作者等，都可以保存在不同的域里。
  不同域的索引方式可以不同，在真正解析域的存储的时候，我们会详细解读。
* **词(Term)**：
  词是索引的最小单位，是经过词法分析和语言处理后的字符串。