[返回目录](../README.md)

### GC 日志

![](./collectors.jpg)

- `Serial` 串行收集器，GC 日志里显示成 DefNew， 即 Default New Generation 的意思，回收时全部工作线程挂起。
- `ParNew` 是 Parallel New Collection 的意思，多个线程收集，且可以和 CMS 协同工作，回收时全部工作线程挂起。
- `Parallel Scavenge Collection` 是多线程收集，日志里显示成 PSYoungGen，回收时全部工作线程挂起。
- `Serial Old（MSC）` 是 Mark Sweep Compact 的意思，使用单线程，回收时全部工作线程挂起。
- `Parallel Old` 使用多线程，回收时全部工作线程挂起。

- `-XX:+UseSerialGC` = `Serial` + `Serial Old`.
- `-XX:+UseConcMarkSweepGC` = `ParNew` + `CMS` + `Serial Old`。 大多时候是由 CMS 回收老年代，当并发回收失败后，才使用 Serial Old 来回收。 
- `-XX:+UseParallelGC` = `Parallel Scavenge` + `Serial Old`。
- `-XX:+UseParNewGC` = `ParNew` + `Serial Old`。
- `-XX:+UseParallelOldGC` = `Parallel Scavenge` + `Parallel Old`。