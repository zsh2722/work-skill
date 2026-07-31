---
name: naming-conventions
description: Improve and review code identifiers so they are natural, concise, domain-meaningful, and consistent with the local language style. Use when creating, renaming, refactoring, or reviewing classes, interfaces, methods, functions, variables, modules, packages, constants, booleans, callbacks, events, tests, or API names; especially to avoid AI-like over-descriptive names, logic-concatenated names, vague names, leaking implementation details, and names that conflict with local or language conventions.
---

# Naming Conventions

Use this skill to choose names that read like production code written by an experienced maintainer. Optimize for domain clarity, local consistency, and API stability over artificial brevity or exhaustive description.

## Core First Principles (元法则)

1. **Ubiquitous Domain Language (业务统一语言)**: Identifiers must map to real-world business domain concepts (What it IS in Business). Never create identifiers that merely describe CPU/runtime buffering or transient execution steps (How Computer Buffers It).
2. **Entity & State Orthogonality (实体与状态正交)**: Maintain a Single Source of Truth (SSOT) for domain entities. An entity's identity (e.g., `currentSong`) remains constant across its lifecycle; phase transitions must be modeled as State Machine enums, never as concatenated variable names (e.g., avoid `pendingSong`, `preparingSong`).
3. **Minimal State Scope (状态作用域最小化)**: Class-level fields must only hold persistent, core object state. Never elevate a transient reference used only during a single async callback or operation into a class field.

## Operating Rules

1. Inspect nearby code before changing names. Preserve established project vocabulary, casing, prefixes, suffixes, and framework conventions.
2. Prefer the repository's broader style rules when available. In this project, use `language-coding-style` for language-specific casing, file names, constants, imports, and formatter expectations.
3. Do not rename public APIs, ABI symbols, serialized fields, database columns, protocol fields, environment variables, config keys, CLI flags, reflection hooks, generated names, or documented external contracts for naming taste alone.
4. Keep behavioral edits separate from rename-only edits when practical.
5. For every non-obvious rename, verify usages with search, compiler/analyzer support, or tests when available.
6. When reviewing, explain why a name is misleading and propose a concrete replacement.

## Naming Decision Process

Apply this sequence:

1. Validate domain existence: Can this concept be naturally spoken in a domain expert discussion? If not, refactor the underlying abstraction before naming.
2. Check SSOT and State Orthogonality: Is this a distinct domain entity or merely a state of an existing entity? If it's a state, use a state enum on the primary entity.
3. Verify scope boundaries: Does this value need class-level persistence, or can it be scoped locally to a method/callback context?
4. Choose the shortest name that remains unambiguous in its scope.
5. Apply the language's casing and initialism rules.
6. Check for false precision: names that describe conditions, implementation steps, framework details, or current call-site context instead of the concept itself.
7. Prefer not renaming when the current name is consistent, stable, and clear enough.

## Good Name Properties

- Name the domain concept, not the implementation accident.
- Fit the scope: short locals are acceptable; public APIs need fuller names.
- Use verbs for operations and nouns or noun phrases for types and values.
- Describe one primary responsibility.
- Make boolean names answer yes/no naturally.
- Use plural names for collections.
- Include units when the type does not make units obvious.
- Keep common terms consistent across the module.
- Use accepted initialisms according to language convention, such as `HTTP`, `URL`, `ID`, `JSON`, and `API`.

## Bad Name Patterns

Flag or fix these unless local convention requires them:

- State-concatenated variables (`pending<Entity>`, `preparing<Entity>`, `processing<Data>`): Indicative of broken SSOT or missing State Machine design.
- Logic-concatenated names: `shouldRemoveCurrentFromQueueWhenSuspended`, `parseValidateExecuteAndSave`.
- Call-site narratives: `removeCurrentBecauseQueueIsFull`, `handleUserClickFromSettings`.
- Implementation leakage: `saveUserViaRoomDao`, `fetchDataWithRetrofit`, `redisBackedUserManager`, unless the implementation is the actual abstraction boundary.
- Vague placeholders: `data`, `obj`, `thing`, `temp`, `flag`, `result`, `info`, `manager`, `helper`, `util`, `common`.
- Type-encoded or Hungarian names: `strName`, `iCount`, `bEnabled`, `pszBuffer`.
- Private abbreviations or pinyin: `yhxx`, `jisuanjiage`, `usrNm`, `calcAvgVal`.
- Repeated context: `User.userNameForUser`, `OrderService.processOrderServiceRequest`.
- Double negatives: `isNotDisabled`, `notInvalid`.
- Artificially long test names that duplicate the full arrange/act/assert story instead of the behavior under test.

## Acceptable Exceptions

Do not mechanically reject a name only because it is long or contains words such as `should`, `from`, `to`, `when`, or `with`.

Allow these when they are idiomatic and clear:

- Test names that describe behavior, especially in projects that use descriptive test naming.
- Boolean policy methods such as `shouldRetry`, `canSubmit`, `hasPermission`, `isEnabled`.
- Conversion names such as `fromJson`, `toJson`, `toString`, `fromBytes`.
- Builder or factory names such as `withTimeout`, `fromConfig`, `createDefault`.
- Framework-required lifecycle names, annotations, callbacks, generated names, and reflection hooks.
- Domain terms that are longer because the business concept is genuinely specific.

## Identifier Guidance

### Types and Interfaces

- Use nouns or role names: `UserRepository`, `PaymentGateway`, `AudioSession`, `CommandMatcher`.
- Avoid process verbs for types: prefer `ImageProcessor` over `ProcessImage`.
- Avoid unnecessary interface prefixes such as `IUserRepository` unless the local project or platform requires them.
- Avoid generic containers such as `BaseManager`, `CommonHelper`, or `DataUtil` unless the abstraction has a precise, documented responsibility.

### Functions and Methods

- Use verb phrases for actions: `loadUser`, `calculateTotal`, `invalidateSession`, `requestFocus`.
- Let the class or module provide context. Inside `Queue`, prefer `removeCurrent()` over `removeCurrentFromQueue()`.
- Split names that combine multiple actions; then split the implementation if the function truly has multiple responsibilities.
- Prefer domain-level verbs over technical plumbing: `persistSession` can be better than `writeSessionToSharedPreferences` unless storage choice is the point.

### Variables and Fields

- Name values by role or domain meaning, not by type alone.
- Keep local loop/index names short only when the scope is tiny and conventional.
- Use units in numeric names when the type does not encode them, such as `timeoutMs` or `buffer_size_bytes`.
- Use collection pluralization: `users`, `pendingRequests`, `supportedFormats`.

### Booleans

- Prefer `is`, `has`, `can`, `should`, `needs`, `supports`, `contains`, or `allows`.
- Name the positive state when practical: `isEnabled` instead of `isNotDisabled`.
- Avoid generic flags: replace `flag` with `isDirty`, `hasFocus`, `canRetry`, or the actual predicate.

### Tests

- Follow local test naming style first.
- Use a behavior name that helps diagnose failure.
- Avoid encoding every setup detail when the test body already makes it clear.
- Prefer consistent patterns such as `returnsEmptyListWhenCacheMisses`, `rejectsExpiredToken`, or `parse_invalid_header_returns_error`, depending on language style.

## Rename Workflow

1. Find all usages before renaming.
2. Check whether the name crosses public, serialized, generated, or reflection boundaries.
3. Prefer IDE/language-aware rename tools when available.
4. Update tests, mocks, fixtures, docs, and comments that reference the renamed concept.
5. Run the narrowest available verification: compile, analyzer, type check, unit tests, or search for the old name.
6. If the rename cannot be fully verified, state the remaining risk.

## Review Output

When reviewing names, lead with actionable findings. For each issue include:

- Current name
- Why it is unclear, misleading, unstable, or inconsistent
- Suggested replacement
- Compatibility risk, if any

Use severity sparingly:

- `P1`: public API, generated boundary, or widely reused name is misleading or risky.
- `P2`: touched code has an unclear or inconsistent name worth fixing now.
- `P3`: optional cleanup; avoid reporting if it would create churn.

If the existing names are acceptable, say so and mention any unchecked boundaries.
