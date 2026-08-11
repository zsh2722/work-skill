---
name: compose-modularization
description: Design, review, or evolve package structure, Gradle module boundaries, public APIs, and dependency direction for Android projects that use Jetpack Compose. Use when code is hard to find, features span technical-layer packages, shared UI or utilities are growing, feature/core/data/domain/design-system modules are being considered, build or team boundaries need improvement, or a project needs an incremental modularization plan without copying Now in Android as a fixed template.
---

# Compose Modularization

Treat directories and Gradle modules as tools for ownership, isolation, and change—not as proof of architectural maturity. Compose affects UI composition, but it does not determine one project structure.

## Inspect the Existing Project

Before proposing a tree:

- Read `settings.gradle[.kts]`, module build files, convention plugins, version catalogs, source sets, navigation composition, dependency injection setup, and representative tests.
- Map the current Gradle dependency graph and note cycles, broad `api` exposure, Android-only modules, and frequently changed shared modules.
- Trace two representative feature changes. Record which packages and modules change together, which teams own them, and where code is difficult to find.
- Separate package organization problems from true module-boundary problems. A package move is cheaper and may be sufficient.
- Establish constraints: app size, team ownership, build times, delivery model, variants, reusable apps/platforms, dynamic delivery, and migration tolerance.

Do not infer the needed granularity from file count, screen count, or Compose usage alone.

## Choose Boundaries from Evidence

Prefer high cohesion and low coupling. A candidate boundary should have a clear responsibility, a small public surface, and a credible reason to change independently.

Use these common roles as options:

- **App module**: application entry point, app-level scaffolding, composition root, and root navigation.
- **Feature package/module**: a user capability or cohesive journey, often containing UI and its state holder. A feature can span multiple destinations and need not own its data implementation.
- **Data module**: repository API plus related data sources and models for a coherent data domain; hide data-source details.
- **Core/common module**: narrowly named capability reused across otherwise unrelated modules. It is not a dumping ground or an architectural layer.
- **Design-system module**: theme, tokens, icons, and reusable primitives whose visual contract is intentionally shared.
- **Shared UI module**: composite UI tied to application models. Keep it distinct from a model-independent design system when that dependency matters.
- **Domain layer/use cases**: add for complex business logic or logic reused across state holders. It is optional; do not wrap every repository call by default.
- **Test/build-logic modules**: introduce when shared fixtures, integration boundaries, or configuration consistency justify them.

Read [decision-guide.md](references/decision-guide.md) when selecting module roles, splitting API/implementation, or assessing migration signals.

## Design the Dependency Graph First

Draw the intended graph before drawing the directory tree.

Default constraints:

- Keep feature-to-feature implementation dependencies out of the graph.
- Let the app or another explicit coordinator mediate cross-feature journeys.
- Pass stable keys or IDs across navigation boundaries instead of rich data objects when the receiving feature can load authoritative data.
- Make core modules independent of feature and app modules.
- Expose the smallest useful API; prefer `implementation` over `api` unless consumers need the transitive type.
- Use `private` and `internal` to enforce boundaries inside modules.
- Use dependency inversion and API/implementation modules only when interchangeability, multiple apps, independent ownership, test substitution, or recompilation isolation earns the overhead.
- Prefer Kotlin/JVM modules when Android resources or framework APIs are unnecessary.
- Reject cycles. If two proposed modules continually require each other's internals, reconsider the boundary before adding indirection.

Do not assume the current Now in Android graph is a universal target. Use it as one documented solution to its goals and constraints.

## Organize Inside a Feature

Keep feature-local code local. Start with the shallowest structure that remains navigable.

Group by cohesive capability or screen when technical-type folders force one change across many distant locations. Introduce `ui`, `navigation`, `data`, or similar subpackages only when each contains enough related code and its ownership is clear.

Do not require every feature to contain `repository`, `domain`, `state`, `event`, `effect`, or `di` packages. Repositories normally represent data domains and may be shared by features. State/event/effect shapes belong to the screen architecture decision; use `$compose-mvi` when that is the actual problem.

Avoid broad names such as `utils`, `common`, `base`, and `extensions`. Name shared code by the capability or policy it provides, and keep one-off helpers near their caller.

## Scale Incrementally

Choose the least expensive structure that resolves observed friction:

- For a small app or early product, keep one Gradle module and organize packages by feature or capability.
- Extract a module when there is a real dependency boundary, independent ownership/reuse, useful test seam, build-performance benefit, or delivery requirement.
- Add shared/core modules only after at least two consumers demonstrate a stable shared contract, except for deliberate platform foundations such as a design system.
- Split `api` and `impl` after the API is meaningful and implementation isolation has measurable value.
- Centralize repeated Gradle configuration with convention plugins and dependency versions with a version catalog as module count grows.

Migrate one vertical slice at a time. Preserve behavior, introduce the target boundary, redirect consumers, verify, then remove obsolete paths. Avoid a repository-wide package move unless the user explicitly accepts the coordination cost.

## Validate the Result

Check more than compilation:

- the Gradle graph is acyclic and matches the stated dependency rules;
- public declarations and `api` dependencies are minimal;
- feature tests can use fakes without depending on implementation details;
- navigation/deep links and state restoration still work;
- representative changes touch a smaller, more coherent area;
- clean and incremental build measurements support any build-performance claim;
- module count and configuration overhead remain proportionate;
- dependency rules are automated when violations would otherwise recur.

## Produce a Contextual Recommendation

For architecture work, provide:

1. observed friction and constraints;
2. current and proposed dependency graphs;
3. responsibility and public API of each proposed module;
4. alternatives, including staying single-module;
5. incremental migration sequence;
6. validation plan and signals that would justify further splitting or merging.

Label project-specific choices as choices. Do not present a sample repository, team-size threshold, directory tree, or design-token wrapper as an official universal requirement.

Read [official-foundations.md](references/official-foundations.md) before making claims about Google's recommendations or Now in Android.
