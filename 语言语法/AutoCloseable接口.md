[返回目录](../README.md)

### AutoCloseable 接口

- JDK1.7 引入了 `AutoCloseable` 接口，实现了该接口的流在 `try-with-resource` 模块的 `try` 语句结束后，即使是在异常的情况下，会自动调用流实现的 `AutoCloseable` 接口里的 `close` 方法。

- `Closeable` 继承了 `AutoCloseable`，所以实现了 `Closeable` 的流在 `try-with-resource` 里也会自动关闭。

- `try` 管理的资源变量都被隐式的转换为 `final` 类型，多个资源用分号隔开。
