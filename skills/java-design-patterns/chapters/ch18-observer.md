# Chapter 18: 观察者模式

## Core Idea
当主题发生事件时通知多个订阅者，使稳定核心流程与可变辅助反应解耦。

## Frameworks Introduced
- **Observer**：主题维护订阅关系并发布事件。
  - When to use：同一事实触发多个相互独立的后续动作。
  - How：事件使用过去式事实 → 订阅者幂等 → 明确同步/异步和失败策略。

## Worked Example
原书在摇号完成后分别写 MQ、发短信并记录结果。主流程只发布“摇号已完成”，新增营销或审计监听器不修改摇号逻辑。

## Modern Java
`java.util.concurrent.Flow` 提供 Publisher、Subscriber、Subscription 和背压，适合异步流；简单进程内通知无需上升到响应式流。跨服务事件要考虑至少一次投递、乱序和重复。

## Anti-patterns
- 用事件伪装必须成功的核心命令。
- 假定监听器顺序固定。
- 订阅者失败导致发布者状态半完成。
- 没有取消订阅或背压造成资源泄漏。

## Key Takeaways
1. 事件描述已发生事实，不要求单一接收者执行。
2. 关键一致性步骤保持显式调用或事务协调。
3. 异步流量需要背压、幂等和可观测性。

## Connects To
- **Ch16**：中介者协调对象，观察者广播事实。
- **Ch24**：Flow 与 Reactive Streams。
