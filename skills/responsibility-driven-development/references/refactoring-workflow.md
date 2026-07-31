# Refactoring Workflow

## Contents

1. Define the target behavior
2. Characterize the current system
3. Choose one boundary move
4. Introduce a seam
5. Redirect callers
6. Control compatibility bridges
7. Delete the old path
8. Verify and report

## 1. Define the Target Behavior

Before editing, state:

- behavior that must remain;
- behavior intentionally changed;
- business invariants;
- public compatibility constraints;
- data migration constraints;
- performance or availability constraints;
- explicit non-goals.

Do not use “clean architecture” as a target. Name the responsibility or information leak being corrected.

## 2. Characterize the Current System

Use tests, call sites, logs, schemas, and production contracts to discover actual behavior.

Mark findings as:

- **required behavior**;
- **legacy compatibility**;
- **accidental behavior**;
- **known defect**;
- **unknown**.

Do not preserve a known defect merely because a characterization test captured it. Preserve it only when compatibility requires a staged correction, and document the exit.

### Find Boundary Smells

- UI or transport code decides business eligibility.
- Business code imports framework, database, or provider models.
- Several components mutate the same state.
- Callers bypass the intended repository or service.
- Global events coordinate a transaction.
- A broad manager owns unrelated workflows.
- A fallback converts failure into apparent success.
- Old and new implementations both accept writes.

## 3. Choose One Boundary Move

Select one coherent change:

- move one rule to its authoritative owner;
- introduce one port around an external capability;
- translate one external model at the boundary;
- move workflow order into one coordinator;
- establish one state owner;
- separate one UI effect from durable state;
- isolate one platform or provider variation.

Avoid combining broad renames, language migrations, framework migrations, formatting, and behavior changes unless they are inseparable.

## 4. Introduce a Seam

Create the smallest seam that allows the old and new structures to coexist temporarily.

Useful seams include:

- a caller-owned interface;
- a typed request/result;
- an adapter around a static or external API;
- a wrapper around time, randomness, filesystem, or process communication;
- a translator between old and new models;
- a coordinator that initially delegates to old behavior.

Reject a seam that merely moves code without changing ownership or information flow.

## 5. Redirect Callers

Redirect one caller group at a time.

For each group:

1. capture current behavior;
2. route through the new owner;
3. verify success, failure, cancellation, and lifecycle paths;
4. search for bypasses;
5. keep only one writable source of truth.

Prefer a narrow vertical slice over building all new layers before any behavior uses them.

## 6. Control Compatibility Bridges

Every bridge must record:

```text
Reason:
Owner:
Allowed callers:
Behavior preserved:
Removal condition:
Removal verification:
Deadline or milestone:
```

Examples of bridges:

- old API delegating to a new use case;
- old data model translated at a boundary;
- callback API wrapping a new asynchronous result;
- old storage read with new storage write during migration.

### Dual-Write Rule

Avoid dual writes. When unavoidable:

- define which store is authoritative;
- define failure ordering and recovery;
- make writes idempotent;
- instrument divergence;
- bound the migration period;
- test partial failure.

Never leave two equal authorities.

## 7. Delete the Old Path

Remove:

- old entry points;
- bypass calls;
- obsolete adapters;
- stale feature flags;
- dead model translations;
- commented-out implementations;
- compatibility tests that no longer represent supported behavior.

Keep migration tests that protect persisted or public compatibility.

Search the repository for old symbols, routes, configuration keys, and direct dependencies before declaring completion.

## 8. Verify and Report

Verify proportionally:

- focused unit tests for rules and state transitions;
- contract tests for ports and adapters;
- integration tests for transaction and process boundaries;
- lifecycle and cancellation tests;
- migration tests for persisted or public data;
- UI tests for rendering and one-time effects;
- static dependency checks or searches for bypasses.

Report:

- responsibility moved;
- old owner and new owner;
- behavior preserved or intentionally changed;
- compatibility bridge and removal condition;
- checks run;
- known gaps;
- obsolete path removed.

### Stop Conditions

Stop and revisit the design when:

- the new owner needs most internals of the old owner;
- mapping spreads across many callers;
- a seam requires global mutable state;
- failures become less precise;
- lifecycle ownership becomes ambiguous;
- tests require mocking many unrelated collaborators;
- the “temporary” bridge becomes the easiest public entry point.
