# UI Responsibilities

## Contents

1. Define the UI boundary
2. Separate intent, state, and effect
3. Assign UI ownership
4. Map domain results to presentation
5. Handle lists, forms, navigation, and components
6. Respect lifecycle and concurrency
7. Test UI responsibilities

## 1. Define the UI Boundary

The UI is responsible for:

- capturing user intent;
- rendering a complete presentation state;
- executing explicitly issued UI effects;
- forwarding lifecycle signals when an application responsibility needs them;
- maintaining ephemeral widget state that has no business meaning.

The UI is not responsible for:

- deciding authorization or eligibility;
- selecting data sources or providers;
- coordinating transactions;
- translating database or transport errors;
- owning durable business state;
- retrying business operations without an application policy;
- mutating shared state before an operation succeeds.

Keep the boundary independent of a specific GUI toolkit.

## 2. Separate Intent, State, and Effect

### Intent

Represent what the user is asking for:

```text
SubmitForm(fields)
SelectPlan(planId)
RetryLoad
DismissMessage(messageId)
```

Do not send raw click handlers deep into the system. Translate toolkit events into meaningful intent at the interaction boundary.

### Presentation State

Use a complete, renderable snapshot:

```text
CheckoutState
  items
  total
  isSubmitting
  fieldErrors
  submitEnabled
  blockingError
```

Prefer one authoritative state stream over several independently updated observables that can form impossible combinations.

Derive display-only values such as labels, enabled states, and visibility from authoritative state in a presenter/mapper or pure transformation.

### Effect

Model one-time actions separately:

```text
NavigateToReceipt(receiptId)
ShowToast(message)
RequestPermission(permission)
OpenExternalUrl(url)
```

Do not store consumable effects as durable state without an identity and acknowledgement policy. Re-rendering state must not repeat payments, navigation, dialogs, or notifications.

## 3. Assign UI Ownership

| Concern | Owner |
|---|---|
| widget focus, scroll position, animation progress | interaction component unless restoration is required |
| form draft | presentation/application owner based on required lifetime |
| validation rule | domain or application policy |
| validation message | presentation mapping |
| loading state | application/presentation owner derived from operation lifecycle |
| navigation decision | application/presentation owner; UI performs navigation |
| selected business entity | application/domain state |
| selected visual row with no business meaning | interaction state |
| retry policy | application owner |
| retry button click | UI intent |

Move state upward only when a longer lifetime or cross-component coordination requires it. Do not place all widget state in a global store.

## 4. Map Domain Results to Presentation

Translate application results into presentation concepts:

```text
Business rejection -> field or page guidance
Dependency unavailable -> retryable error state
Conflict -> refresh or conflict-resolution state
Cancellation -> return to idle without failure noise
Programming/configuration error -> fail visibly and record diagnostics
```

Do not expose exception class names, HTTP codes, database messages, or provider-specific enums directly to the UI unless they are genuinely user-facing domain concepts.

Keep localized text and formatting outside domain rules.

## 5. Handle Lists, Forms, Navigation, and Components

### Lists

Give pagination one owner. It owns:

- current cursor/page;
- active query;
- loading and terminal state;
- deduplication;
- stale request rejection;
- refresh semantics.

Adapters/renderers display supplied items and report item intents. They must not query repositories or decide business actions.

### Forms

Separate:

- raw draft input;
- parsed/validated command;
- business acceptance;
- field-level presentation errors.

Do not make persistence entities double as editable form models.

### Navigation

Express navigation targets in application/presentation language. Keep route strings, controller classes, and toolkit transactions at the adapter boundary.

Make back-stack policy explicit when it is part of the user flow.

### Reusable Components

A visual component may own:

- layout and styling;
- local interaction behavior;
- rendering of component data;
- emission of semantic component intents.

It must not fetch remote configuration, determine account eligibility, or coordinate unrelated components unless it is explicitly an application-level composite.

For server- or configuration-driven UI, separate:

- template/schema;
- parser and validation;
- component factory/registry;
- renderable data;
- event declaration;
- business event handler;
- platform renderer.

The registry creates components; it does not become the business state store.

## 6. Respect Lifecycle and Concurrency

- Observe state only while the interaction owner can render it.
- Cancel or detach work that belongs to a destroyed interaction.
- Keep use-case work alive across UI recreation only when its owner has the required longer lifetime.
- Ignore or cancel stale responses when a newer intent supersedes them.
- Serialize actions that must not overlap, such as duplicate submission.
- Make effect delivery resilient to recreation without accidental replay.
- Unregister listeners symmetrically with their registration owner.

Do not solve lifecycle problems with process-wide mutable state unless the state is truly process-wide.

## 7. Test UI Responsibilities

Test pure state reduction and mapping without a toolkit:

- intent produces loading state;
- success produces content and one navigation effect;
- business rejection maps to the correct guidance;
- retryable dependency failure enables retry;
- stale result does not replace current state;
- repeated render does not repeat an effect.

Test the toolkit layer for:

- correct rendering of representative states;
- intent wiring;
- lifecycle-safe observation;
- effect execution;
- state restoration where required.

Avoid tests that reproduce internal observable assignments without asserting user-visible behavior or ownership.
