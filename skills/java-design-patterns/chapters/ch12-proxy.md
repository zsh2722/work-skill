# Chapter 12: 代理模式

## Core Idea
用与目标相同的接口控制访问，在调用前后执行鉴权、事务、延迟加载、远程通信或监控。

## Frameworks Introduced
- **Proxy**：代理保存目标引用并决定何时、如何委托。
  - When to use：访问需要统一控制，且调用方不应感知控制机制。
  - How：接口代理优先 → InvocationHandler/生成代理 → 明确异常和身份语义。

## Worked Example
原书模拟 MyBatis：调用 DAO 接口方法时，动态代理读取方法与 SQL 映射，执行数据库操作并返回结果。

## Modern Java & Spring
JDK `Proxy.newProxyInstance` 为接口生成代理。Spring AOP 使用 JDK 动态代理或 CGLIB；类代理不能增强 `final`、`private` 方法，目标对象内部的 `this` 自调用会绕过代理。

## Anti-patterns
- 认为加了注解就一定经过代理。
- 在同类内部调用事务/缓存方法并期待 advice 生效。
- 代理改变 `equals/hashCode` 或异常契约却不记录。

## Key Takeaways
1. 先确认调用确实穿过代理边界。
2. 接口代理通常更清晰，类代理受继承规则限制。
3. 装饰器添加业务职责，代理控制访问机制。

## Connects To
- **Ch09**：装饰器。
- **Ch24**：Spring 7 代理机制与模块限制。
