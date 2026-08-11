# Design Lenses

Use these lenses to explore a design. They are prompts, not acceptance gates.

## Meaning

- What product decision or occurrence does the input represent?
- Is its current name tied to a widget callback or to enduring behavior?
- Would a future reader understand why it exists without opening the UI code?
- Is it one concept or a bundle of unrelated flags and platform objects?

Names such as `SelectEngine` often reveal more intent than `OnDropdownChanged`, but callback-oriented names can still be appropriate for UI-local behavior.

## Ownership

- Who has enough knowledge to decide the outcome?
- Where is the authoritative version of the state?
- Are multiple components coordinating copies of the same truth?
- Is ownership aligned with lifecycle and resource lifetime?

Centralization can make consistency easier; distribution can make independent parts simpler. The important question is whether coordination cost matches the relationships in the product.

## Cohesion

- Which inputs, rules, state, and dependencies tend to change together?
- What must be understood together to make a safe modification?
- Does a proposed group have its own language and focused scenarios?
- Does extraction reduce reasoning, or merely add delegation?

Possible groupings include business capability, input source, workflow stage, policy, or UI responsibility. No axis is universally correct.

## Information Flow

- Where is raw input translated into meaning?
- Which details cross the UI, state-holder, domain, and data boundaries?
- Does a boundary expose more information than its consumer needs?
- Can the flow be followed without hidden global communication?

UDF offers a useful default direction, but local components and external producers may have their own smaller flows.

## Time

- Can work overlap or finish out of order?
- Can the producer outrun the consumer?
- May inputs be lost, merged, replayed, or repeated?
- What should happen when the screen is not visible?
- Which resources need explicit release?

Type hierarchies do not express these answers automatically. Make temporal assumptions visible in code, tests, or nearby documentation when they matter.

## Change

- What recent features made the current shape harder to work with?
- Which areas are likely to change for evidenced reasons?
- Does a proposed abstraction protect a stable rule from a real variation?
- What is the cost if the predicted change never arrives?

Prefer evidence from history and planned work over imagined extensibility.

## Choosing an Interaction Shape

### Typed methods may fit when

- the screen is small or actions are independent;
- call-site discoverability matters more than unified dispatch;
- event values do not need common processing.

### A flat sealed type may fit when

- exhaustive handling is useful;
- the event set is still coherent and navigable;
- logging, replay, testing, or adapters benefit from values.

### A nested hierarchy may fit when

- recognizable capabilities already exist;
- each group improves navigation and focused reasoning;
- top-level exhaustiveness remains valuable.

### Separate state holders may fit when

- state, lifecycle, dependencies, or reuse are meaningfully independent;
- coordination between the parts is limited and explicit.

### A separate stream/processor may fit when

- input is continuous or high frequency;
- buffering, sampling, cancellation, or resource lifetime dominates its design;
- treating each value as a human-scale action obscures its nature.

These options can coexist. Choose asymmetry when the problem is asymmetric.
