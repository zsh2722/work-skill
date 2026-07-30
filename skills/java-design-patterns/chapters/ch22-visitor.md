# Chapter 22: 访问者模式

## Core Idea
在稳定对象结构上增加多种类型相关操作，通过双分派避免在业务代码中反复类型判断。

## Frameworks Introduced
- **Visitor**：元素接受访问者，访问者为每种元素定义操作。
  - When to use：元素类型稳定，而报表、导出、校验等操作经常增加。
  - How：固定元素层次 → 定义访问契约 → 每个操作一个访问者。

## Worked Example
原书让家长和校长以不同视角读取学生、老师信息：家长关注排名和教师级别，校长关注班级与升学率。

## Modern Java
当元素集合封闭时，sealed interface + record patterns + 穷尽 `switch` 可替代传统 Visitor，并由编译器检查遗漏。若元素类型由插件开放扩展，经典 Visitor 仍有价值，但新增元素会修改所有访问者。

## Anti-patterns
- 元素类型频繁新增仍选择 Visitor。
- 访问者为方便而穿透对象封装。
- 把简单格式化写成庞大双分派体系。

## Key Takeaways
1. Visitor 优化“操作经常增加、类型稳定”的方向。
2. 封闭类型优先评估 pattern switch。
3. 明确其对封装和迪米特原则的代价。

## Connects To
- **Ch08**：遍历组合结构。
- **Ch24**：sealed classes、records、模式匹配。
