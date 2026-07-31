# Anti-Patterns

## Contents

1. Responsibility concentration
2. Empty and pass-through abstractions
3. Boundary bypasses
4. Global state and event control flow
5. Silent fallback and swallowed errors
6. Multiple truths and permanent bridges
7. Pattern-driven design
8. Correction strategy

## 1. Responsibility Concentration

### God Object

**Symptoms**

- owns several workflows, integrations, caches, and UI concerns;
- has many unrelated dependencies;
- changes for unrelated features;
- exposes broad getters so others can reach internals.

**Harm**

- one change risks many behaviors;
- tests require excessive setup;
- state and lifecycle become ambiguous.

**Correct**

- identify authoritative decisions and state;
- extract cohesive use cases or capabilities;
- keep composition separate from runtime business behavior.

Do not split mechanically into many pass-through classes.

### Generic Manager

`Manager`, `Helper`, `Util`, and `Common` names often hide unclear ownership.

Keep such a name only when the managed resource and lifecycle are precise, such as a connection-pool manager. Otherwise name the domain capability or use case.

## 2. Empty and Pass-Through Abstractions

### One-Implementation Interface

An interface is not automatically wrong with one implementation, but require a reason: external boundary, deterministic substitution, public contract, or active migration.

Remove it when it only mirrors the class and no caller benefits from the separation.

### Anemic Forwarding Layer

**Symptoms**

- every method immediately calls the next layer;
- types and errors pass through unchanged;
- no ownership, translation, policy, or stability is added.

**Correct**

- collapse the layer; or
- give it a real responsibility such as use-case orchestration, error translation, or source selection.

### Base Class with Empty Defaults

Empty methods can hide missing behavior and make unsupported variants appear valid.

Prefer:

- abstract requirements;
- capability queries with explicit unsupported results;
- composition of optional behavior;
- a named no-op whose semantics are truly safe.

## 3. Boundary Bypasses

### Direct Persistence Access

**Symptoms**

- UI or application code reaches a DAO beside the repository;
- some writes update one store while others update another;
- mapping and query rules spread to callers.

**Harm**

- repository is no longer authoritative;
- caching and invariants diverge;
- storage replacement touches every caller.

**Correct**

- add the required capability to a caller-owned port;
- implement it at the data boundary;
- migrate callers;
- remove direct access.

### External Model Leakage

Do not let DTOs, database entities, SDK callbacks, parcel types, or provider errors become general business models.

Translate them where the external contract enters the system.

## 4. Global State and Event Control Flow

### Service Locator as Default Dependency Injection

Static lookup hides dependencies, lifetime, and failure.

Prefer explicit construction or injection. Keep a locator only at a legacy or plugin boundary, and prevent arbitrary domain/application access.

### Event Bus as Workflow Engine

Events are useful for independent reactions to facts. They are harmful when required steps depend on subscription order or implicit listeners.

Use an explicit coordinator for required sequence, transaction, retry, and compensation. Publish an event after the authoritative transition.

### Multiple Writable Observables

Several mutable streams representing one screen or business state can produce impossible combinations.

Use one state owner and derive read models.

## 5. Silent Fallback and Swallowed Errors

### False Success

Returning success, empty data, or a default value after required work fails corrupts the caller's understanding.

Return a named unsupported, unavailable, rejected, or degraded result.

### Catch-All Logging

Logging an exception and continuing is not error handling when the caller needs to react.

Catch only to:

- translate;
- add context;
- retry under explicit policy;
- compensate;
- terminate safely.

Preserve the cause.

### Ambiguous Null

Do not use null for several meanings such as not found, not loaded, unavailable, invalid, and unauthorized. Use a result model when callers need different actions.

## 6. Multiple Truths and Permanent Bridges

### Dual Authority

Two managers, stores, state streams, or APIs both accepting writes create divergence.

Choose one authority. Make the other a read-only adapter or compatibility delegate until removal.

### Permanent Compatibility Bridge

A bridge without owner, telemetry, callers, and removal condition becomes the architecture.

Record the exit before adding the bridge.

### Commented-Out Architecture

Commented code and parallel “new” packages are not migration documentation. Remove dead code; use version control and explicit migration notes in the active task.

## 7. Pattern-Driven Design

### Interface Inflation

Do not create interfaces to make a diagram appear decoupled. Measure decoupling by whether stable policy avoids volatile details.

### Strategy for Closed Trivial Branches

A short, finite conditional is often clearer. Extract strategy only when behavior varies independently, carries dependencies/state, or changes frequently.

### State for Display Flags

Use State when transitions and state-specific behavior are substantial. A boolean or sealed result may be enough for simple display state.

### Chain for Fixed Workflow

A chain is justified when steps are independently ordered, skipped, stopped, or extended. A fixed workflow is clearer as explicit code.

### Observer for Required Order

Use direct orchestration when every step must run or failures require compensation.

### Speculative Plugin Architecture

Do not add discovery, registration, or classpath plugins for variants controlled in one codebase unless independent delivery is a real requirement.

## 8. Correction Strategy

Correct anti-patterns in this order:

1. protect business behavior with tests;
2. identify the authoritative owner;
3. stop new bypasses and writes;
4. introduce the smallest boundary;
5. redirect one caller group;
6. make failures explicit;
7. remove old authority and bridge;
8. simplify types that no longer earn their cost.

Do not replace one global manager with several global managers. Do not replace direct code with a pattern unless the responsibility and variation are clearer afterward.
