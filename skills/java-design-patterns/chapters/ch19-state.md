# Chapter 19: 状态模式

## Core Idea
把依赖当前状态的行为拆到状态对象中，使合法转换显式化，避免散落的状态条件判断。

## Frameworks Introduced
- **State**：上下文把行为委托给当前状态，转换产生新状态。
  - When to use：生命周期清晰、不同状态允许的操作差异大且持续变化。
  - How：列出状态×事件表 → 定义合法转换 → 每次转换原子持久化。

## Modern Java
封闭状态集合可用 enum 或 sealed interface；简单状态机用穷尽 pattern `switch` 可能比大量状态类更清晰。需要开放扩展或每个状态行为复杂时使用经典多态。

## Worked Example
原书把营销活动的编辑、审核、通过、拒绝、上线等流转拆成状态服务，非法转换返回明确结果。

## Anti-patterns
- 只把原 `if/else` 一对一搬成空壳类。
- 未定义并发转换和乐观锁。
- 状态与流程历史混为一谈。

## Key Takeaways
1. 先画状态转换表，再写类。
2. 状态少且封闭时优先 enum/sealed + 穷尽 switch。
3. 持久化转换必须原子且可审计。

## Connects To
- **Ch13**：责任链描述处理路径。
- **Ch23**：封闭代数数据类型与匹配。
