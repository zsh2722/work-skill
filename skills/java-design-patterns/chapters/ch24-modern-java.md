# Chapter 24: 现代 Java 设计模式实践（截至 2026）

## Core Idea
模式表达的是设计意图，不要求复刻 1990 年代的类结构。现代 Java 可以减少样板代码、强化封闭模型和并发语义，但不能替代对变化方向与边界的判断。

## Language Features

### Records
用 record 表示不可变数据载体，适合命令、事件、备忘录、值对象和 AST 节点。它减少样板代码，但不会自动完成深度不可变、业务校验或持久化版本兼容。

### Sealed Classes + Pattern Matching
sealed 层次适合封闭状态、结果和表达式类型；pattern `switch` 可以穷尽处理，常简化 State、Visitor 和 Interpreter。若实现需要由未知插件扩展，则不要封闭层次。

### Functional Interfaces
lambda 与方法引用可简化无状态 Strategy、Command、Factory 和回调。逻辑变长、需要依赖或需要独立测试名称时，应恢复具名类。

### ServiceLoader
`ServiceLoader` 让应用只依赖服务接口，在运行时发现零到多个提供者。适合插件式 Factory/Provider；模块化应用使用 `uses`/`provides` 声明。

## Framework Reality

### Dependency Injection
Spring 容器承担对象创建、依赖装配和作用域管理，覆盖许多 Factory/Singleton 意图。业务对象仍应通过构造器声明依赖，避免 Service Locator。

### AOP Proxies
Spring AOP 基于 JDK 动态代理或 CGLIB。类代理无法增强 `final`、`private` 方法；目标对象中的 `this` 自调用绕过 advice。事务、缓存、异步等问题排查时先验证代理边界。

### Reactive Observer
`java.util.concurrent.Flow` 对应 Reactive Streams 的 Publisher/Subscriber/Subscription，并提供请求数量控制。高吞吐异步事件需要背压；普通域事件无需强行响应式化。

### Virtual Threads
Java 21 虚拟线程让 thread-per-request 在 I/O 密集服务中更可扩展。虚拟线程应按任务创建而非池化；不要用 ThreadLocal 池化昂贵资源。它改变并发实现成本，但不消除 Command、Mediator 或 Observer 的职责边界。

## Decision Rules

| Situation | Prefer | Avoid |
|---|---|---|
| 封闭类型、需要穷尽处理 | sealed + pattern switch | 为每个小操作创建 Visitor |
| 开放插件实现 | interface + ServiceLoader/DI | sealed 层次 |
| 短小无状态算法 | lambda Strategy | 大量一行实现类 |
| 行为有依赖和生命周期 | 具名 Strategy/Command | 捕获复杂状态的 lambda |
| 简单同步通知 | 直接监听器 | 完整响应式栈 |
| 异步流且生产快于消费 | Flow/Reactive Streams | 无界 push Observer |
| I/O 密集并发任务 | 虚拟线程/每任务线程 | 池化虚拟线程 |

## Sources
- [JEP 395: Records](https://openjdk.org/jeps/395)
- [JEP 409: Sealed Classes](https://openjdk.org/jeps/409)
- [JEP 440: Record Patterns](https://openjdk.org/jeps/440)
- [JEP 441: Pattern Matching for switch](https://openjdk.org/jeps/441)
- [JEP 444: Virtual Threads](https://openjdk.org/jeps/444)
- [Java SE 26 FunctionalInterface](https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/lang/FunctionalInterface.html)
- [Java SE 25 ServiceLoader](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/ServiceLoader.html)
- [Java SE 26 Flow](https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/concurrent/Flow.html)
- [Spring Framework 7 Proxying Mechanisms](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html)

## Key Takeaways
1. 先选择开放或封闭扩展方向，再选择多态或模式匹配。
2. 语言特性减少样板，不替代职责与生命周期设计。
3. 框架代理、异步流和虚拟线程都有必须显式理解的运行时边界。
