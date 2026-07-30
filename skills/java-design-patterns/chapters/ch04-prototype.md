# Chapter 4: 原型模式

## Core Idea
从已准备好的对象复制新对象，避免重复执行昂贵初始化；复制语义必须显式区分浅拷贝、深拷贝和共享。

## Frameworks Introduced
- **Prototype**：以模板实例为起点产生变体。
  - When to use：初始化昂贵、模板稳定、实例间只存在局部差异。
  - How：定义明确复制方法 → 复制可变成员 → 保留允许共享的不可变成员 → 修改变体。

## Modern Java
不要默认依赖 `Object.clone()`。现代 Java 更适合复制构造器、静态 `copyOf`、record 的重建方法，或明确的序列化边界。

```java
record Exam(List<Question> questions) {
    Exam shuffled(RandomGenerator random) {
        var copy = new ArrayList<>(questions);
        Collections.shuffle(copy, random);
        return new Exam(List.copyOf(copy));
    }
}
```

## Worked Example
原书先生成一份试卷模板，再为每个考生复制并随机排列题目和选项。共享不可变题库，复制排列列表，可以同时控制性能和隔离性。

## Anti-patterns
- 浅复制后两个实例共享可变集合。
- 对循环引用对象做无边界深拷贝。
- 复制敏感标识、锁、连接或缓存状态。

## Key Takeaways
1. 先定义“什么共享、什么复制”。
2. 优先显式复制 API，而不是神秘的 `clone()`。
3. 复制后必须重新建立身份与业务不变量。

## Connects To
- **Ch17**：备忘录保存状态快照，但目的不是创建新业务对象。
- **Ch03**：Builder 更适合从零逐步创建。
