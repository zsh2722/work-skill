# Official Foundations and Source Interpretation

Use these primary sources when current verification is needed:

- Android modularization overview: <https://developer.android.com/topic/modularization>
- Common modularization patterns: <https://developer.android.com/topic/modularization/patterns>
- Android architecture guide: <https://developer.android.com/topic/architecture>
- Architecture recommendations: <https://developer.android.com/topic/architecture/recommendations>
- Domain layer guidance: <https://developer.android.com/topic/architecture/domain-layer>
- Data layer guidance: <https://developer.android.com/topic/architecture/data-layer>
- Now in Android repository: <https://github.com/android/nowinandroid>
- NIA modularization learning journey: <https://github.com/android/nowinandroid/blob/main/docs/ModularizationLearningJourney.md>
- NIA architecture learning journey: <https://github.com/android/nowinandroid/blob/main/docs/ArchitectureLearningJourney.md>

## Stable Interpretations

- Official Android guidance explicitly says there is no single modularization strategy for every project. Treat its patterns as adaptable guidance.
- High cohesion and low coupling are the primary design principles; a prescribed directory tree is not.
- Official patterns describe feature, data, app, common/core, and test modules. They do not require every project to contain every type.
- A feature commonly represents a screen or a cohesive group of screens and often contains UI plus a ViewModel. Official guidance shows feature modules depending on data modules; it does not prescribe a repository implementation inside every feature.
- Common/core modules are reused capabilities and do not represent one architecture layer.
- Android's recommended architecture requires clear UI and data layers. Its domain layer is optional and intended for complex or reused business logic.
- Repositories abstract and coordinate data sources. UI components and ViewModels should not access data sources directly.
- Official modularization guidance recommends minimal public APIs, `implementation` over `api` where possible, consistent build configuration, and Kotlin/JVM modules when Android capabilities are unnecessary.
- Now in Android is a reference application optimized for its own goals. Its current feature `api`/`impl` split, core module set, navigation contracts, and catalog app are examples, not universal requirements.

## Claims to Avoid

Do not claim that Google officially recommends:

- “Everything is Feature” as an absolute rule;
- one Gradle module per business feature or screen;
- a mandatory domain/use-case layer for every large app or every repository call;
- repositories, DI, state, event, and effect folders inside every feature;
- wrapping all Material components or all dimension literals in application-specific tokens;
- a specific team-size or page-count threshold for module extraction;
- Compose itself requiring more modularization than View-based UI.

When citing Now in Android, distinguish “NIA currently does this” from “Android guidance recommends this.” Verify the current repository before describing its exact module tree because samples evolve.
