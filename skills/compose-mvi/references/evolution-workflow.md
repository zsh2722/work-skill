# Evolution Workflow

Use this workflow to learn through small changes while feature development continues. Adapt or skip steps according to risk.

## Observe

Start from a recent or upcoming change. Note where the team spends effort:

- searching for the right branch;
- understanding side effects;
- coordinating state changes;
- working around lifecycle or race behavior;
- modifying unrelated code;
- duplicating tests or setup;
- resolving merge conflicts.

Concrete friction gives the refactor a purpose.

## Reconstruct

Trace representative paths through UI, intent/method, handler, dependency, and state. Include a successful case, a failure, and a timing-sensitive case when relevant.

A lightweight inventory may help:

| Input or change | Meaning | Current owner/path | State/dependencies involved | Timing/lifecycle notes |
|---|---|---|---|---|

Use the table to think, not as mandatory documentation.

## Form a Hypothesis

Phrase the architectural move as a testable hypothesis:

> If overlay configuration is treated as one capability, adding a new overlay option should require understanding only its state and handler, without touching capture or analysis behavior.

> If frames bypass the user-event dispatcher, interaction latency and memory behavior should become independent of analysis throughput.

The hypothesis keeps structure connected to an outcome.

## Compare Moves

Consider a few moves of different sizes:

- clarify names or comments;
- reorganize types without behavioral change;
- extract a responsibility;
- change state ownership;
- separate an asynchronous pipeline;
- split a feature boundary.

Compare expected clarity, behavioral risk, compatibility cost, test effort, and reversibility. The smallest move is not always best, but it is often the cheapest way to learn.

## Integrate with Feature Work

Look for a refactoring slice adjacent to the feature being changed. Improve the path that the team already needs to understand.

Keep intermediate states coherent. When temporary compatibility is useful, make its purpose and removal signal visible without demanding a universal deprecation mechanism.

Avoid running old and new state ownership models indefinitely. Transitional duplication is most dangerous when both paths can make independent decisions.

## Observe the Result

After the change, ask:

- Did the target feature become easier to locate and reason about?
- Did fewer unrelated concepts need to change?
- Did testing become more focused?
- Did new delegation or synchronization appear?
- Did runtime behavior remain correct?
- What did the change reveal about the next boundary?

Architecture evolves through feedback. A successful extraction may reveal a larger capability; an awkward one may be evidence to collapse or regroup it.

## Possible Incremental Sequences

There is no fixed order. Depending on the risk, a team might:

- clarify vocabulary before moving code;
- protect behavior with tests before changing concurrency;
- separate a high-risk stream before reorganizing low-risk user actions;
- group the area touched by the next feature and leave stable areas alone;
- first make state ownership explicit, then reconsider event structure;
- reorganize definitions first when discoverability is the only problem.

Choose the sequence that maximizes learning and keeps delivery safe enough for the product context.
