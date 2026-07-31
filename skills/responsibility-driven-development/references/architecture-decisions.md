# Architecture Decisions

## Contents

1. Treat layers as responsibility roles
2. Direct dependencies toward stability
3. Design ports and adapters
4. Use an abstraction gate
5. Centralize composition
6. Isolate variation axes
7. Define state, error, lifecycle, and concurrency semantics
8. Apply the pattern gate

## 1. Treat Layers as Responsibility Roles

Use these roles when they clarify ownership:

- **Interaction**: capture intent and render state.
- **Application**: coordinate a use case.
- **Domain**: enforce business rules and invariants.
- **Port**: express an external capability required by an inner responsibility.
- **Adapter**: translate a concrete external mechanism into a port.
- **Composition**: choose implementations and lifetimes.

Do not require six directories, modules, or classes. A small feature may combine interaction and application code or use a direct adapter without a domain object.

Split a role only when at least one applies:

- it changes for a different reason;
- it has a different lifetime;
- it needs an independent test boundary;
- it isolates a volatile dependency;
- it owns a distinct consistency boundary;
- it protects inner code from an external contract.

## 2. Direct Dependencies Toward Stability

Stable policies must not depend on volatile details.

Prefer:

```text
Interaction -> Application -> Domain
                     |
                     v
                    Port <- Adapter -> External system

Composition creates and connects all collaborators.
```

Dependency direction is about source knowledge, not runtime call direction. An adapter can call back through an inner contract without making the inner policy depend on the adapter.

### Stability Questions

- Which code changes when the provider changes?
- Which code changes when the business rule changes?
- Which code changes when the UI changes?
- Which code changes when deployment or platform changes?

If the same class changes for several answers, it crosses variation axes.

## 3. Design Ports and Adapters

Define a port when an inner responsibility needs an external capability and one of these is true:

- the provider may change;
- the external contract is unsuitable for business code;
- tests need a deterministic substitute;
- failure, threading, or lifecycle translation is required;
- the capability crosses a process or deployment boundary.

Name the port after the needed capability, not the provider:

- prefer `PaymentAuthorizer` over `StripeService`;
- prefer `DocumentStore` over `PostgresRepository`;
- prefer `Clock` over `SystemTimeManager`.

Keep the port small. Do not copy the provider SDK into a project-owned interface.

### Adapter Responsibilities

An adapter may:

- translate request and response models;
- map errors;
- enforce protocol-level retry or idempotency rules;
- manage provider lifecycle;
- normalize threading or callback behavior;
- add provider-specific observability.

An adapter must not:

- decide unrelated business eligibility;
- navigate UI;
- coordinate an entire use case;
- expose raw provider errors when callers need domain-specific outcomes.

## 4. Use an Abstraction Gate

Create an abstraction only when its value exceeds its cost.

### Evidence That Supports an Abstraction

- two real implementations exist;
- a near-term migration requires a compatibility boundary;
- an external dependency must be isolated;
- deterministic testing requires substituting nondeterminism;
- the contract is more stable than its implementation;
- the abstraction names a meaningful domain capability.

### Evidence Against an Abstraction

- the only purpose is to satisfy a layering diagram;
- the interface mirrors one implementation exactly;
- callers still import implementation-specific types;
- the implementation is stable, local, and trivial;
- the abstraction adds only forwarding methods;
- no caller can explain the contract independently of the implementation.

Do not use testability as a blanket reason. Prefer testing observable behavior; introduce a seam where nondeterminism or external effects actually exist.

## 5. Centralize Composition

Select concrete implementations at a composition boundary:

- application startup;
- module initializer;
- dependency-injection configuration;
- request factory for request-scoped collaborators;
- plugin discovery boundary.

Keep provider selection out of business methods.

The composition boundary owns:

- implementation choice;
- shared versus scoped lifetime;
- initialization order;
- resource shutdown;
- configuration validation;
- fallback policy.

Avoid hidden service location from arbitrary classes. A global locator trades constructor clarity for runtime lookup, hidden dependencies, and difficult tests.

Use plugin discovery only when implementations are genuinely open or independently deployed. For a closed, finite set of variants, explicit construction or an exhaustive selection is simpler.

## 6. Isolate Variation Axes

Name one axis per abstraction:

| Variation | Typical boundary |
|---|---|
| algorithm or policy | function, strategy, policy object |
| data source | port plus source adapter |
| provider protocol | adapter |
| platform | platform adapter or source-set boundary |
| workflow step set/order | explicit coordinator or chain |
| presentation | presenter/mapper plus renderer |
| deployment/process | client port plus remote adapter |
| object family | factory or composition configuration |

Do not combine axes prematurely. A `CloudWindowsEncryptedStorageManager` likely mixes provider, platform, security policy, and storage capability.

When two axes vary independently and combinations matter, consider composition or Bridge. When they do not vary independently, keep the structure direct.

## 7. Define State, Error, Lifecycle, and Concurrency Semantics

### State

- Name the authoritative owner.
- Define legal transitions.
- Make invalid transitions impossible or explicitly rejected.
- Expose read-only observations to consumers.
- Derive presentation state instead of duplicating mutable business state.

### Errors

Classify failures:

- business rejection;
- dependency unavailable;
- invalid external response;
- timeout or cancellation;
- conflict/concurrent modification;
- programming/configuration error.

Translate failures at boundaries. Do not collapse distinguishable failures into `false`, `null`, empty collections, or generic messages.

### Lifecycle

State who creates, starts, stops, and releases a resource. Match listener registration, task cancellation, and cache lifetime to the owner's lifetime.

Default implementations must declare whether they mean:

- unsupported capability;
- safe no-op;
- temporary degraded behavior;
- in-memory substitute;
- configuration error.

Never return apparent success when required work was not performed.

### Concurrency

State:

- serialization or parallelism requirements;
- thread/dispatcher ownership;
- cancellation behavior;
- idempotency key or deduplication policy;
- consistency boundary;
- ordering guarantees.

Do not hide concurrency behind a generic manager or callback.

## 8. Apply the Pattern Gate

Use a pattern only after identifying the pressure it relieves.

| Pressure | Candidate | Reject when |
|---|---|---|
| interchangeable behavior | Strategy | one short closed conditional is clearer |
| state-specific behavior and transitions | State | state is only a display flag |
| optional ordered handlers | Chain | workflow is fixed and must be easy to trace |
| incompatible external contract | Adapter | layer only renames methods |
| simplified subsystem entry | Facade | facade accumulates business ownership |
| creation varies | Factory | direct construction is stable and local |
| facts must notify independent consumers | Observer | it hides required business sequence |
| action must be queued/audited/undone | Command | a direct method call is sufficient |

When `$java-design-patterns` is available, use it for deeper comparison and language-specific implementation. Always carry forward:

- observed problem;
- variation axis;
- direct alternative;
- complexity budget;
- removal condition.

Do not let a pattern recommendation override the responsibility model.
