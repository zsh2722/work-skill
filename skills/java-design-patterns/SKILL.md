---
name: java-design-patterns
description: "Practical Java design-pattern knowledge base covering all 23 GoF patterns, selection heuristics, trade-offs, refactoring techniques, and modern Java 21+ implementations with records, sealed types, pattern matching, lambdas, ServiceLoader, Flow, virtual threads, dependency injection, and Spring AOP. Use when choosing, applying, reviewing, teaching, or refactoring Java design patterns."
---

<!-- argument-hint: [problem, pattern name, comparison, or chapter number] -->

# Java Design Patterns

**Primary source**: 《重学 Java 设计模式》, 小傅哥
**Coverage**: 23 GoF patterns + modern Java/Spring practice
**Source pages**: ~268 | **Chapters**: 24 | **Updated**: 2026-07-30

## How to Use This Skill

- **No argument** — apply the decision framework below.
- **Problem statement** — identify the changing dimension, recommend the smallest suitable pattern, and explain costs.
- **Pattern/comparison** — load the relevant chapter(s), e.g. `strategy vs state`.
- **Chapter** — load `ch01` through `ch24`.
- **Code review/refactor** — preserve behavior with tests, then move one variation point at a time.

When answering, distinguish timeless pattern intent from modern Java implementation. Never recommend a pattern only because its class diagram can be reproduced.

## Core Decision Framework

### 1. Find the Variation

| Variation | Candidate patterns |
|---|---|
| Which object/product is created | Factory Method, Abstract Factory, Builder, Prototype |
| How incompatible systems connect | Adapter, Facade |
| How capabilities combine | Bridge, Composite, Decorator |
| How access is controlled/shared | Proxy, Flyweight, Singleton |
| How a request moves | Chain, Command, Mediator |
| How behavior changes | Strategy, State, Template Method |
| How changes are announced/recovered | Observer, Memento |
| How structures are traversed/operated | Iterator, Visitor, Interpreter |

### 2. Decide Open vs Closed Extension

- **Unknown implementations may arrive later** → interface polymorphism, DI, `ServiceLoader`, classic Strategy/Visitor/Composite.
- **All variants are controlled and finite** → sealed interface + records + exhaustive pattern `switch`.
- **A behavior is short, stateless, single-method** → lambda or method reference.
- **A behavior owns dependencies, configuration, state, or lifecycle** → named class.

### 3. Protect Stable Code

Use the book's recurring refactoring loop:

1. Capture current behavior with tests.
2. Mark duplicated type, state, algorithm, or workflow branches.
3. Extract one stable contract.
4. Move each variation into an independent implementation.
5. Centralize selection at the composition boundary.
6. Re-run the same tests.
7. Add one new variant to verify that stable code remains unchanged.

### 4. Charge the Complexity Budget

Prefer the pattern only when expected change justifies:

- more types and indirection;
- harder tracing and debugging;
- lifecycle, ordering, concurrency, and error semantics;
- framework proxy or serialization constraints.

If the patterned version is harder to explain and the variation is unlikely, keep the simple code.

## High-Value Distinctions

- **Factory vs Strategy** — Factory chooses/creates an object; Strategy performs an interchangeable algorithm.
- **Adapter vs Facade** — Adapter changes a contract; Facade simplifies a subsystem.
- **Decorator vs Proxy** — Decorator adds responsibilities; Proxy controls access.
- **Bridge vs Strategy** — Bridge separates two structural variation axes; Strategy replaces one algorithm.
- **State vs Strategy** — State transitions internally over time; Strategy is normally selected externally.
- **Observer vs Mediator** — Observer broadcasts facts; Mediator coordinates an interaction protocol.
- **Command vs Event** — Command asks one handler to act; event states that something already happened.
- **Template vs Strategy** — Template varies steps through inheritance; Strategy varies behavior through composition.
- **Visitor vs pattern switch** — Visitor favors open operations over stable element types; sealed pattern switch is concise for closed types.

## Modern Java Guardrails

- Records are shallowly immutable data carriers; validate invariants and copy mutable components.
- Sealed types improve exhaustiveness but are wrong for third-party extension points.
- Spring AOP is proxy-based: `this` self-invocation bypasses advice; class proxies cannot advise `final` or `private` methods.
- `Flow`/Reactive Streams adds backpressure; ordinary in-process notifications do not automatically need it.
- Virtual threads are created per task, not pooled. They change concurrency cost, not architectural responsibilities.
- `ServiceLoader` discovers providers; application code should depend on the service, not provider modules.

## Chapter Index

| # | Topic | Key decision |
|---:|---|---|
| [ch01](chapters/ch01-factory-method.md) | Factory Method | Create one of many products |
| [ch02](chapters/ch02-abstract-factory.md) | Abstract Factory | Create compatible product families |
| [ch03](chapters/ch03-builder.md) | Builder | Build validated complex objects |
| [ch04](chapters/ch04-prototype.md) | Prototype | Copy expensive prepared objects |
| [ch05](chapters/ch05-singleton.md) | Singleton | Control instance scope and ownership |
| [ch06](chapters/ch06-adapter.md) | Adapter | Translate an incompatible boundary |
| [ch07](chapters/ch07-bridge.md) | Bridge | Separate two variation axes |
| [ch08](chapters/ch08-composite.md) | Composite | Treat part and whole uniformly |
| [ch09](chapters/ch09-decorator.md) | Decorator | Stack optional responsibilities |
| [ch10](chapters/ch10-facade.md) | Facade | Simplify subsystem use cases |
| [ch11](chapters/ch11-flyweight.md) | Flyweight | Share repeated immutable state |
| [ch12](chapters/ch12-proxy.md) | Proxy | Control target access |
| [ch13](chapters/ch13-chain-of-responsibility.md) | Chain | Pass requests through handlers |
| [ch14](chapters/ch14-command.md) | Command | Represent an operation as data |
| [ch15](chapters/ch15-iterator.md) | Iterator | Traverse without exposing structure |
| [ch16](chapters/ch16-mediator.md) | Mediator | Coordinate collaborator protocols |
| [ch17](chapters/ch17-memento.md) | Memento | Snapshot and restore state |
| [ch18](chapters/ch18-observer.md) | Observer | Notify independent subscribers |
| [ch19](chapters/ch19-state.md) | State | Model state-specific behavior |
| [ch20](chapters/ch20-strategy.md) | Strategy | Replace algorithms |
| [ch21](chapters/ch21-template-method.md) | Template Method | Fix a skeleton, vary steps |
| [ch22](chapters/ch22-visitor.md) | Visitor | Add operations to stable types |
| [ch23](chapters/ch23-interpreter.md) | Interpreter | Evaluate a small DSL |
| [ch24](chapters/ch24-modern-java.md) | Modern Java | Simplify or replace classic forms |

## Topic Index

- **AOP, dynamic proxy, self-invocation** → ch12, ch24
- **AST, DSL, expression tree** → ch08, ch23
- **backpressure, reactive streams** → ch18, ch24
- **caching, object sharing** → ch11
- **dependency injection, composition root** → ch02, ch05, ch24
- **if/else refactoring** → ch01, ch13, ch19, ch20
- **lambda, functional interface** → ch07, ch14, ch20, ch24
- **records, immutable data** → ch03, ch04, ch14, ch17, ch23, ch24
- **rollback, undo, versioning** → ch14, ch17
- **sealed classes, pattern switch** → ch08, ch19, ch22, ch23, ch24
- **ServiceLoader, plugins** → ch01, ch24
- **virtual threads, concurrency** → ch05, ch18, ch24

## Supporting Files

- [glossary.md](glossary.md) — precise terminology
- [patterns.md](patterns.md) — complete pattern catalog and trade-offs
- [cheatsheet.md](cheatsheet.md) — one-page selection rules and smells

## Scope & Limits

The original 22 Java business cases are synthesized from the book. Interpreter and modern implementation guidance are supplemental, based on OpenJDK, Oracle Java SE, Reactive Streams, and Spring Framework documentation linked in ch24. Validate framework/version details against the target project before implementation.
