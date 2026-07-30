# Chapter 14: 命令模式

## Core Idea
把“要执行的操作”封装成值，使请求可以排队、记录、重试、撤销或交给不同执行者。

## Frameworks Introduced
- **Command**：命令描述意图，处理器执行行为，调用者负责调度。
  - When to use：任务队列、菜单操作、批处理、审计日志、远程请求。
  - How：命令只携带必要数据 → 处理器保持单一职责 → 调度层管理生命周期。

## Modern Java
数据型命令适合用 record；单次进程内回调可用 `Runnable`、`Callable` 或函数式接口。需要持久化和跨版本消费时，必须设计稳定 schema，而不是序列化任意 lambda。

```java
record PlaceOrder(OrderId id, CustomerId customer) {}
```

## Worked Example
原书把菜品请求封装成命令，由服务员收集和调度，再交给对应厨师执行。点单者不直接依赖厨师实现。

## Anti-patterns
- 命令对象携带完整可变领域对象。
- 重试非幂等命令却没有幂等键。
- 将业务规则放在调度器而非处理器。

## Key Takeaways
1. 命令是意图，不是数据库实体快照。
2. 跨进程命令需要版本、幂等和失败策略。
3. 简单回调用函数式接口，复杂生命周期用显式命令类型。

## Connects To
- **Ch17**：命令与备忘录可共同实现撤销。
- **Ch21**：模板可定义命令处理骨架。
