# Modularization Decision Guide

## Contents

- Package or Gradle module
- Boundary signals
- Common module roles
- API and implementation split
- Design system boundaries
- Migration sequence
- Failure patterns

## Package or Gradle Module

Use a package when the main need is discoverability and nearby code ownership. Use a Gradle module when build-time dependency enforcement, separate compilation, reuse, variants, delivery, or team ownership provides enough value to pay configuration and dependency-graph costs.

Before extraction, ask:

- What dependency becomes impossible or explicit after this split?
- What public contract will consumers use?
- Who owns the module?
- Can it be tested independently in a useful way?
- Does it need Android resources or can it be Kotlin/JVM?
- What new coordination, DI, build configuration, and navigation work appears?

If the answers are vague, improve packages and visibility first.

## Boundary Signals

Signals favoring a split:

- a capability changes for reasons unrelated to its neighbors;
- multiple apps or features consume a stable contract;
- a team can own and release work behind that contract;
- implementation changes cause expensive downstream recompilation;
- variants or tests need interchangeable implementations;
- architecture rules cannot be maintained by visibility and packages alone.

Signals favoring keeping or merging:

- modules expose many internals to make ordinary changes possible;
- two modules change together almost every time;
- a module contains only forwarding code;
- DI and mapping dominate the business code;
- circular dependencies require a growing mediator;
- names describe generic technical buckets rather than responsibilities.

## Common Module Roles

Feature boundaries usually follow user capabilities or cohesive journeys. Data boundaries usually follow data/business domains, not screens. Therefore, do not duplicate one repository per feature when several features operate on the same authoritative data.

Core is a location category, not a license for arbitrary sharing. Prefer `core:analytics`, `core:network`, or `core:designsystem` over a large `core:common`. Keep feature-specific abstractions in their feature until another genuine consumer exists.

The app module is the composition root. It may know feature implementations in order to assemble the application; features should not depend back on it.

## API and Implementation Split

Split API from implementation when at least one applies:

- consumers need a small stable contract but not implementation dependencies;
- multiple implementations exist for builds, apps, or tests;
- features need navigation contracts without implementation coupling;
- independent teams need an explicit integration boundary;
- implementation churn materially harms incremental builds.

Do not split merely because a large sample does. A single module with `internal` implementation is often clearer until the contract stabilizes.

## Design System Boundaries

A design system can contain theme, typography, colors, shapes, spacing policy, icons, and reusable branded primitives. It should define a deliberate visual API rather than wrap every Material component mechanically.

Keep model-independent primitives in the design system. Put composite widgets that render application/domain models in a shared UI or domain-specific module when that keeps the design system dependency-light.

Tokens are useful when the design organization actually governs those values or expects coordinated changes. Do not replace every literal with a token without semantic meaning; local layout values may remain local.

## Migration Sequence

1. Capture the current dependency graph and a representative change path.
2. Define one target boundary and its smallest public contract.
3. Move or copy one vertical slice while keeping behavior stable.
4. Redirect consumers and enforce visibility/dependency rules.
5. Run feature, navigation, integration, and build checks.
6. Remove the old path after all consumers migrate.
7. Compare maintenance and build evidence before extracting the next module.

Prefer reversible steps and avoid mixing module extraction with behavioral redesign unless the task requires both.

## Failure Patterns

- **Everything is a feature**: obscures shared data domains and platform capabilities.
- **One screen, one module**: creates configuration and graph overhead without independent responsibility.
- **Feature-owned repository by default**: duplicates sources of truth and couples data modeling to presentation.
- **Mandatory use case per call**: adds pass-through layers; add domain logic when complexity or reuse exists.
- **Universal event/effect folders**: confuses screen-state conventions with project modularization.
- **God core/common module**: recreates the monolith at a lower level.
- **Premature API/impl split**: freezes unstable contracts and multiplies modules.
- **Directory-tree-first design**: produces a neat tree without a coherent dependency graph.
