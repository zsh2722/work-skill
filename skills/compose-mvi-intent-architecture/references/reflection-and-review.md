# Reflection and Review

Use relevant questions rather than applying the whole list mechanically.

## Understanding

- Can a reader trace an important interaction from UI to result?
- Do names expose product meaning at the appropriate boundary?
- Is complexity visible where it belongs, or displaced into generic plumbing?
- Does the structure help with the changes the team actually makes?

## State and Decisions

- Is it clear which component decides each transition?
- Can related values become temporarily inconsistent?
- Is duplicate truth intentional and synchronized for a reason?
- Are derived values stored because computation or persistence justifies it?
- Does state describe what the UI needs, or leak internal machinery?

## Events and Effects

- Are inputs modeled at a useful semantic level?
- Are UI behavior, business decisions, external updates, and internal results distinguishable where that distinction matters?
- What should happen if collection stops temporarily?
- Does an effect need delivery guarantees that its mechanism does not provide?
- Would state, an explicit callback, or a platform boundary express the behavior more naturally?

## Time and Resources

- What can race, repeat, queue, or arrive late?
- Is cancellation part of correctness or only an optimization?
- Can a high-rate producer increase memory use without bound?
- Who releases frames, bitmaps, handles, or subscriptions on success, failure, and cancellation?
- Does lifecycle ownership match the lifetime of the work?

## Boundaries and Dependencies

- Does each boundary reduce knowledge, or just forward calls?
- Are Android/platform details present because they are useful here or because translation was deferred accidentally?
- Are multiple state holders genuinely independent?
- Does a proposed interface represent a real variation or only mirror an implementation?

## Evolution

- What observed problem does the change address?
- Which alternative was considered?
- What new concepts and maintenance costs are introduced?
- Can the move be learned from or reversed?
- What future signal would suggest splitting, regrouping, or simplifying again?

## Verification

Choose evidence proportional to uncertainty:

- unit tests for state rules and handler decisions;
- coroutine/flow tests for ordering, repetition, cancellation, and lifecycle gaps;
- Compose tests for rendering and callback wiring;
- instrumentation for platform/lifecycle integration;
- traces, metrics, or benchmarks for responsiveness and throughput;
- code-change observation for cohesion and navigation improvements.

Focus tests on behavior and risk rather than the chosen class arrangement. A refactor should remain free to evolve without rewriting tests that merely mirror private dispatch structure.

## Communicating Findings

Separate demonstrated risks from design preferences.

For a correctness issue, describe the triggering scenario and consequence directly. For a structural concern, explain the friction it causes, the contextual trade-off, and plausible options. Avoid presenting one preferred MVI vocabulary as universally correct.
