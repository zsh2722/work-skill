# Pattern Catalog

## 创建

### Factory Method
**When to use**：产品实现持续增加，调用方需要稳定接口。
**How**：产品接口 + 独立实现 + 注册表/工厂。
**Trade-offs**：隔离创建变化，但实现类型可能膨胀。

### Abstract Factory
**When to use**：一组协作者必须成套替换。
**How**：产品族接口 + 家族工厂 + 组合根选择。
**Trade-offs**：保证一致性，但新增产品种类会修改所有家族。

### Builder
**When to use**：构造参数多、跨字段校验复杂。
**How**：分步收集，`build()` 统一校验并创建不可变结果。
**Trade-offs**：提高可读性，但简单对象会过度设计。

### Prototype
**When to use**：初始化昂贵，实例只局部不同。
**How**：显式复制可变状态，安全共享不可变状态。
**Trade-offs**：对象图和循环引用使深复制复杂。

### Singleton
**When to use**：资源在明确作用域内确实只能有一个。
**How**：优先容器作用域；纯 Java 可用枚举。
**Trade-offs**：容易形成隐藏全局依赖和测试污染。

## 结构

### Adapter
**When to use**：外部接口、协议或模型与领域契约不同。
**How**：在边界转换数据、行为、错误和版本。
**Trade-offs**：隔离外部变化，但映射层需要维护。

### Bridge
**When to use**：两个变化轴形成组合爆炸。
**How**：每个维度独立抽象，通过组合装配。
**Trade-offs**：维度不正交时反而增加复杂度。

### Composite
**When to use**：需要统一处理部分与整体。
**How**：叶子与组合实现共同组件接口。
**Trade-offs**：类型约束和环检测更困难。

### Decorator
**When to use**：运行时可选地叠加职责。
**How**：共同接口 + 委托链。
**Trade-offs**：顺序影响语义，调用栈可能难观察。

### Facade
**When to use**：调用一个用例需要了解多个子系统。
**How**：提供粗粒度用例入口并转换底层模型。
**Trade-offs**：容易演变成 God Service。

### Flyweight
**When to use**：大量对象包含可测量的重复状态。
**How**：共享内在状态，外在状态按调用传入。
**Trade-offs**：缓存失效、容量和并发管理复杂。

### Proxy
**When to use**：访问需要鉴权、事务、远程或延迟控制。
**How**：同接口代理目标，在委托前后增强。
**Trade-offs**：自调用、final/private 方法和异常语义容易踩坑。

## 行为

### Chain of Responsibility
**When to use**：请求依次通过可变处理步骤。
**How**：显式顺序、终止结果和链路追踪。
**Trade-offs**：长链影响性能和调试。

### Command
**When to use**：操作需要排队、持久化、重试或撤销。
**How**：命令 record + 独立处理器 + 调度层。
**Trade-offs**：跨进程需要版本与幂等设计。

### Iterator
**When to use**：自定义结构或惰性资源需要标准遍历。
**How**：迭代器维护位置，集合隐藏表示。
**Trade-offs**：并发修改和资源关闭语义复杂。

### Mediator
**When to use**：协作者形成网状依赖。
**How**：中介集中交互协议，协作者保持局部职责。
**Trade-offs**：中介可能成为 God Object。

### Memento
**When to use**：撤销、回滚、版本恢复。
**How**：不可变最小快照 + 有界历史。
**Trade-offs**：存储成本和版本兼容。

### Observer
**When to use**：一个事实触发多个独立反应。
**How**：过去式事件 + 幂等订阅者 + 失败/背压策略。
**Trade-offs**：顺序、一致性和重复投递。

### State
**When to use**：行为随生命周期状态变化。
**How**：状态×事件表 + 合法转换 + 原子持久化。
**Trade-offs**：状态多时实现类型膨胀。

### Strategy
**When to use**：同类算法需要动态替换。
**How**：统一输入输出 + 函数/对象策略 + 注册选择。
**Trade-offs**：选择逻辑可能重新集中成大分支。

### Template Method
**When to use**：流程骨架稳定、少数步骤变化。
**How**：固定骨架 + 抽象步骤/hook。
**Trade-offs**：继承耦合；组合有时更合适。

### Visitor
**When to use**：元素类型稳定、操作经常新增。
**How**：双分派，或封闭层次上的 pattern switch。
**Trade-offs**：新增元素成本高，可能破坏封装。

### Interpreter
**When to use**：小型、受控 DSL。
**How**：语法 → AST → 显式上下文 → 求值。
**Trade-offs**：不适合通用语言，必须限制资源和能力。
