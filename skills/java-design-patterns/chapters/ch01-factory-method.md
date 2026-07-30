# Chapter 1: 工厂方法模式

## Core Idea
把“选择并创建具体对象”的职责移出业务调用方。调用方只依赖产品接口，新增产品时新增实现和注册关系，不改稳定业务流程。

## Frameworks Introduced
- **Factory Method**：用创建接口延迟具体类型选择。
  - When to use：同一业务入口需要返回多种实现，且类型会持续增加。
  - How：提取产品接口 → 每种产品独立实现 → 工厂根据显式类型或元数据选择实现。

## Key Concepts
- **产品接口**：调用方真正依赖的稳定契约。
- **创建策略**：类型标识到实现的选择规则。
- **注册表**：用 `Map<Key, Supplier<Product>>` 替代不断增长的条件分支。

## Modern Java
简单、无状态的创建逻辑可用构造器引用和 `Supplier<T>`；跨模块插件优先考虑 `ServiceLoader`，避免中心工厂依赖所有提供者。

```java
Map<PrizeType, Supplier<PrizeService>> factories = Map.of(
    PrizeType.COUPON, CouponService::new,
    PrizeType.GOODS, GoodsService::new
);
PrizeService service = factories.get(type).get();
```

## Worked Example
原书把优惠券、实物和兑换卡的不同接口统一为奖品服务。重构后，发奖流程只接收统一参数并调用 `send`；新增奖品只需提供新实现和注册项。

## Anti-patterns
- 工厂返回 `Object` 并让调用方强制转换。
- 类型稳定且只有一个实现时仍创建多层工厂。
- 把产品业务逻辑重新塞回工厂。

## Key Takeaways
1. 工厂隔离创建变化，不负责产品行为。
2. 先建立稳定产品接口，再讨论创建方式。
3. 开放插件用 `ServiceLoader`，封闭应用可用注册表。

## Connects To
- **Ch02**：产品族需要协同创建时升级为抽象工厂。
- **Ch06**：旧接口不一致时先适配再交给工厂。
