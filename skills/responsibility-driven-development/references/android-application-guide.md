# Android Application Guide

## Contents

1. Map responsibility roles to Android
2. Keep UI lifecycle-safe
3. Model state and effects
4. Isolate data and platform boundaries
5. Handle services and process boundaries
6. Isolate product and platform variants
7. Apply Java and Kotlin pragmatically
8. Verify Android changes

Use this guide only after establishing the framework-independent responsibility model.

## 1. Map Responsibility Roles to Android

Possible mappings:

| Responsibility role | Android implementation options |
|---|---|
| Interaction | Activity, Fragment, Compose screen, custom View |
| Presentation/application | ViewModel, presenter, screen coordinator, use-case object |
| Domain | plain Java/Kotlin objects and policies |
| Port | Java/Kotlin interface or function type |
| Adapter | Room/SQLite source, Retrofit client, Binder client, file/device adapter |
| Composition | Application startup, DI module, Activity/Fragment factory, service initializer |

Do not make these one-to-one rules. A small screen may use a ViewModel directly with one port. A complex workflow may need a dedicated use case or coordinator.

## 2. Keep UI Lifecycle-Safe

### Fragment Views

Treat the Fragment instance and its View as different lifetimes.

- Bind and observe view-owned state with the view lifecycle.
- Release binding, adapters, listeners, and callbacks when the view is destroyed.
- Keep durable operation state in a longer-lived owner only when recreation must preserve it.
- Do not capture a Fragment or View in a process-wide singleton.

### Compose

- Hoist business state to the appropriate state owner.
- Keep composables focused on rendering state and emitting intents.
- Use effect APIs for lifecycle-bound collection and one-time actions.
- Do not start repositories or register global listeners during arbitrary recomposition.

### Activity and Service Lifetimes

Use application context only for objects that genuinely need process lifetime. Do not upgrade a short-lived responsibility to process lifetime to avoid lifecycle design.

## 3. Model State and Effects

Use `StateFlow`, `LiveData`, or another lifecycle-aware observable according to local project conventions. The responsibility rule is more important than the library:

- expose read-only state;
- keep mutation private to the owner;
- represent a coherent screen state;
- model one-time effects separately;
- avoid several writable observables for one state machine.

Example:

```kotlin
data class ProfileUiState(
    val name: String = "",
    val isSaving: Boolean = false,
    val error: String? = null
)

sealed interface ProfileIntent {
    data class NameChanged(val value: String) : ProfileIntent
    data object Save : ProfileIntent
}

sealed interface ProfileEffect {
    data object Close : ProfileEffect
}
```

This example defines interaction concepts only. Business validation and persistence remain behind the application responsibility.

Avoid using a generic event wrapper as a substitute for effect ownership and delivery semantics.

## 4. Isolate Data and Platform Boundaries

Keep distinct models when responsibilities differ:

```text
Network DTO -> adapter mapping -> domain/application model
Database entity -> adapter mapping -> domain/application model
Domain/application result -> presentation mapper -> UI state
```

Mapping is valuable when it:

- prevents schema or provider changes from spreading;
- enforces defaults and validation;
- translates failure semantics;
- protects mutability or nullability expectations.

Do not create ceremonial duplicate models when they have identical ownership and change together.

### Repository Boundary

A repository or data port should express application needs:

```kotlin
interface ProfileStore {
    suspend fun load(userId: UserId): LoadProfileResult
    suspend fun save(profile: Profile): SaveProfileResult
}
```

Its implementation may coordinate Room, Retrofit, cache, or files. Callers must not reach around it to a DAO for convenience.

### Threading

Assign blocking work to an adapter or data implementation with an explicit dispatcher/executor policy. Do not make UI callers remember which repository methods block.

Preserve cancellation when converting callbacks or Rx streams to coroutines. Do not wrap asynchronous callbacks with blocking waits on the main thread.

## 5. Handle Services and Process Boundaries

Treat Binder, Messenger, broadcasts, and Android services as transport mechanisms.

Define an application-owned client port and implement it with a process adapter:

```text
Use case -> PlaybackPort <- BinderPlaybackAdapter -> remote service
```

The adapter owns:

- binding and reconnection;
- binder death;
- callback translation;
- process-safe model conversion;
- timeout and unavailable semantics;
- listener cleanup.

The use case owns:

- whether playback is allowed;
- workflow order;
- business retry policy;
- state transitions visible to the rest of the application.

Do not expose `IBinder`, `RemoteException`, service actions, or parcel-specific types beyond the adapter unless they are part of an intentionally public Android SDK.

## 6. Isolate Product and Platform Variants

Use source sets, build variants, resources, or injected adapters to isolate differences.

Choose based on the variation:

- **resource-only difference**: resource overlay;
- **platform API difference**: platform adapter selected by composition;
- **different dependency family**: build variant or module dependency;
- **small finite policy difference**: explicit configuration or exhaustive policy selection;
- **independently delivered implementation**: plugin discovery only when truly required.

Keep product checks out of shared business methods. Validate variant wiring at build or startup rather than failing deep in a workflow.

Do not place general business rules in a product-specific source set unless the rule genuinely differs by product.

## 7. Apply Java and Kotlin Pragmatically

- Preserve Java interoperability while a module is mixed-language.
- Prefer plain immutable values for commands, results, and states.
- Use sealed types for a closed result or state set; use open interfaces only for genuine third-party or module extension.
- Use coroutines/Flow for new Kotlin asynchronous work when local infrastructure supports them; do not migrate unrelated RxJava code as part of a responsibility move.
- Preserve interruption, cancellation, backpressure, and error semantics during asynchronous migrations.
- Avoid Kotlin convenience APIs that obscure ownership or create hidden global scope.
- Use explicit executors where Java code requires controlled scheduling and shutdown.

Consult `$language-coding-style` for language details when available.

## 8. Verify Android Changes

Select relevant checks:

- JVM tests for pure rules, reducers, mappings, and use cases;
- coroutine tests with controlled dispatchers;
- Room/SQLite integration tests for mapping and transactions;
- service/Binder tests for connection loss and callback cleanup;
- Fragment/Compose tests for rendering and intent wiring;
- lifecycle tests for recreation and cancellation;
- variant compilation for affected source sets;
- searches for direct DAO, SDK, route, or service access that bypasses the new boundary.

Do not accept a refactor solely because one default variant compiles.
