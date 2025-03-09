# Linux常用命令

## Linux有哪些常用的命令

1. `top`用来查看系统资源

   ```bash
   top - 14:30:02 up 2 days,  3:14,  1 user,  load average: 0.31, 0.45, 0.50
   Tasks: 158 total,   1 running, 157 sleeping,   0 stopped,   0 zombie
   %Cpu(s):  3.1 us,  0.5 sy,  0.0 ni, 96.4 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem :  8045840 total,  3220644 free,  2173620 used,  2651576 buff/cache
   KiB Swap:  2097148 total,  2097148 free,        0 used.  5123452 avail Mem 
   
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    2345 root      20   0  475344  30320  19804 S   2.5  0.4   3:21.56 java
    1987 user1     20   0  131452   9284   6548 S   1.2  0.1   1:12.34 python
    1123 user2     20   0   98768   4212   3056 S   0.5  0.1   0:45.89 sshd
   
   ```

- `top -H -p <PID>`：显示进程PID的所有线程

2. `ps -ef | grep java`查找所有正在运行的进程中，与java相关的进程

- -e：显示所有进程（包括其他用户的进程）
- -f：以完整格式显示进程信息
- |：是管道，用于把一个命令的输出传递给另一个命令作为输入。
- grep：负责筛选出`ps -ef`的输出中包含”java“关键字的行

3. `netstat`查看网络连接

   ```bash
   Active Internet connections (servers and established)
   Proto Recv-Q Send-Q Local Address          Foreign Address        State       PID/Program name  
   tcp        0      0 192.168.1.100:22       192.168.1.1:54321      ESTABLISHED 1234/sshd        
   tcp        0      0 192.168.1.100:80       0.0.0.0:*              LISTEN      4567/nginx       
   tcp6       0      0 :::3306                :::*                   LISTEN      7890/mysqld      
   
   ```

4. `ping`测试网络连通性

   ```bash
   find [搜索目录] [搜索条件] [执行操作]
   ```

   [搜索目录]：要搜索的目录（如 `/`, `/home`, `.`）

   [搜索条件]：查找文件的规则（如按名称、类型、大小、修改时间等）

   [执行操作]：找到文件后执行的操作（如删除、复制、打印）

5. `find`查找文件

6. `chmod`修改文件权限

7. `kill`终止进程

8. `df`查看磁盘空间

   ```bash
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sda1       50G   20G   30G  40%  /
   tmpfs          2.0G   64M  1.9G   4%  /dev/shm
   /dev/sdb1      100G   60G   40G  60%  /mnt/data
   ```

9. `mkdir`创建目录、`rm`删除文件、`cp`复制文件、`mv`移动文件

10. `zip`压缩文件、`unzip`解压文件



## 文件操作的命令有哪些

`ls`：列出目录内容。`ls -l`显示详细信息，`ls -a`显示隐藏文件。

`cd`：更改当前目录。`cd ..`回到上级目录，`cd ~`回到用户的主目录。

`pwd`：显示当前工作目录的完整路径。

`cp`：复制文件或目录。`cp source_file target_file`复制文件，`cp -r source_directory target_directory`复制目录。

`mv`：移动或重命名文件或目录。

`rm`：删除文件或目录。`rm -r`递归删除目录及其内容。

`mkdir`：创建新目录。

`cat`：查看文件内容。`cat file1 file2`合并文件内容显示。



## ls和ll命令的区别

`ls`:

```bash
file1.txt  file2.txt  dir1  dir2
```

- 仅显示文件和目录的名称，不显示详细信息。

`ll`：

```bash
drwxr-xr-x  2 user user 4096 Mar  9 10:30 dir1
-rw-r--r--  1 user user 1024 Mar  9 10:31 file1.txt
-rwxr-xr-x  1 user user 2048 Mar  9 10:32 script.sh
```

- 权限、所有者、大小、时间等



## Windows如何创建一个空文件

**1. 使用 `echo` 命令创建空文件**

```cmd
echo. > filename.txt
```

**2. GUI**



## 如何查看系统的日志文件

在 Linux 中，可以通过 `cat`、`more`、`less`、`tail`、`head` 等命令查看系统日志文件。

也可以直接通过 `vim` 打开日志文件，然后按照关键字去搜查对应的日志信息。

或者通过grep搜索关键字

```bash
grep "error" /var/log/syslog
```



## 常见的系统日志文件有哪些

Linux 系统日志文件通常存放在 **`/var/log/`** 目录下，例如：

```bash
/var/log/syslog      # 系统日志（Ubuntu/Debian）
/var/log/messages    # 系统日志（CentOS/RHEL）
/var/log/auth.log    # 认证日志（登录、SSH）
/var/log/kern.log    # 内核日志
/var/log/dmesg       # 内核启动日志
/var/log/nginx/      # Nginx 访问日志和错误日志
/var/log/apache2/    # Apache 日志
```



## 系统管理的命令有哪些

`ps`：显示当前运行的进程。`ps aux`显示所有进程。

```bash
  PID TTY          TIME CMD
 1234 pts/1    00:00:00 bash
 5678 pts/1    00:00:00 ps
```

`top`：实时显示进程动态。

`kill`：终止进程。`kill -9 PID`强制终止。

`df`：显示磁盘空间使用情况。`df -h`以易读格式显示。

`du`：显示目录或文件的磁盘使用情况。

`free`：显示内存和交换空间的使用情况。

```bash
               total        used        free      shared  buff/cache   available
Mem:         7940464      436068     7365736        3552      138660     7302396
Swap:        2097152           0     2097152
```

`chmod`：更改文件或目录的权限。

`chown`：更改文件或目录的所有者和所属组



## 如何查看Linux内存使用情况

可以使用 watch 配合 free 命令实时监控内存使用情况。如 `watch -n 1 free -m`每秒刷新一次内存使用情况。

![二哥的 Java 进阶之路：free](https://cdn.tobebetterjavaer.com/stutymore/linux-20241223163021.png)



## 如何查看系统负载

top 命令是实时查看系统性能的常用工具，系统负载信息通常显示在 top 命令输出的顶部。它还显示了系统运行的进程、内存使用情况等



**Load Average 是什么**

load average 是一个反映系统负载的指标，表示在一段时间内系统正在处理的平均进程数量。通常，它包含三个数值，分别对应过去 1 分钟、5 分钟和 15 分钟的平均负载。

比如说上图中出现的 `load average: 1.80, 1.74, 1.83` 表示：

- 1.80：表示过去 1 分钟内，系统平均有 1.80 个进程在等待处理（包括 CPU 正在处理和等待被调度的进程）。
- 1.74：表示过去 5 分钟内的平均负载。
- 1.83：表示过去 15 分钟内的平均负载。

如果 load average 大于 CPU 核心数，表示系统的进程比 CPU 能处理的多，系统可能处于过载状态。



## 讲一下chmod的参数

chmod 命令在 Linux 中用来改变文件或目录的访问权限。这个命令的使用可以基于符号表示法（也称为文本方法）或者八进制数表示法。

Linux 中的权限可以应用于三种类别的用户：

- 文件所有者（u）
- 与文件所有者同组的用户（g）
- 其他用户（o）

像 `chmod 777 file` 赋予文件所有权限，就属于八进制数表示法。`7=4+2+1`，分别代表读、写、执行权限。

`chmod 755 file.sh` 等同于 `rwxr-xr-x`

```bash
7 = rwx(111)  (所有者)
5 = r-x(101)  (组)
5 = r-x(101)  (其他用户)
```



## kill -9中的 9 是什么意思？

`kill -9 PID` 是一种强制终止进程的方式，其中的 9 表示信号编号，代表 SIGKILL 信号。

- 无法被进程拦截，进程没有机会做任何清理操作。

- 立即终止，适用于僵尸进程或无法正常退出的进程。

- 可能导致数据丢失（进程无法保存未完成的操作）。



## 网络管理的命令有哪些

- `ping`：检查与远程服务器的连接。
- `wget`：从网络上下载文件。
- `ifconfig`：显示网络接口的配置信息。
- `netstat`：显示网络连接、路由表和网络接口信息。



## Linux系统的8080端口有多少个TCP连接，怎么看？

可以通过 netstat 命令查看，如`netstat -an | grep ':8080' | grep 'tcp' | wc -l`。

![二哥的 Java 进阶之路：netstat 命令查看 8080 端口](https://cdn.tobebetterjavaer.com/stutymore/linux-20241223161926.png)

- `-a`：显示所有网络连接和监听端口。
- `-n`：以数字形式显示地址和端口。
- `grep ':8080'`：过滤出 8080 端口的连接。
- `grep 'tcp'`：仅显示 TCP 连接。
- `wc -l`：统计匹配到的行数，即连接数



## 压缩和解压命令有哪些

`tar`：打包或解包`.tar`文件

- tar cvf archive.tar files打包
  - c：Create（创建新归档文件）
  - v：Verbose（显示详细过程）
  - f：File（指定归档文件名）

- tar xvf archive.tar解包

  - x：Extract（解包归档文件）

    

`gzip` / `gunzip`：压缩或解压`.gz`文件。



`zip` / `unzip`：压缩或解压`.zip`文件。



![image-20250309123854422](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250309123854422.png)

## Linux下查找一个文件怎么做

在 Linux 环境下查找文件，有多种命令和方法可以使用。find 命令是最常用的文件查找工具之一，它可以在指定目录下递归查找符合条件的文件和目录。

例如：在当前目录及其子目录中查找名为 "example.txt" 的文件

```bash
find . -name "example.txt"
```

例如：查找 `/home` 目录中所有 `.txt` 结尾的文件：

```bash
find /home -name "*.txt"
```

例如：查找 `/var/log` 目录中修改时间在 7 天以前的 `.log` 文件

```bash
find /var/log -name "*.log" -mtime +7
```



# Linux系统管理

## 用户和用户组有什么区别

在 Linux 中，用户和用户组是系统权限管理的核心概念。

每个用户在 Linux 中都有一个独立的账户，用于标识该用户并控制其对系统资源的访问。用户包括普通用户和超级用户（root）。普通用户的权限有限，只能访问和修改自己拥有的文件和目录，而超级用户拥有系统的最高权限，能够执行任何操作。

每个用户在系统中都有一个唯一的用户 ID（UID），以及一个关联的用户名（login name）。

用户组是用户的集合，用于简化权限管理。每个用户可以属于一个或多个用户组，而每个用户组都有一个唯一的组 ID（GID）。通过将用户分配到不同的组，系统可以更方便地管理对文件和目录的访问权限。

一个文件或目录可以由一个用户和一个用户组拥有，系统根据文件或目录的所有者和所属组来确定其他用户对它的访问权限。

可以使用 groupadd 命令来创建新的用户组。例如：

```bash
sudo groupadd developers
```

可以使用 useradd 命令来创建新的用户。创建用户时可以指定该用户的默认用户组、主目录等。例如，创建一个名为 johndoe 的用户，并将其添加到 developers 组：

```bash
sudo useradd -m -g developers johndoe
```

- `-m`：表示创建用户的同时创建用户的主目录（通常在`/home/username`）。
- `-g`：指定用户的初始用户组。



## 如何用linux命令去查找某个qps

如果服务通过网络提供访问，可以使用 netstat 或 ss 命令统计特定端口的连接数，并结合 watch 命令来监控实时的连接速率。

例如，统计 HTTPS 服务（通常运行在端口 443）每秒的请求数：

- `netstat -an`：显示所有连接和监听端口。
- `grep ':443 '`：过滤出端口 443 的连接。
- `grep ESTABLISHED`：过滤出已经建立的连接。
- `wc -l`：统计连接数。
- `watch -n 1`：每秒刷新一次命令的输出。

观察连接数的变化，可以大致估算出每秒的请求数。

![二哥的 Java 进阶之路：技术派的 443 请求数](https://cdn.tobebetterjavaer.com/stutymore/linux-20240902112732.png)



