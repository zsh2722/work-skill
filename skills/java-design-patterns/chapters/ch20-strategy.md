# Chapter 20: 策略模式

## Core Idea
把一组可互换算法置于共同契约后，由上下文在运行时选择，而不改变使用算法的流程。

## Frameworks Introduced
- **Strategy**：封装算法变化。
  - When to use：折扣、路由、排序、风控或定价存在同类可替换规则。
  - How：定义输入输出契约 → 独立纯策略 → 在边界选择 → 上下文调用。

## Modern Java
无状态单方法策略优先函数式接口、lambda 或方法引用；有依赖、配置和生命周期的策略使用具名类。用 `Map<Key, Strategy>` 注册，避免选择逻辑重新变成大分支。

```java
Map<CouponType, UnaryOperator<BigDecimal>> discounts = Map.of(
    CouponType.FLAT, price -> price.subtract(TEN),
    CouponType.PERCENT, price -> price.multiply(NINETY_PERCENT)
);
```

## Worked Example
原书把满减、直减、折扣等优惠券计算拆成策略，统一上下文负责调用，新增优惠类型不改已有算法。

## Anti-patterns
- 策略共享可变请求状态。
- lambda 过长且没有业务名称。
- 选择规则散落在控制器、服务和工厂多处。

## Key Takeaways
1. 策略契约必须统一输入、输出与错误语义。
2. 简单算法用函数，复杂算法用对象。
3. 策略解决“怎么算”，工厂解决“创建谁”。

## Connects To
- **Ch07**：桥接处理两个维度。
- **Ch14**：命令表示请求，策略表示算法。
