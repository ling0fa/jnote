[返回目录](../README.md)

### 常用JVM诊断工具命令

#### jps

```bash
$ jps [ option ] [ hostid ]

$ jps -l # 输出主类全名
$ jps -v # 输出启动参数
```

#### jstat

虚拟机统计信息监视工具

```bash
$ jstat [ option vmid [ interval [ s| ms ]] [ count ] ]
$ jstat -gc 6678 500 10 # 每 500ms 输出 10 次 GC 情况
```

还有很多工具，`-class / -gc / -gccapacity / -gcutil / -gccause / -gcnew / -gcnewcapacity / -gcold / -gcoldcapacity / -gcpermcapacity / -compiler / -printcompilation`。

#### jinfo

Java 配置信息工具，提供查看和修改虚拟机参数的能力。

```bash
$ jinfo [ option ] pid
```

#### jmap

Java 内存映像工具

```bash
$ jmap [ option ] vmid
$ jmap -heap 7788 # 显示堆详情，使用哪种回收器，参数配置等
$ jmap -dump:format=b,file=dump.bin 7788 # 生成堆快照
```

#### jhat

堆快照分析工具，和 jmap 搭配使用。一般使用图形化界面来分析，很少会用这个命令。

```bash
$ jhat dump.bin
```

#### jstack

生成当前时刻线程快照，一般用于分析死锁等线程会长时间等待的问题。

```bash
jstack [ option ] vmid
jstack -l 7788 # 输出和锁相关的信息
```