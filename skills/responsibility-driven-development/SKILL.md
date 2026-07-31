---
name: responsibility-driven-development
description: Model business behavior, assign clear ownership, control information boundaries, and design the smallest maintainable architecture before implementing or refactoring code. Use for requirement modeling, new feature design, implementation planning, legacy-code refactoring, architecture reviews, UI/business separation, state and lifecycle ownership, repository or integration boundaries, workflow decomposition, dependency direction, and deciding whether a design pattern is justified.
---

# Responsibility-Driven Development（职责驱动开发）

Design from business responsibilities and information boundaries instead of copying a framework layout or a pattern catalog.

Treat existing code as evidence, not authority. Preserve behavior that is correct, correct designs that are unsafe or costly, and remove accidental structure from the target model.

## Select the Operating Mode

Choose one primary mode and keep the work scoped to it:

- **Model**: reconstruct actors, goals, facts, rules, states, actions, failures, and invariants.
- **Design**: assign responsibilities, define boundaries and contracts, and choose the smallest viable structure.
- **Implement**: make a focused change that follows an agreed responsibility model.
- **Refactor**: move one responsibility or variation axis at a time while protecting behavior.
- **Review**: report correctness and ownership problems before style or pattern preferences.

If the request mixes modes, model and design before implementing. Do not implement when the user asked only for analysis or review.

## Run the RDD Workflow

### 1. Establish Truth

Inspect requirements, callers, tests, data contracts, neighboring code, and runtime constraints.

Separate:

- observed behavior;
- explicit requirements;
- inferred intent;
- unresolved assumptions.

Do not infer business rules from class names or existing layers alone.

### 2. Discover the Business

Identify:

- actor and goal;
- trigger and preconditions;
- business facts and invariants;
- commands and events;
- states and legal transitions;
- success result and failure outcomes;
- side effects and consistency boundary.

Read [responsibility-modeling.md](references/responsibility-modeling.md) for non-trivial workflows, stateful behavior, or unclear ownership.

### 3. Assign Responsibilities

Give each decision, state, side effect, and integration one authoritative owner.

For each responsibility, record:

- purpose;
- input and output;
- knowledge it may hold;
- knowledge it must not hold;
- state ownership;
- side effects;
- failure semantics;
- lifetime and concurrency expectations.

Split responsibilities when they change for different reasons. Keep closely related rules together when separating them would create chatty, anemic objects.

### 4. Control Information

Push volatile technical information outward:

- keep interaction code limited to intents, renderable state, and effects;
- keep use-case ordering, authorization, retry, and transaction policy in the application responsibility;
- keep business invariants independent of UI, storage, transport, and platform types;
- express external needs through caller-owned ports;
- translate database, network, device, process, file, and framework details in adapters;
- select implementations and own process-wide lifetimes in a composition boundary.

Treat these as roles, not mandatory folders or modules. Collapse roles when the code is simple and their policies do not vary.

### 5. Identify Real Variation

Name the concrete variation axis: algorithm, source, platform, protocol, policy, presentation, lifecycle, or deployment.

Keep stable rules independent of that axis. Do not create extension points for imagined futures.

Read [architecture-decisions.md](references/architecture-decisions.md) before adding interfaces, layers, repositories, service locators, plugins, global state, or cross-process boundaries.

### 6. Design Contracts from the Caller

Define the smallest contract the caller needs:

- use domain language;
- make success and failure explicit;
- expose ownership, nullability, ordering, idempotency, threading, and cancellation when relevant;
- avoid leaking provider models, persistence entities, transport envelopes, or framework callbacks;
- avoid generic maps and flag combinations when a typed request or policy expresses the concept.

Do not derive an interface by copying every public method from an implementation.

### 7. Apply the Pattern Gate

Start with a direct implementation, language feature, or standard-library abstraction.

Before applying a design pattern, answer:

1. What observed problem does it solve?
2. What real or credible change makes the direct design costly?
3. Why is the simpler alternative insufficient?
4. Which stable code does the pattern protect?
5. What types, indirection, lifecycle, ordering, and debugging cost does it add?
6. When should the pattern be simplified or removed?

When `$java-design-patterns` is available, use it for pattern-specific selection and implementation guidance. Supply the business problem and variation axis; do not ask it to choose from structure alone.

Always state why the chosen pattern fits and why the closest alternatives do not. If no pattern clears the gate, keep the direct design.

### 8. Implement or Refactor Incrementally

Protect current behavior before moving a boundary. Introduce one seam, redirect one responsibility, verify it, then remove the old path.

Define every compatibility bridge with:

- reason;
- owner;
- allowed callers;
- removal condition;
- verification needed before deletion.

Read [refactoring-workflow.md](references/refactoring-workflow.md) for legacy changes, migrations, public compatibility, or dual implementations.

### 9. Verify and Clean Up

Verify in this priority order:

1. business behavior and data safety;
2. state transitions, concurrency, and lifecycle;
3. failure and cancellation semantics;
4. dependency and information boundaries;
5. tests and observability;
6. readability and removal of obsolete paths.

Do not leave two authoritative paths, commented-out implementations, silent fallbacks, or ownerless state.

## Apply UI Responsibility Rules

Read [ui-responsibilities.md](references/ui-responsibilities.md) whenever work includes screens, controllers, presenters, view models, components, adapters, navigation, or user-visible state.

Read [android-application-guide.md](references/android-application-guide.md) only for Android or Java/Kotlin platform work. Keep the framework-specific advice subordinate to the general responsibility model.

## Review with Explicit Severity

Read [review-rubric.md](references/review-rubric.md) for architecture reviews and before finalizing a non-trivial implementation.

Read [anti-patterns.md](references/anti-patterns.md) when code contains broad managers, pass-through layers, direct persistence access, global event systems, default implementations, swallowed errors, long-lived bridges, or pattern-heavy designs.

Lead reviews with actionable findings:

- correctness and data loss;
- state, concurrency, and lifecycle;
- responsibility and information leaks;
- failure semantics;
- testability and compatibility;
- unnecessary abstraction and cleanup.

## Produce the Required Output

For non-trivial work, include:

- business goal and invariants;
- actors, states, actions, and failures;
- responsibility topology and authoritative owners;
- allowed and forbidden information at each boundary;
- real variation axes and stable contracts;
- direct solution versus pattern trade-off;
- incremental implementation or refactoring sequence;
- compatibility bridges and removal conditions;
- tests and acceptance criteria;
- remaining debt and explicit non-goals.

Keep the output proportional. A small local change may need only a compact responsibility statement, the chosen direct design, and focused verification.

## Create a Standalone Agent

When the user asks for a dedicated RDD agent, use [responsibility-driven-development-agent.md](assets/responsibility-driven-development-agent.md) as the standalone system prompt.

Copy its engineering contract without adding project-specific architecture. Adapt only the target platform's agent metadata, tool declarations, and permission model.

## Preserve Scope and Clarity

- Correct unsafe or structurally misleading existing approaches instead of canonizing them as style.
- Prefer explicit collaboration over hidden global lookup.
- Prefer one authoritative state owner over synchronization among copies.
- Prefer visible workflow orchestration over event-driven control flow.
- Prefer domain-specific contracts over broad `Manager`, `Helper`, `Util`, or `Common` surfaces.
- Preserve external API, serialization, schema, protocol, and generated boundaries unless the task authorizes migration.
- Use `$language-coding-style` and `$naming-conventions` when available for language and identifier details; do not duplicate their rules here.
