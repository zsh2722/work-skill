# Review Rubric

## Contents

1. Review order and severity
2. Correctness
3. Responsibility and information boundaries
4. State, concurrency, and lifecycle
5. Contracts and failures
6. Abstraction and pattern cost
7. Refactoring completeness
8. Tests and observability
9. Finding format

## 1. Review Order and Severity

Review in this order:

1. business correctness and data safety;
2. state, concurrency, and lifecycle;
3. responsibility ownership and information leaks;
4. contracts and failure semantics;
5. compatibility and migration;
6. abstraction and pattern cost;
7. testability, observability, and clarity.

Use severity:

- **P0**: data loss, security failure, unrecoverable corruption, or critical production outage.
- **P1**: incorrect business behavior, race, lifecycle leak, build/runtime break, public incompatibility, or hidden required failure.
- **P2**: responsibility leak, unstable dependency, ambiguous ownership, incomplete migration, or unjustified abstraction with material maintenance cost.
- **P3**: localized clarity or simplification improvement that is safe to defer.

Do not report personal pattern preferences as P1 or P2 without a concrete failure or change cost.

## 2. Correctness

Check:

- Are business invariants explicit and enforced by one owner?
- Can invalid state transitions occur?
- Are authorization and eligibility evaluated before effects?
- Is partial failure handled across multiple effects?
- Are retries idempotent where duplication is harmful?
- Can cancellation be mistaken for success or failure?
- Are fallbacks allowed by the business contract?
- Can stale results overwrite newer state?

## 3. Responsibility and Information Boundaries

Check:

- Does each decision have one authoritative owner?
- Does UI code decide business rules?
- Does business code import framework, provider, persistence, or transport details?
- Does an adapter coordinate unrelated workflows?
- Do callers bypass a repository or capability port?
- Does a broad manager change for unrelated reasons?
- Are dependencies visible at construction/composition time?
- Can the responsibility be explained without naming its framework?

## 4. State, Concurrency, and Lifecycle

Check:

- Is mutable state owned in one place?
- Are legal transitions defined?
- Are multiple observable values able to form impossible combinations?
- Are thread, dispatcher, ordering, and serialization rules explicit?
- Are listeners registered and removed by the same lifetime owner?
- Are background tasks cancelled or intentionally retained?
- Can duplicate commands run concurrently?
- Are process-wide objects genuinely process-wide?

## 5. Contracts and Failures

Check:

- Is the contract designed from caller needs?
- Does it leak implementation models?
- Are null, empty, false, and default values unambiguous?
- Are business rejection, dependency failure, timeout, conflict, and cancellation distinguishable where needed?
- Are exception causes and diagnostics preserved?
- Are unsupported/default implementations observable?
- Are idempotency, side effects, and ownership documented when non-obvious?

## 6. Abstraction and Pattern Cost

Check:

- What concrete variation justifies the abstraction?
- Is there more than one real implementation or a credible migration?
- Would a direct function, conditional, sealed type, or standard-library feature be clearer?
- Does the pattern protect stable code?
- Does it introduce hidden order, lifecycle, or global state?
- Is the interface only a copy of one implementation?
- Is a factory, facade, mediator, manager, or registry accumulating business behavior?
- Is there a condition for simplifying or removing the abstraction?

If a pattern is proposed, require:

- observed problem;
- direct alternative;
- selected pattern;
- rejected closest alternatives;
- complexity cost;
- verification that a new variant leaves stable code unchanged.

## 7. Refactoring Completeness

Check:

- Is current behavior characterized?
- Is the target ownership explicit?
- Does each compatibility bridge have a removal condition?
- Is there one authoritative write path?
- Do old entry points still accept direct use?
- Are old models, routes, flags, or adapters still referenced?
- Did the change mix unrelated formatting, migration, or feature work?
- Is cleanup included in the same migration milestone?

## 8. Tests and Observability

Check:

- Do tests assert business behavior rather than implementation calls only?
- Are invalid transitions and boundary failures covered?
- Are adapter contract tests present where external translation matters?
- Are concurrency, cancellation, and lifecycle paths tested?
- Are migrations and partial failures tested?
- Can operators distinguish business rejection from technical failure?
- Are fallback and bridge usage observable?

## 9. Finding Format

Write each finding as:

```text
[Severity] Short title
Evidence: exact file, symbol, behavior, or flow
Problem: violated responsibility, invariant, or boundary
Impact: concrete correctness or maintenance cost
Correction: smallest coherent fix
Verification: test or check that proves the fix
```

Lead with findings. If there are no findings, state that and list checks or runtime paths not verified.

For design work without code, use:

```text
Decision:
Business pressure:
Direct option:
Patterned option:
Chosen approach:
Rejected alternatives:
Added complexity:
Exit/simplification condition:
```
