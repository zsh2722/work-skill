# Glossary

**Abstract Factory（抽象工厂）** — 创建一组相互兼容的产品族（Ch02）。

**Adapter（适配器）** — 把既有接口和语义转换为目标契约（Ch06）。

**Backpressure（背压）** — 消费者通过请求数量限制生产速度的流量控制（Ch18, Ch24）。

**Bridge（桥接）** — 把两个独立变化维度拆成组合关系（Ch07）。

**Builder（建造者）** — 分步骤收集配置并一次性创建有效复杂对象（Ch03）。

**Chain of Responsibility（责任链）** — 请求沿有序节点传播直至处理或终止（Ch13）。

**Command（命令）** — 将操作意图封装成可调度、记录或重试的值（Ch14）。

**Composite（组合）** — 用统一接口处理叶节点和组合节点（Ch08）。

**Decorator（装饰器）** — 保持接口不变地叠加对象职责（Ch09）。

**Dependency Injection（依赖注入）** — 由组合根或容器向对象提供协作者（Ch02, Ch24）。

**Facade（外观）** — 为复杂子系统提供面向用例的粗粒度入口（Ch10）。

**Factory Method（工厂方法）** — 隔离具体产品选择和创建（Ch01）。

**Flyweight（享元）** — 分离内在与外在状态，共享重复对象（Ch11）。

**Functional Interface（函数式接口）** — 只有一个抽象方法、可由 lambda 或方法引用实现的接口（Ch07, Ch20, Ch24）。

**Interpreter（解释器）** — 用表达式树表示并执行小型领域语言（Ch23）。

**Iterator（迭代器）** — 在不暴露集合结构的情况下维护遍历状态（Ch15）。

**Mediator（中介者）** — 集中管理多个协作者之间的交互协议（Ch16）。

**Memento（备忘录）** — 保存并恢复对象状态快照（Ch17）。

**Observer（观察者）** — 一个事实发生后通知多个独立订阅者（Ch18）。

**Open/Closed Principle（开闭原则）** — 对扩展开放，对稳定代码修改关闭。

**Pattern Matching（模式匹配）** — 按类型和结构解构值并执行穷尽分支（Ch19, Ch22–24）。

**Port（端口）** — 领域对外声明的稳定能力契约（Ch06）。

**Prototype（原型）** — 从准备好的实例复制新对象（Ch04）。

**Proxy（代理）** — 通过相同接口控制目标对象访问（Ch12）。

**Record** — Java 的透明不可变数据载体，适合值、命令、事件和快照（Ch03, Ch14, Ch17, Ch24）。

**Sealed Type（密封类型）** — 限制允许子类型集合的类或接口（Ch08, Ch19, Ch22–24）。

**ServiceLoader** — JDK 的服务提供者发现与加载机制（Ch01, Ch24）。

**Singleton（单例）** — 在明确作用域内限制实例数量为一（Ch05）。

**State（状态）** — 将依赖状态的行为与合法转换显式建模（Ch19）。

**Strategy（策略）** — 在共同契约后封装可互换算法（Ch20）。

**Template Method（模板方法）** — 固定算法骨架，将少量步骤延迟给实现者（Ch21）。

**Visitor（访问者）** — 在稳定元素层次上增加类型相关操作（Ch22）。

**Virtual Thread（虚拟线程）** — JDK 管理的轻量线程，适合 I/O 密集的每任务线程模型（Ch24）。
