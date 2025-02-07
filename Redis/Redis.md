# Redis基础

## 什么是Redis

Redis是Remote Dictionary Service，是一种基于键值对的**NoSQL**数据库

- 支持多种数据结构，如string、hash、list、set、zset、bitmaps、hyperloglog、geo等
- 所有数据存放在内存中，读写性能非常出色
- 可以将所有数据持久化到硬盘上，发生断电或机器故障时，内存中的数据不会丢失
- 还提供了键过期、发布订阅、事务、流水线、Lua 脚本等附加功能



## Redis可以用来做什么

**1. 缓存**

- 由于 Redis 的数据存储在内存中，所以读写速度非常快，远超基于磁盘存储的数据库。使用 Redis 缓存可以极大地提高应用的响应速度和吞吐量。

**2. 排行榜 / 计数器**

- Redis 的 ZSet 非常适合用来实现排行榜的功能，可以根据 score（分值）进行排序，实时展示用户的活跃度。
- 同时 Redis 的原子递增操作可以用来实现计数器功能

**3. 分布式锁**

- 可以实现分布式锁，用来控制跨多个进程的资源访问



## Redis有哪些数据类型

Redis一共有八种数据类型

![三分恶面渣逆袭：Redis基本数据类型](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-10434dc7-c7a3-4c1a-b484-de3fb37669ee.png)

**1. string**

**2. hash**

- 格式：Key => ({field1, value1}, {field2, value2})

- 什么时候使用hash类型而不是用string类型存储

  - <img src="https://cdn.tobebetterjavaer.com/stutymore/redis-20240315115713.png" alt="二哥的 Java 进阶之路" style="zoom: 50%;" />

  - 使用 hash 比使用 string 更便于进行序列化，我们可以将一整个用户对象序列化，然后作为一个 value 存储在 Redis 中，存取更加便捷。

**3. list**

- list 是一个简单的字符串列表，按照插入顺序排序。可以添加一个元素到列表的头部（左边）或者尾部（右边）。

**4. set**

- Set 是一个无序集合，元素是唯一的，不允许重复。

**5. zset**

![二哥的 Java 进阶之路](https://cdn.tobebetterjavaer.com/stutymore/redis-20240315120652.png)

- Zset 是有序集合，比 set 多了一个排序属性 score。

**6. bitmap**

**7. HyperLogLog**

**8. GEO**



## Redis为什么快

**1. 基于内存的数据存储**

- 将数据存储在内存当中，使得数据的读写操作避开了磁盘 I/O

**2. 单线程模型**

- 使用单线程模型来处理客户端的请求，这意味着在任何时刻只有一个命令在执行。这样就避免了线程切换和锁竞争带来的消耗

**3. IO多路复用**

![有盐先生：IO 多路复用](https://cdn.tobebetterjavaer.com/stutymore/redis-20240918114125.png)

- 基于 Linux 的 select/epoll 机制。该机制允许内核中同时存在多个监听套接字和已连接套接字，内核会一直监听这些套接字上的连接请求或者数据请求，一旦有请求到达，就会交给 Redis 处理，就实现了所谓的 Redis 单个线程处理多个 IO 读写的请求。

**4. 高效的数据结构**



## 什么是阻塞IO

阻塞IO就是等待数据就绪（从磁盘拷贝到用户的缓冲区）和读取数据（从用户的缓冲区）两个阶段都必须阻塞等待

![image-20250206134735377](https://github.com/user-attachments/assets/90abd8d0-2fec-4733-b2a6-24d205380eb3)




## 什么是非阻塞IO

非阻塞IO的recvfrom操作会立即返回结果而不是阻塞用户进程，第二阶段数据拷贝的时候还是阻塞的

![image-20250206134927654](https://github.com/user-attachments/assets/c42670ef-98f0-4d96-a8a5-aa9726bfe13a)




## 能说一下IO多路复用吗

无论是阻塞IO还是非阻塞IO，用户应用在第一阶段都需要调用recvfrom来获取数据，差别在于无数据时的处理方案：

- 阻塞IO阻塞等待数据
- 非阻塞IO使CPU空转

如果服务端处理客户端Socket请求时，在单线程情况下，只能一次处理每一个Socket，如果正在处理的Socket恰好未就绪，线程就会被阻塞，哪怕其它的Socket就绪了也必须等待。

**文件描述符FD**：是一个从0开始递增的无符号整数，用来关联Linux中的一个文件。在Linux中，一切皆文件，例如常规文件、视频、硬件设备等 ，当然也包括网络套接字Socket

**IO多路复用**：利用单个线程来同时监听多个FD，并在某个FD可读、可写时得到通知，从而避免无效的等待，充分利用CPU资源





![image-20250206141456276](https://github.com/user-attachments/assets/0d134340-579f-46c3-bb1e-5d6324a8cd34)




## select、poll和epoll有什么区别

IO 多路复用是一种高效管理多个 IO 事件的技术，通过单线程监控多个文件描述符（fd），实现高并发的 IO 操作。



select和poll只会通知用户进程有FD就绪，但不确定具体是哪个FD，需要用户进程逐个遍历FD来确认

epoll则会在通知用户进程FD就绪的同时，把已就绪的FD写入用户空间



**select**

![image-20250206155415228](https://github.com/user-attachments/assets/c8fda273-b0fe-4d85-b127-22e654d86674)


- 需要将整个fd_set从用户空间拷贝到内核空间，select结束后还要再次拷贝回用户空间
- select无法直接得知哪个fd就绪，需要遍历整个fd_set
- fd_set监听的fd数量不能超过1024

**poll**

![image-20250206160158647](https://github.com/user-attachments/assets/5106b88e-6741-4471-9598-498185ec8120)


- poll模式对select模式做了简单改进，但性能提升不影响

- 使用动态数组管理fd，监听数量无上限（唯一的改进）
- 监听的FD越多，每次遍历消耗的时间也越久，性能反而会下降

**epoll**

![image-20250206160857431](https://github.com/user-attachments/assets/485c7417-b707-4bca-8b89-57a0e8993c63)



- 内核空间只拷贝就绪的FD到用户空间的指定位置
- 用户空间只需遍历就绪的fd，不用遍历所有FD

这样，整个过程只在进行 select、poll、epoll 这些调用的时候才会阻塞，收发客户消息是不会阻塞的，整个进程或者线程就被充分利用起来，这就是事件驱动，所谓的 reactor 模式。



## Redis6.0使用多线程是怎么回事

单线程模型意味着 Redis 在大量 IO 请求时，无法充分利用多核 CPU 的优势。

Redis 6.0以前，网络 I/O 由主线程处理 → 主线程既要处理命令，又要等待 socket 读写，性能受限

![三分恶面渣逆袭：Redis6.0多线程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-b7b24e25-d2dc-4457-994f-95bdb3674b8e.png)

- Redis主线程监听epoll事件
- Redis IO线程池（多个Redis IO线程）负责读取socket数据，并放到主线程的队列中

- Redis主线程（仍然是单线程，保证原子性）解析请求，并执行命令，将执行结果写回队列
- 响应客户端Redis IO 线程负责将队列中的结果写入（write）socket



## SADD操作的时间复杂度是多少

- Redis 的 Set 底层使用哈希表（dict）实现。

- 插入元素到哈希表的时间复杂度是 O(1)，因为哈希表直接计算哈希值，然后插入到对应的 bucket 中，不需要遍历整个集合。

- 多个元素插入需要 O(N)，因为需要执行 `N` 次 O(1) 的插入操作。



## 单线程Redis的QPS是多少

Redis 的 QPS（Queries Per Second，每秒查询率）受多种因素影响，包括硬件配置（如 CPU、内存、网络带宽）、数据模型、命令类型、网络延迟等。

根据官方的基准测试，一个普通服务器的 Redis 实例通常可以达到**每秒数万到几十万的 QPS**。

可以通过 `redis-benchmark` 命令进行基准测试：



```shell
redis-benchmark -h 127.0.0.1 -p 6379 -c 50 -n 10000
```

- `-h`：指定 Redis 服务器的地址，默认是 127.0.0.1。
- `-p`：指定 Redis 服务器的端口，默认是 6379。
- `-c`：并发连接数，即同时有多少个客户端在进行测试。
- `-n`：请求总数，即测试过程中总共要执行多少个请求。



# 持久化

## 什么是RDB

RDB全程Redis Database Backup file（Redis数据备份文件），也被叫做Redis数据快照。

简单来说就是把内存中的所有数据都记录到磁盘中，当Redis实例故障重启后，从磁盘读取快照文件，恢复数据。

![image-20250206223920746](https://github.com/user-attachments/assets/92b13b81-9a7b-49f6-8ee5-14f4e29aab62)

- Redis主动停机时会自动执行一次RDB

![image-20250206224344711](https://github.com/user-attachments/assets/270c961c-d11f-4dc8-b944-5f3ebd8220f5)


- Redis内部有触发RDB的机制可以在redis.conf文件中找到，格式如下：

  ![image-20250206224550325](https://github.com/user-attachments/assets/c236e276-de27-447c-9108-b6aa657e26ba)


- RDB的其它配置也可以在redis.conf文件中设置：

  ![image-20250206224757743](https://github.com/user-attachments/assets/40729d88-095d-4d98-b08e-ad0aa43b92af)




## RDB在什么场景下会自动触发

1. redis.conf文件中的配置

2. 当Redis服务器通过SHUTDOWN命令正常关闭时，如果没有禁用RDB持久化，Redis会自动执行一次RDB持久化，确保数据在下次启动时能够恢复
3. 在Redis复制场景中，当一个 Redis 实例被配置为从节点并且与主节点建立连接时，它可能会根据配置接收主节点的 RDB 文件来初始化数据集。这个过程中，主节点会在后台自动触发 RDB 持久化，然后将生成的 RDB 文件发送给从节点。



 ## bgsave原理是什么

bgsave开始时会fork主进程得到子进程，子进程共享主进程的内存数据，完成fork后读取内存数据并写入RDB文件

![image-20250206230114561](https://github.com/user-attachments/assets/ba28013a-5d2c-4699-82e1-02444eef8435)


- 主进程将自己的页表（虚拟内存到物理内存的映射）拷贝给子进程

- 子进程异步写入RDB文件的过程中主进程有可能会修改数据
  - 需要写入时，必须先对原始数据进行拷贝，在拷贝上写



## 什么是AOF

![三分恶面渣逆袭：AOF工作流程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-a9fb6202-b1a1-484d-a4fa-fef519090b44.png)

AOF全称为Append Only File（追加文件），Redis处理的每一个写命令都会记录在AOF文件，可以看作是命令日志文件。

![image-20250206230522729](https://github.com/user-attachments/assets/e5da5b16-b19a-4269-9e63-af7a31bec8e6)


- AOF默认是关闭的，需要修改redis.conf配置文件来开启AOF

  ![image-20250206230551874](https://github.com/user-attachments/assets/3e2b0e7b-6684-4718-ba03-6b0856994806)


- AOF的命令记录的频率也可以通过redis.conf文件来配置

  ![image-20250206230638198](https://github.com/user-attachments/assets/fc22611b-ca2f-425e-8b98-8692d3b062c0)


![image-20250206230710896](https://github.com/user-attachments/assets/5936aadf-a4a9-4b3a-b42b-baac2afcb19b)




- 因为是记录命令，AOF文件会比RDB文件大得多，而且AOF会记录对同一个key的多次写操作，但只有最后一次写操作才有意义。通过执行`bgrewriteaof`命令，可以让AOF文件执行重写功能，用最少的命令达到相同效果。

​	![image-20250206231122418](https://github.com/user-attachments/assets/02ba8d7a-7244-46cd-b129-f2cdb9e34d21)


- Redis也会在触发阈值时自动去重写AOF文件，阈值也可以在redis.conf中配置

  ![image-20250206231256847](https://github.com/user-attachments/assets/ee13878e-a2ad-43f5-89f2-4684be598ae0)




## AOF重写期间命令可能会写入两次，会造成什么影响

AOF 重写期间，Redis 会将新的写命令**同时写入旧的 AOF 文件和重写缓冲区**。这样会带来额外的磁盘开销。

但可以防止在 AOF 重写尚未完成时，Redis 发生崩溃，导致数据丢失。即使重写失败，旧的 AOF 文件仍然是完整的。



当重写完成后，会通过原子操作将新的 AOF 文件替换旧的 AOF 文件。



## RDB和AOF各自有什么优缺点

RDB非常适合备份数据，比如在夜间进行备份，然后将 RDB 文件复制到远程服务器。但可能会丢失最后一次持久化后的数据。



AOF最大优点是灵活，实时性好，可以设置不同的 fsync 策略，如每秒同步一次，每次写入命令就同步，或者完全由操作系统来决定何时同步。但 AOF 文件往往比较大，恢复速度慢，因为它记录了每个写操作。



## RDB和AOF如何选择

![image-20250206233418655](https://github.com/user-attachments/assets/b86a78ea-d65d-49cc-847e-5312ea993167)




## Redis的数据恢复

![三分恶面渣逆袭：Redis启动加载数据](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-f9aab5e9-a875-4316-9ec9-0c5650afe5c1.png)

1. AOF 开启且存在 AOF 文件时，优先加载 AOF 文件。
2. AOF 关闭或者 AOF 文件不存在时，加载 RDB 文件。



## Redis4.0的混合持久化了解吗

在 Redis 4.0 版本中，混合持久化模式会在 AOF 重写的时候同时生成一份 RDB 快照，然后将这份快照作为 AOF 文件的一部分，最后再附加新的写入命令。

![三分恶面渣逆袭：混合持久化](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-19c531e5-da95-495a-a4c4-d63a0b8bba95.png)

这样，当需要恢复数据时，Redis 先加载 RDB 文件来恢复到快照时刻的状态，然后应用 RDB 之后记录的 AOF 命令来恢复之后的数据更改，既快又可靠。



# 高可用

## redis的部署方式有哪些

除了单机部署外，Redis还可以通过主从复制、哨兵模式和集群模式来实现高可用。

**主从复制**

- 允许一个 Redis 服务器（主节点）将数据复制到一个或多个 Redis 服务器（从节点）。这种方式可以实现读写分离，适合读多写少的场景

**哨兵模式**

- 用于监控主节点和从节点的状态，实现自动故障转移。
- 如果主节点发生故障，哨兵可以自动将一个从节点升级为新的主节点

**集群模式**

- Redis集群通过分片的方式存储数据，每个节点存储数据的一部分，用户请求可以并行处理。

- 集群模式支持自动分区、故障转移，并且可以在不停机的情况下进行节点增加或删除。



## 主从复制了解吗

![三分恶面渣逆袭：Redis主从复制简图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-60497f1e-8afb-44b3-bb7a-d4c29e5ac484.png)

主节点负责处理所有的写操作，并将这些操作异步复制到从节点。从节点主要用于读取操作，以分担主节点的压力和提高读性能。



**作用：**

1. **数据冗余：** 

   主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2. **故障恢复：** 

   - 如果主节点挂掉了，可以将一个从节点提升为主节点，从而实现故障的快速恢复。
   - 如果从节点挂掉了，主节点不受影响，但应该尽快修复并重启挂掉的从节点，使其重新加入集群并从主节点同步数据。

3. **负载均衡：** 在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务 *（即写 Redis 时连接主节点，读 Redis 时连接从节点）*，分担服务器负载。



## Redis数据同步原理

主从第一次同步是**全量同步**:

![image-20250207110022142](https://github.com/user-attachments/assets/36f25af3-98bb-4268-8b40-95fae3e6d6b0)


但如果slave重启后同步，则执行**增量同步**：

![image-20250207110509172](https://github.com/user-attachments/assets/42d90177-278d-4d15-a9ca-4244d873a4be)


repl_baklog大小有上限，写满后会覆盖最早的数据。如果slave断开时间过久，导致尚未备份的数据被覆盖，则无法基于log做增量同步，只能再次全量同步



## master如何判断slave是不是第一次来同步数据

**Replication Id**：简称repid，是数据集的标记，id一致则说明是同一数据集。每一个master都有唯一的replid，slave则会继承master节点的replid

**offset**：偏移量，随着记录在repl_baklog中的数据增多而增大，slave完成同步时也会记录当前同步的offset。如果slave的offset小于master的offset，说明slave数据落后于master，需要更新。



master通过replid是否一致来判断是否是第一次同步



## 主从复制出现数据不一致怎么办

Redis采用异步复制，主库的写入不会立即同步到从库，而是异步进行。

**解决方法**：

![极客时间：Redis 核心技术与实战](https://cdn.tobebetterjavaer.com/stutymore/redis-20240709135618.png)

- 通过 Redis 的 `INFO replication` 命令监控主从节点的复制进度，及时发现和处理复制延迟。
- 获取主节点的 `master_repl_offset` 和从节点的 `slave_repl_offset`，计算两者的差值。如果差值超过预设的阈值，采取措施（如停止从节点的数据读取）以减少读到不一致数据的情况。



## Redis主从有几种常见的拓扑结构

**1. 一主一从**

![一主一从结构](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-5d91a67c-dbff-4a8d-bf9d-1fe7602d5a27.png)

**2. 一主多从**

![一主多从结构](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-71074254-699a-480b-bbb0-c68f364a380b.png)

![树状主从结构](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-dff14203-5e01-4d1b-a775-10ee444ada54.png)



## 脑裂问题了解吗

Redis 的脑裂问题是指在主从模式或集群模式下，由于网络分区或节点故障，可能导致系统中出现多个主节点，从而引发数据不一致、数据丢失等问题。

可以通过 Sentinel 模式和 Cluster 模式中的投票机制和强制下线机制来解决。



## 主从复制如何优化

- 可以在master启用无磁盘复制，避免全量同步时的磁盘IO

- Redis单节点上的内存占用不要太大

- 适当提高repl_baklog的大小

- 限制一个master上的slave节点数量，如果实在太多slave，可以采用主-从-从链式结构减轻master压力

  ![image-20250207111033638](https://github.com/user-attachments/assets/f156dbd1-96af-47e5-8861-fc467ca68b97)




## Redis的哨兵模式是什么

![image-20250207111353918](https://github.com/user-attachments/assets/4b7db478-152d-447f-8771-7de8068d586b)


Sentinel基于心跳机制监测服务状态，每隔1秒向集群的每个实例发送ping命令

- 主观下线：某sentinel节点发现某实例未在规定时间响应
- 客观下线：若超过指定数量的sentinel都认为该实例主观下线



## 哨兵模式如何实现故障转移

![image-20250207112054081](https://github.com/user-attachments/assets/d1e1f300-e35b-40e3-ac68-7b9ecbff7c3a)




## 领导者Sentinel节点选举了解吗？

Redis 使用 Raft 算法实现领导者选举的：当主节点挂掉后，新的主节点是由剩余的从节点发起选举后晋升的。

![二哥的 Java 进阶之路：领导者Sentinel节点选举](https://cdn.tobebetterjavaer.com/stutymore/redis-20240819112712.png)

1. 每个在线的 Sentinel 节点都有资格成为领导者，当它确认主节点下线时候，会向其他哨兵节点发送命令，表明希望由自己来执行主从切换，并让所有其他哨兵进行投票。

   这个投票过程称为“Leader 选举”。候选者会给自己先投 1 票，然后向其他 Sentinel 节点发送投票的请求。

2. 收到请求的 Sentinel 节点会进行判断，如果候选者的日志与自己的日志一样新，任期号也小于自己，且之前没有投票过，就会同意投票，回复 Y。否则回复 N。

3. 候选者收到投票后会统计支持自己的得票数，如果候选者获得了集群中超过半数节点的投票支持（即多数原则），它将成为新的主节点。

   新的主节点在确立后，会向其他从节点发送心跳信号，告诉它们自己已经成为主节点，并将其他节点的状态重置为从节点。

4. 如果多个节点同时成为候选者，并且都有可能获得足够的票数，这种情况下可能会出现选票分裂。也就是没有候选者获得超过半数的选票，那么这次选举就会失败，所有候选者都会再次发起选举。

为了防止无限制的选举失败，每个节点都会有一个选举超时时间，且是随机的。



## Redis分片集群是什么

![Redis 集群示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-5cbc6009-251e-4d5b-8f22-8d543938eccb.png)

- 集群中有多个master，每个master保存不同数据
- 每个master都可以有多个slave节点
- master之间通过ping检测彼此健康状态
- 客户端请求可以访问集群任意节点，最终都会被转发到正确节点

**散列插槽**

![三分恶面渣逆袭：槽](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-e0ed9d62-3406-40db-8b01-c931f1020612.png)

CRC16 是一种哈希算法，它可以将任意长度的输入数据映射为一个 16 位的哈希值。

- Redis会把每一个master节点映射到0~16383共16384个插槽上

![image-20250207114839491](https://github.com/user-attachments/assets/b64a0084-44de-4bb1-9251-22b7db57324b)




## Redis的一致性哈希是什么

![三分恶面渣逆袭：一致性哈希分区](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-89bd1c1c-251c-4f53-bba3-fe945b2ae9e2.png)

- 将哈希值空间组织成一个环，数据项和节点都映射到这个环上。数据项由其哈希值直接映射到环上，然后顺时针分配到遇到的第一个节点。从而来减少节点变动时数据迁移的量。

存在的问题：

- 节点在圆环上分布不平均，会造成部分缓存节点的压力较大
- 当某个节点故障时，这个节点所要承担的所有访问都会被顺移到另一个节点上，会对后面这个节点造成压力。



## 什么是Redis的虚拟槽分区

![三分恶面渣逆袭：虚拟槽分配](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-e0ed9d62-3406-40db-8b01-c931f1020612.png)

- 在虚拟槽（也叫哈希槽）分区中，槽位的数量是固定的（例如 Redis Cluster 有 16384 个槽），每个键通过哈希算法（比如 CRC16）映射到这些槽上，每个集群节点负责管理一定范围内的槽。

- 这种分区可以灵活地将槽（以及槽中的数据）从一个节点迁移到另一个节点，从而实现平滑扩容和缩容；数据分布也更加均匀，Redis Cluster 采用的正是这种分区方式。

  

能保证扩容后，大部分数据停留在扩容前的位置，只有少部分数据需要迁移到新的槽上。



## Redis集群如何保证扩容过程中数据正常访问插入

![三分恶面渣逆袭：扩容实例](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-1d24bb63-2b05-4db9-bd6b-983f16a4830e.png)

当需要扩容时，新的节点被添加到集群中，集群会自动执行数据迁移，以重新分布哈希槽到新的节点。数据迁移的过程可以确保在扩容期间数据的正常访问和插入。

当数据正在迁移时，客户端请求可能被路由到原有节点或新节点。Redis Cluster 会根据哈希槽的映射关系判断请求应该被路由到哪个节点，并在必要时进行重定向。

如果请求被路由到正在迁移数据的哈希槽，Redis Cluster 会返回一个 MOVED 响应，指示客户端重新路由请求到正确的目标节点。这种机制也就保证了数据迁移过程中的最终一致性。



## 缓存设计

## 缓存穿透、缓存雪崩、缓存击穿了解吗

**缓存穿透**

查询不存在的数据，由于缓存没有命中，请求每次都会穿过缓存去查询数据库。如果这种查询非常频繁，会给数据库造成很大压力

![三分恶面渣逆袭：缓存穿透](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-029951e6-8b99-4364-a570-010853deb594.png)

解决方法：

- 缓存空值/默认值

  ![三分恶面渣逆袭：缓存空值/默认值](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-288af5a2-ae5a-427a-95e9-b4a658b01386.png)

- 布隆过滤器

  ![三分恶面渣逆袭：布隆过滤器](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-0e18ea40-a2e5-4fa6-989e-e771f6e4b0fc.png)

**缓存雪崩**

在某一个时间点，由于大量的缓存数据同时过期或缓存服务器突然宕机了，导致所有的请求都落到了数据库上（比如 MySQL），从而对数据库造成巨大压力，甚至导致数据库崩溃的现象。

![三分恶面渣逆袭：缓存雪崩](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-1464fe22-c463-4850-8989-b899510cb10e.png)

解决方法：

- 给不同的Key的TTL添加随机值
- 利用Redis集群提高服务的可用性
- 给缓存业务添加降级限流策略，防止在缓存失效时数据库被打垮



**缓存击穿**

缓存击穿是指某一个或少数几个数据被高频访问，当这些数据在缓存中过期的那一刻，大量请求就会直接到达数据库，导致数据库瞬间压力过大。

![三分恶面渣逆袭：缓存击穿](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/redis-86579ee6-9dae-4274-a5cc-af6812f48da4.png)

解决方法：

- 互斥锁：只有一个请求会去数据库查询，写入缓存，后面的请求直接从缓存中获取数据（强一致）

- 逻辑过期：不设置过期时间

  ![image-20250207125127446](https://github.com/user-attachments/assets/5deef1bd-d774-4c14-918c-32cec4b48941)






# 参考资料

黑马程序员

- P40 - P43（从穿透看到击穿）
- P97 - P100（持久化）
- P155 - P159（数据结构）
