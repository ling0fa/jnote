[返回目录](../README.md)

### 什么时候触发 Full GC？

- **调用 `System.gc()`**。这种方式应避免，可以通过启动参数 `-XX:+DisableExplicitGC` 来忽略这种调用。

- **老年代空间不足**。新生代对象转入、创建大对象大数组直接晋升到老年代，发现老年代空间不足，触发 Full GC。所以避免使用大对象、大数组就显得非常重要。
- **CMS 的 Concurrent Mode Failure 和 Promotion Failed**。
    - Concurrent Mode Failure 是 CMS 在 GC **过程中**，有新生代对象转入老年代，老年代剩余空间不足导致（CMS 的缺点之一，参考 [CMS 收集器](./垃圾收集器.md#cms-收集器)，进而会使用 Serial Old 收集器来 Full GC）。
    - Promotion Failed 是在 YGC 后新生代空间不足，直接将对象放到老年代，结果老年代也放不下新生代的大对象导致。
- **内存分配担保失败** 参考[内存分配与回收策略](./内存分配与回收策略.md)，相当于是通过计算可能会出现上面的这种情况，于是直接进行 FGC。