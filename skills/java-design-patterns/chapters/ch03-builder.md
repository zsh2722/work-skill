# Chapter 3: 建造者模式

## Core Idea
把复杂对象的逐步构建与最终表示分离，显式表达必填项、可选项、校验和构建顺序。

## Frameworks Introduced
- **Builder**：用可读的步骤累积配置，最后一次性创建有效对象。
  - When to use：构造参数多、组合多、存在跨字段约束，或构建过程分阶段。
  - How：收集参数 → 校验整体不变量 → 创建不可变结果。

## Modern Java
Java `record` 很适合小型不可变数据载体，但不会自动解决多步骤校验；参数较少时直接用 record 构造器，参数复杂时让 Builder 最终创建 record 或不可变类。

```java
var plan = HousePlan.builder(area)
    .ceiling(Ceiling.SECOND_LEVEL)
    .floor(Floor.WOOD)
    .paint(Paint.LOW_VOC)
    .build();
```

## Worked Example
原书将吊顶、涂料、地板、地砖组合成不同装修套餐。Builder 统一价格累计和清单生成，套餐只描述物料选择；新增套餐不再复制整段计算逻辑。

## Anti-patterns
- 只有两个参数仍生成几十行 Builder。
- `build()` 不校验，使无效对象流入系统。
- Builder 可被多线程共享或构建后继续修改结果。

## Key Takeaways
1. Builder 的价值是表达构建语义和不变量。
2. 小数据优先 record，复杂构建再引入 Builder。
3. 组合爆炸时把套餐关系配置化。

## Connects To
- **Ch04**：构建成本高且结果相近时可原型复制。
- **Ch24**：records 与不可变数据建模。
