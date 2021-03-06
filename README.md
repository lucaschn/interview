# 面试经历中被问过的题目

（整理时间5月13日）
1. 工作一段时间后，为了夯实基础做过哪些努力？
2. lumen和workerman了解吗？介绍下workerman的原理
3. 讲一下websocket的协议，浏览器是怎么识别websocket协议的，以及websocket的协议版本区别？
4. 怎么压缩数据的，讲一下gzip压缩是怎么压缩处理的？gzip压缩有几个级别，6级别会压缩到什么程度？
5. 业务开发中websocket的连接最高峰存在多少个连接？一千个连接会占用多少内存？
6. 多少数据量时会采用分布式的部署？
7. 多台gateway是怎么负载分流长连接的？当APP发起websocket连接时是怎样平均分配到每台gateway服务器的？
8. gateway进程挂了怎么处理？
9. websocket连接中如何保证成功率，即如何判断客户端是否收到消息、怎么处理丢包的问题？
【建立应答与重发机制，待补充具体处理方式】
10. 多台websocket的用户如何共享数据，例如同一聊天组的user1的websocket保存在服务器1中，user2的websocket保存在服务器2中。两台服务器的用户如何正常通讯？
【这和http session不一样，StandardWebSocketSession一是无法序列化，二是它是在一台服务器保持TCP连接，另一台服务器拿到数据也不能通信。所以那些说存到共享的容器（memcache/redis）中进行共享session的都是不行的。两个办法，一个是用redis作发布、订阅，所有socket都订阅一个消息。第二种方法是用消息队列，jgroups作集群组播通信，互相通知。类似的还有MQ等等。】
11. 介绍下微服务架构原理？
12. 介绍下平衡二叉树？
13. 水平分表是用什么方式处理的？介绍下mysql分库分表策略，如何解决增表、减表问题？
【按时间分表、按区间范围分表、hash分表】
14. 说下mysql的优化方向？索引是怎么优化的？索引的原理是怎样的？
15. 说下INNODB和MYISAM的区别？innodb是什么数据结构？说下b+树和b树的区别？innodb的主键索引和非主键索引的区别？

--------------------

（整理时间5月22日）
1. 在上家公司主要做了哪些工作呢？有哪些是你觉得比较有意思且比较成功的项目？
2. 对区块链的知识本身有了解吗？最近有在学什么其他技术吗？
3. 业务开发中数据量大吗？是否遇到过mysql慢速的问题？是因为什么引起的？解决思路是怎样的？
4. 如何实现全文检索功能？如果数据库是mysql的话怎么处理？介绍下ES的特点？
【MYSQL的FULLTEXT索引、对内容进行分片、分词查找】
5. 关系型数据库和非关系型数据库的区别是什么？有了解哪些非关系型数据库吗？
6. redis有哪几种数据类型？每个数据类型的时间复杂度分别是什么？使用场景分别是什么？介绍下redis发布订阅机制原理？说说对新的数据类型streams的了解？
7. 介绍下tcp三次握手的过程？哪个阶段开始传输数据？如果客户端在握手过程中失败了，服务器会怎么处理？
8. 在业务开发中，workerman与APP端进行数据交互时有做身份验证吗？怎么处理的？
9. php中的类是单继承的，那要有多个类继承有什么方案呢？
【php trait的原理】
10. php有哪几个常用的魔术方法？介绍下构造函数和析构函数的作用，以及分别在什么时候会调用？
11. php的public、protected、private 三种访问控制模式有什么区别？（主要考察PHP类的封装性、继承性、多态性）
12. php有哪几种设计模式？介绍下你熟悉的设计模式？简单写几种设计模式看看？
13. 介绍下php中的引用赋值？ $a=1; $b=&$a; unset($a)后$b是什么，为什么？unset($b)后$a是什么，为什么？
【都等于1。在php中，引用赋值不同于指针的感念，他只是将另一个变量名指向了某个内存地址。此题中:$b = &$a;只是将$b这个名字也指向了$a变量所指向的内存地址。unset时只是释放了这个名字的指向，并没有释放内存中的值。另一方面讲unset($a),其实也并未真正立刻释放内存中的值，也只是释放了这个名字的指向而已，该函数只有在变量值所占空间超过256字节长的时候才会释放内存，并且只有当指向该值的所有变量（比如有引用变量指向该值）都被销毁后，地址才会被释放。】
14. 遇到mysql慢速时有什么排查方向呢？
15. mysql的存储引擎有哪几种？分别适用于什么场景？
16. 介绍下mysql事务的四个隔离级别，以及各级别之间的区别？
17. innodb引擎什么情况下会产生行锁，什么情况下会变成表锁？导致索引失效的原因？
【or 语句，like 前缀，索引字段是字符串但查询条件里没用使用引号扩起，联合索引未遵循最左原则没使用第一个索引字段而使用其他索引字段】
18. 介绍下b+树？
19. 更新数据时，是先删除缓存再更新DB，还是先更新DB再删除缓存？如果缓存和数据库一致性时有什么解决方案呢？
【先更新DB再删除缓存可以降低读到脏数据的概率。方案一、采用延时双删策略+缓存过期设置，整体思路：在写库前后都进行redis.del(key)操作，并且设定合理的超时时间，确保读请求结束，写请求可以删除读请求造成的缓存脏数据；所有的写操作以数据库为准，只要到达缓存过期时间，则后面的读请求自然会从数据库中读取新值然后回填缓存，可以保证最终数据一致性。方案二、异步更新缓存(基于订阅binlog的同步机制)，整体思路：MySQL binlog增量订阅消费+消息队列+增量数据更新到redis，一旦MySQL中产生了新的写入、更新、删除等操作，就可以异步把binlog相关的消息推送至Redis，Redis再根据binlog中的记录，对Redis进行更新。（消息推送工具：canal、kafka、rabbitMQ等）】
20. redis是怎么解决键冲突的？
21. redis如何保证系统宕机数据不会丢失？【数据持久化】，介绍一些redis的持久化机制有哪几种？各自的区别是什么？
22. 介绍下常用的redis常用集群方案？
23. 网站访问很慢从哪些方向去排查？当发现服务器负载很高应该如何排查处理？
24. 介绍下完全二叉树、平衡二叉树、二叉查找树？
25. 求一万个数求前十个最大的数？
【top K的算法问题，分组】
26. laravel用过哪些中间件？介绍下懒加载原理？介绍下laravel的服务容器概念？
