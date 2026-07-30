# Java Design Patterns Cheatsheet

## 先问四个问题

1. **什么在变化？** 创建类型、结构组合、算法、状态还是协作流程？
2. **扩展集合开放还是封闭？** 未知插件用接口多态；固定类型用 sealed + 穷尽匹配。
3. **行为是否足够简单？** 单方法无状态行为用 lambda；有依赖和生命周期用具名类。
4. **收益是否覆盖成本？** 若新增类、间接层和调试成本高于未来变化，不使用模式。

## 快速选择

| 变化信号 | 首选 | 关键检查 |
|---|---|---|
| 多种产品创建 | Factory Method | 工厂不要包含产品业务 |
| 成套实现切换 | Abstract Factory | 产品族是否真的相关 |
| 参数和构建步骤多 | Builder | `build()` 是否验证不变量 |
| 昂贵模板产生变体 | Prototype | 浅/深复制边界 |
| 两个独立变化轴 | Bridge | 是否正交 |
| 外部接口不兼容 | Adapter | 是否翻译错误与版本 |
| 运行时叠加能力 | Decorator | 包装顺序 |
| 控制对象访问 | Proxy | 调用是否经过代理 |
| 部分与整体统一 | Composite | 环和所有权 |
| 请求逐级处理 | Chain | 顺序与终止 |
| 算法可替换 | Strategy | 选择逻辑位置 |
| 行为随状态变化 | State | 先画转换表 |
| 事实通知多方 | Observer | 一致性、幂等、背压 |
| 固定骨架变步骤 | Template | 继承还是组合 |
| 稳定类型增操作 | Visitor | sealed switch 是否更简单 |
| 小型规则语言 | Interpreter | DSL 资源限制 |

## 现代 Java 替换规则

- **Command/Event/Memento/AST 数据** → 优先 `record`。
- **封闭 State/Result/AST** → `sealed interface` + pattern `switch`。
- **短 Strategy/Factory/Command** → 函数式接口、lambda、方法/构造器引用。
- **开放插件 Provider** → `ServiceLoader` 或依赖注入。
- **异步 Observer 有速度差** → `Flow`/Reactive Streams 背压。
- **I/O 密集每请求任务** → 虚拟线程；不要池化虚拟线程。

## Tells & Smells

- `switch(type)` 在多层重复出现 → 缺 Strategy/Factory/State 边界。
- 每加渠道都复制所有验证方式 → 缺 Bridge。
- Spring 事务/缓存注解不生效 → 检查 `this` 自调用、`final/private` 和代理类型。
- 事件订阅者承担必须成功的核心步骤 → 应改为显式命令或事务协调。
- Builder 可创建半有效对象 → 校验放错位置。
- 缓存 Map 持续增长 → Flyweight 缺少容量和失效策略。
- 模式引入后比原逻辑更难解释 → 回退到更简单设计。
