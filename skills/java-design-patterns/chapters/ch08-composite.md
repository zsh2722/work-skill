# Chapter 8: 组合模式

## Core Idea
用树形结构统一表示叶节点和组合节点，让调用方以相同方式处理“部分”和“整体”。

## Frameworks Introduced
- **Composite**：组件接口由叶子和容器共同实现。
  - When to use：组织树、菜单、文件系统、规则树、表达式树。
  - How：定义统一操作 → 叶节点执行原子行为 → 组合节点对子节点递归委托。

## Modern Java
封闭的树节点类型可用 sealed interface 与 records 建模，再用穷尽 `switch` 遍历；需要第三方扩展节点时，经典多态 Composite 更合适。

## Worked Example
原书把性别、年龄等营销规则组成决策树。外部只调用根节点，节点决定下一步并最终产生结果；新增规则节点不改树执行骨架。

## Anti-patterns
- 组合节点允许任意成环，递归无法终止。
- 叶节点被迫实现无意义的 `add/remove`。
- 在遍历时修改子节点集合。

## Key Takeaways
1. 树结构必须定义所有权、顺序和环约束。
2. 统一调用不等于统一所有管理操作。
3. 封闭树可用 sealed 类型，开放树用多态接口。

## Connects To
- **Ch13**：责任链是线性传播结构。
- **Ch23**：解释器常以表达式组合树实现。
