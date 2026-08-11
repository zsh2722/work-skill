---
name: compose-mvi
description: Think through, review, and evolve Jetpack Compose screen architecture using MVI, UDF, state holders, events, intents, reducers, and handlers as adaptable design tools. Use for Kotlin/Android/Compose work when event definitions, state handling, ViewModels, or feature boundaries have become difficult to understand; when deciding whether to group, split, simplify, or retain an existing Intent design; or when planning gradual architecture improvements without imposing a rigid framework or disruptive rewrite.
---

# Compose MVI

Use MVI and UDF as ways to reason about change, ownership, and information flow—not as templates that every screen must resemble.

The goal is not to produce a particular hierarchy. The goal is to make the next change easier to understand without making the current system unnecessarily elaborate.

## Begin with the Situation

Read the code, requirements, tests, failure history, runtime behavior, and team context before proposing structure.

Ask:

- What has actually become difficult: finding code, understanding behavior, testing, coordination, concurrency, or extension?
- Which complexity comes from the business, and which was introduced by the implementation?
- What changes frequently? What remains stable?
- Which decisions must stay consistent with one another?
- What does the team need to understand quickly during ordinary feature work?
- What constraints matter now: delivery pace, compatibility, lifecycle, performance, or ownership?

Do not treat file length, Intent count, or `when` branch count as defects by themselves. They are signals to investigate.

## Build a Mental Model

Trace one representative interaction end to end:

```text
something happens
    → meaning is interpreted
    → work or policy is applied
    → state changes
    → UI reflects the result
```

Then trace a failure, a repeated action, and an asynchronous update. Notice where meaning becomes unclear, state has multiple owners, or timing changes the outcome.

Use these dimensions to describe the design:

- **Meaning**: what does this input express in product language?
- **Ownership**: who is allowed to decide and change the result?
- **Cohesion**: which behaviors and state must be understood together?
- **Flow**: how do information and decisions travel?
- **Time**: what can overlap, repeat, arrive late, or outlive the UI?
- **Boundary**: which platform, UI, domain, or data details cross layers?
- **Change**: which parts evolve together in real development?

These are lenses, not categories every code element must fit perfectly.

## Choose Structure as a Response

Consider several shapes before settling on one:

- direct typed methods;
- a small flat event type;
- a nested hierarchy organized by meaningful capability;
- separate handlers behind one screen state holder;
- multiple cooperating state holders;
- an external stream or processor beside user interaction handling;
- retaining the current shape with better naming or navigation.

Compare them by the problem they solve and the cost they add. Prefer the smallest change that makes an important relationship visible.

Nested `sealed interface` types are useful when they reveal stable subdomains and make definitions, dispatch, and tests easier to navigate. They are less useful when they only create folders inside a hierarchy while every branch still depends on everything else.

Do not force symmetry. One capability may deserve a hierarchy, another a method, and another an observed data stream.

Read [design-lenses.md](references/design-lenses.md) when choosing among these shapes.

## Let Boundaries Emerge from Cohesion

Look for behavior that shares language, state, dependencies, invariants, timing, or reasons to change. Treat that cluster as a candidate boundary, then test the boundary against real scenarios.

A useful boundary tends to:

- reduce what must be held in mind for a change;
- give decisions and state a recognizable owner;
- keep closely related rules together;
- limit the number of unrelated dependencies touched;
- allow focused verification;
- remain understandable at call sites.

A boundary is suspect when it creates forwarding layers, duplicate state, synchronization work, or names such as `Basic`, `Common`, and `Misc` without a precise local meaning.

Use business language when it clarifies the model. Use technical language when the concern is genuinely technical. Avoid inventing domains merely to make the type tree balanced.

## Treat State, Events, and Effects as Perspectives

Distinguish them by how the application needs to reason about them:

- **State** describes information the UI or business logic needs to know now.
- **An event or intent** expresses something that happened or was requested.
- **An effect** describes work or UI behavior that is not naturally represented as current state.

The boundaries are contextual. A message may be transient in one product and durable in another. Navigation may be local UI behavior or the visible consequence of an important workflow transition.

Ask what must survive recomposition, temporary collection gaps, configuration changes, process recreation, or leaving the screen. Let those product expectations guide representation and ownership.

Avoid arguing from the name of a primitive such as `Channel`, `SharedFlow`, or `StateFlow`. Examine its actual delivery, replay, cancellation, and buffering behavior in this use case.

Read [official-foundations.md](references/official-foundations.md) when relating a proposal to Android guidance. Present official UDF recommendations as foundations that permit multiple implementations, not proof of one MVI style.

## Include Time in the Architecture

For asynchronous or high-frequency work, architecture includes temporal behavior as much as type structure.

Explore:

- whether every input matters or only the latest;
- what repeated user action means;
- what happens when old work completes after new work;
- where cancellation belongs;
- whether producers can outrun consumers;
- what resources are retained while work waits;
- how failure changes subsequent processing.

Do not automatically route every callback through the same Intent mechanism. A user decision, a camera frame, and a repository update may deserve different paths because they carry different meaning and timing. They may also share a path when uniform ordering or observability is genuinely valuable.

Choose operators and queues only after deciding the desired product behavior.

## Evolve the Design While Shipping

Treat architecture as a series of reversible learning steps.

Find one cohesive change that can improve the model while ordinary feature work continues. Preserve behavior where it matters, make the new relationship visible, verify it, and remove obsolete structure when confidence is sufficient.

Possible moves include:

- rename an event to expose meaning;
- move one unrelated input out of a general dispatcher;
- gather a cohesive set of events under a shared concept;
- extract a handler where it creates a real responsibility boundary;
- clarify the owner of a state transition;
- replace duplicated truth with a derived value;
- document concurrency behavior near the code;
- split a state holder after independent lifecycles or change patterns become evident;
- collapse an abstraction that no longer earns its cost.

The order depends on risk, current feature work, test coverage, and team familiarity. Do not prescribe a universal migration sequence.

Read [evolution-workflow.md](references/evolution-workflow.md) for prompts that help select the next safe learning step.

## Verify the Idea, Not the Diagram

Use tests and runtime evidence to answer the uncertainty that motivated the change.

Examples:

- If ownership was unclear, verify that one path now determines the transition.
- If concurrency was risky, exercise overlap, cancellation, and stale completion.
- If grouping should improve cohesion, observe whether a feature change now touches a smaller, more meaningful area.
- If an external stream was separated, measure responsiveness, buffering, and resource use.
- If an effect changed representation, verify the expected lifecycle experience.

Testing depth should follow risk. A naming-only refactor and a frame-processing redesign do not need the same ceremony.

Read [reflection-and-review.md](references/reflection-and-review.md) for adaptable review questions.

## Produce a Contextual Recommendation

For substantial work, explain:

- the concrete friction observed;
- the mental model reconstructed from code;
- forces and constraints shaping the decision;
- plausible alternatives;
- the proposed direction and why it fits now;
- costs, uncertainties, and signals that would invalidate it;
- one or more incremental next moves;
- evidence that would increase confidence.

Use language such as “consider,” “prefer when,” “a useful signal,” and “in this context.” Be decisive about demonstrated correctness risks, but keep structural preferences open to local trade-offs.

Do not introduce reducers, middleware, effect buses, use cases, base classes, or nested hierarchies simply because they are associated with MVI. Do not reject them merely because a simpler design exists; use them when their benefits are visible in the system being changed.
