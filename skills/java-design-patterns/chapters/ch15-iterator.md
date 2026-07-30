# Chapter 15: 迭代器模式

## Core Idea
把遍历算法与集合内部表示分离，让调用方按统一协议逐个访问元素。

## Frameworks Introduced
- **Iterator**：维护遍历位置并提供 `hasNext/next` 语义。
  - When to use：自定义树、图、分页游标或惰性数据源。
  - How：明确遍历顺序 → 定义并发修改策略 → 惰性获取下一个元素。

## Worked Example
原书为组织架构树实现深度优先遍历，调用方不需要知道部门与员工节点的存储结构。

## Modern Java
普通集合直接使用 `Iterable`、增强 `for`、Stream 或 `Spliterator`。不要为已有集合重复实现 GoF 迭代器；只有自定义遍历状态或惰性资源时才值得。

## Anti-patterns
- `next()` 隐式执行昂贵远程调用但 API 未说明。
- 遍历期间修改底层结构且没有快照或 fail-fast 策略。
- 用 Stream 表达依赖副作用顺序的复杂流程。

## Key Takeaways
1. 先定义顺序、惰性和资源关闭语义。
2. 优先采用 JDK 标准遍历协议。
3. 分页迭代器要处理重复、丢失和游标失效。

## Connects To
- **Ch08**：组合树经常由迭代器遍历。
- **Ch22**：访问者可在遍历过程中执行类型相关操作。
