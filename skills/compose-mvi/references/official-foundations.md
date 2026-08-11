# Official Android Architecture Foundations

Use this reference to distinguish Android's official guidance from project-specific MVI conventions. Treat the guidance as a set of forces and defaults to interpret in context, not as a compliance checklist.

## Source Map

- [Guide to app architecture](https://developer.android.com/topic/architecture): separation of concerns, layered architecture, single source of truth, and unidirectional data flow.
- [Recommendations for Android architecture](https://developer.android.com/topic/architecture/recommendations): current prioritized recommendations for Compose, UI state, ViewModels, coroutines, flows, repositories, and lifecycle-aware collection.
- [Compose UI architecture](https://developer.android.com/develop/ui/compose/architecture): events flow up, state flows down, state hoisting, state holders, and ViewModel integration.
- [UI layer](https://developer.android.com/topic/architecture/ui-layer): immutable UI state, state production, state holders, and UDF responsibilities.
- [UI events](https://developer.android.com/topic/architecture/ui-layer/events): user events, UI-owned behavior, ViewModel handling, and why durable outcomes should normally be represented in state.
- [State holders and UI state](https://developer.android.com/topic/architecture/ui-layer/stateholders): business-logic versus UI-logic state holders and their lifetimes.
- [UI state production](https://developer.android.com/topic/architecture/ui-layer/state-production): event sources, state production pipelines, observable state, and lifecycle/resource considerations.

Recheck these pages when the task depends on current API recommendations. Android guidance evolves.

## What Is Officially Recommended

- Follow UDF: UI state flows down; events/actions flow up.
- Use a screen-level ViewModel when it provides lifecycle and business-state benefits.
- Keep reusable UI components independent of screen ViewModels; use hoisted state or plain state holders.
- Expose immutable observable UI state, commonly `StateFlow`.
- Collect state lifecycle-aware in Compose, commonly with `collectAsStateWithLifecycle`.
- Keep ViewModels independent of `Activity`, `Fragment`, `Context`, `Resources`, and UI lifecycle objects.
- Communicate across layers with coroutines and flows.
- Process business events promptly and update state; avoid relying on lossy ViewModel-to-UI event delivery for durable outcomes.
- Adapt recommendations to the app rather than treating them as rigid rules.

## What Is Not Mandated

Android guidance does not mandate:

- a type named `Intent`;
- one public `onIntent(Intent)` entry point;
- a reducer for every screen;
- nested sealed interfaces;
- an effect `Channel` or `SharedFlow`;
- a base MVI ViewModel;
- one UiState property when pieces are genuinely unrelated;
- a domain/use-case layer for every action.

Typed ViewModel methods are a valid UDF implementation. A sealed Intent hierarchy is a project-level design choice that is useful when exhaustive event modeling, logging, replay, middleware, or a unified dispatch boundary provides concrete value.

## Translate Guidance into Questions

| Concern | Questions to explore |
|---|---|
| Screen state | Is this screen-level business state, reusable UI behavior, or a mixture that would benefit from distinct owners? |
| UI input | Does a typed method, event value, or unified dispatcher make intent and call sites clearer here? |
| Render state | Which values must remain consistent, and which truly evolve independently? |
| Durable outcome | What experience is expected after collection gaps, recreation, or returning to the screen? |
| Navigation/focus/snackbar | Which part is a business decision and which part is UI behavior? |
| External updates | Does this input behave like a user decision or like a stream with its own temporal semantics? |
| Android UI types | Does crossing the boundary simplify the design or spread platform/resource concerns unnecessarily? |

## Terminology Discipline

Use **UDF** for the official flow principle. Use **MVI** for the project's chosen implementation style. Do not claim that a particular sealed hierarchy is “the official MVI architecture.”

Treat an event as something that happened or was requested; treat state as the durable snapshot needed to render. Avoid naming all callbacks “Intent” if they have different ownership or delivery semantics.
