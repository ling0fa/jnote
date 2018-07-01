[返回目录](../README.md)

### HashMap 和 ConcurrentHashMap 计算下标逻辑

- 1.7
    - `HashMap`：4 次位运算，5 次异或运算，共 9 次扰动
    ```
        h ^= k.hashCode();
        h ^= (h >>> 20) ^ (h >>> 12) ^ (h >>> 7) ^ (h >>> 4)
        index = h & (length - 1) 
    ```
    - `ConcurrentHashMap`：1 次位运算，1 次异或运算，共 2 次扰动。
    各种把 `hashcode` 左移右移得到 `h`，`index = h & (length - 1)` 定位到某个 `segment`，`h` 和 `segment` 内部的 `table` 的 `length-1` 相与，定位到某个 `HashEntry`，再循环比较链表内容。

- 1.8
    - HashMap：
    ```
        index =  (h ^ (h >>> 16)) & (length-1)
    ```
    - ConcurrentHashMap：
    ```
        index = (h ^ (h >>> 16)) & 0x7fffffff & (length - 1)
    ```

- 主要还是简化了 `HashMap` 的计算规则，按照 1.8 注释，这样做的理由是对 speed, utility, quality of bit-spreading 权衡的结果，高低 16 位异或后再与，比直接和长度减一相与的碰撞几率低。

- `ConcurrentHashMap` 多了一个和 `0x7fffffff` 与操作，是因为 `ConcurrentHashMap` 的大小是 2 的 n 次幂，高两位 bit 用来做控制位，所以最大只能是 `1<<30`，`0x7fffffff` 除了符号位是 0，其余都是1，所以与之后，符号位始终为 0，下一位是控制位，低 30 位与上长度减一，结果是数组下标。