# Chapter 5: 单例模式

## Core Idea
确保某个有状态协调者在指定作用域内只有一个实例。真正问题是生命周期和所有权，而不是隐藏一个全局变量。

## Frameworks Introduced
- **Singleton**：控制实例数量并提供受控访问。
  - When to use：进程级注册表、不可重复的协调资源，且作用域确实是 JVM。
  - How：优先容器作用域或枚举；明确并发、序列化和测试策略。

## Modern Java
依赖注入容器中的 singleton 通常是“每个容器一个实例”，不一定是 JVM 全局。无继承需求的纯 Java 单例可用枚举；不要为了懒加载默认使用复杂双重检查。

```java
enum MetricsRegistry {
    INSTANCE;
    void record(String name) { /* ... */ }
}
```

## Worked Example
原书比较饿汉、懒汉、同步方法、静态内部类、双重检查、CAS 与枚举。现代判断顺序应是：能否由组合根创建并注入；若必须 JVM 唯一，再选择最简单且正确的实现。

## Anti-patterns
- 业务代码到处直接访问全局实例。
- singleton 持有请求级或用户级状态。
- 用单例掩盖依赖关系，导致测试相互污染。
- 在虚拟线程时代用 `ThreadLocal` 单例式缓存昂贵资源。

## Key Takeaways
1. 优先显式依赖和清晰作用域。
2. 单例不自动线程安全。
3. 生命周期由容器管理时不要再手写全局实例。

## Connects To
- **Ch24**：依赖注入、虚拟线程与现代生命周期。
