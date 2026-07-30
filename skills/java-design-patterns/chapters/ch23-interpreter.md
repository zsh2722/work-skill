# Chapter 23: 解释器模式（补充）

## Core Idea
为小型、稳定的领域语言定义语法表示，并把表达式组合成可求值的树。该模式补足原书未覆盖的第 23 个 GoF 模式。

## Frameworks Introduced
- **Interpreter**：终结符与非终结符共同实现表达式接口。
  - When to use：权限表达式、规则过滤器、查询条件等语法规模小且受控。
  - How：先定义语法 → 构建 AST → 显式上下文 → 解释或编译执行。

## Modern Java
AST 很适合用 sealed interface 和 records 建模，pattern `switch` 可实现穷尽求值。

```java
sealed interface Expr permits Lit, Add, Multiply {}
record Lit(int value) implements Expr {}
record Add(Expr left, Expr right) implements Expr {}
record Multiply(Expr left, Expr right) implements Expr {}

static int eval(Expr e) {
    return switch (e) {
        case Lit(var v) -> v;
        case Add(var l, var r) -> eval(l) + eval(r);
        case Multiply(var l, var r) -> eval(l) * eval(r);
    };
}
```

## Worked Example
折扣规则可表示为 `and(user.vip, amount > 100)`。解析器生成表达式树，解释器只读取显式上下文，不直接访问整个订单服务。

## Anti-patterns
- 用该模式实现通用编程语言。
- 直接解释用户输入但没有语法限制和资源上限。
- 每次请求重新解析相同表达式。

## Key Takeaways
1. 只适合小型 DSL；复杂语法使用解析器工具。
2. AST 与执行上下文必须隔离。
3. 缓存已解析 AST，并限制深度、时间与可调用能力。

## Connects To
- **Ch08**：表达式树是组合模式。
- **Ch22**：对 AST 增加多种操作可使用访问者或 pattern switch。
