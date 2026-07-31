# Responsibility Modeling

## Contents

1. Model behavior before structure
2. Build the business narrative
3. Separate commands, events, state, and effects
4. Assign authoritative ownership
5. Control the information budget
6. Use responsibility cards
7. Check model quality

## 1. Model Behavior Before Structure

Do not begin with controllers, services, repositories, modules, or design patterns. Begin with what must be true for the business to work.

Use this sequence:

1. Name the actor.
2. State the actor's goal.
3. Identify the trigger.
4. List preconditions.
5. Describe the successful path in domain language.
6. List alternative and failure paths.
7. Extract invariants and state transitions.
8. Identify externally visible effects.

Example:

> A reviewer approves a pending request. Approval is allowed only while the request is pending and the reviewer has authority. A successful final approval makes the request effective and records who approved it. A rejected or expired request cannot later become approved.

This narrative reveals rules and ownership without choosing a framework.

## 2. Build the Business Narrative

Capture these elements:

| Element | Question |
|---|---|
| Actor | Who initiates or observes the behavior? |
| Goal | What business outcome are they trying to achieve? |
| Trigger | What starts the behavior? |
| Fact | What information is already true? |
| Rule | What decision constrains the behavior? |
| Command | What action is being requested? |
| Event | What fact has already occurred? |
| State | What durable condition affects future behavior? |
| Effect | What externally visible change must happen? |
| Failure | What named outcome prevents success? |
| Invariant | What must never become false? |

Do not treat data fields as a business model. A field becomes meaningful only when connected to rules, ownership, and transitions.

### Ask Rule-Finding Questions

- Who is allowed to perform the action?
- Under which state is it legal?
- Which data must be consistent at the same time?
- Can the action be retried safely?
- What happens if an external dependency succeeds but persistence fails?
- Which outcome must callers distinguish?
- Which facts must be recorded for audit or recovery?
- Who can change the state next?

## 3. Separate Commands, Events, State, and Effects

### Command

A command asks an owner to perform an action. Use an imperative concept such as `ApproveRequest` or `ReserveInventory`.

A command may be rejected. It has one intended owner.

### Event

An event records that a fact occurred. Use a past-tense concept such as `RequestApproved` or `InventoryReserved`.

An event must not be used as a disguised command to hide orchestration. If downstream work is required for the same use case, make the coordinator explicit.

### State

State affects which future actions are legal. Give durable business state one authoritative owner.

Do not create multiple writable copies and synchronize them by callbacks. Derive read models when several consumers need different views.

### Effect

An effect is externally visible work such as sending a message, navigating, writing a file, charging a provider, or showing a notification.

Separate the decision to produce an effect from the adapter that performs it.

## 4. Assign Authoritative Ownership

Assign each item exactly one owner:

- business invariant;
- mutable state;
- workflow order;
- retry and timeout policy;
- transaction boundary;
- external integration;
- renderable state;
- one-time UI effect;
- process-wide resource lifetime.

Ownership means the owner:

- makes the final decision;
- validates changes;
- publishes the authoritative result;
- defines failure semantics;
- controls mutation.

Other components may observe, request, cache, or translate, but must not independently decide the same rule.

### Detect Ownership Conflicts

Look for:

- the same conditional repeated in UI, service, and persistence code;
- several mutable flags representing one lifecycle;
- adapters deciding business eligibility;
- callers that update state before a use case succeeds;
- repositories that decide presentation behavior;
- views that retry business operations without application policy;
- global event handlers that mutate the same state from many locations.

## 5. Control the Information Budget

Give each responsibility only the knowledge required for its decision.

| Role | May know | Must not know |
|---|---|---|
| Interaction | user intent, renderable state, local view identity | database schema, provider protocol, business authorization rules |
| Application | use-case order, permissions, transaction and retry policy | concrete widget types, SQL, transport envelopes |
| Domain | business facts, invariants, legal transitions | framework lifecycle, network client, file paths unless domain-relevant |
| Port | capability required by the caller | provider implementation details |
| Adapter | provider contract and domain/application contract | unrelated business workflows, UI navigation |
| Composition | implementation selection and lifetime | business decisions executed on every request |

Treat this table as a decision aid, not mandatory packaging.

### Translate at Boundaries

Translate external representations near the boundary:

- transport response to application/domain result;
- persistence entity to domain model;
- framework callback to typed event or result;
- device error code to capability-specific failure;
- UI text input to validated command.

Do not reuse one data class across all layers merely to avoid mapping. Shared identity does not imply shared responsibility.

## 6. Use Responsibility Cards

For a non-trivial collaborator, write a short card:

```text
Responsibility:
Purpose:
Owns:
Receives:
Returns/publishes:
May know:
Must not know:
Side effects:
Failure semantics:
Lifetime/concurrency:
```

Example:

```text
Responsibility: Submit expense claim
Purpose: Enforce submission rules and make an accepted claim pending
Owns: Use-case order and consistency boundary
Receives: SubmitClaim command
Returns/publishes: Accepted claim ID or named rejection
May know: Claim policy, claim port, clock, actor permissions
Must not know: HTTP status codes, SQL tables, screen controls
Side effects: Persist state and append audit record
Failure semantics: Distinguish rule rejection from unavailable storage
Lifetime/concurrency: Request-scoped; idempotent by command ID
```

If a card contains several unrelated purposes or many unrelated dependencies, split it. If two cards cannot act without repeatedly exposing each other's internals, reconsider whether they are one cohesive responsibility.

## 7. Check Model Quality

Accept a model only when:

- invariants have named owners;
- commands have intended handlers;
- events describe facts rather than hidden requests;
- states have legal transitions;
- failures are distinguishable where callers need different actions;
- side effects have explicit ordering and recovery expectations;
- technical representations stop at their boundaries;
- the model can be explained without framework vocabulary;
- a new variant changes one selection point and its own implementation, not every caller;
- the direct solution remains understandable without a diagram.

Reject or revise a model when:

- responsibilities are named after generic technical layers only;
- every object is a passive data holder and all decisions live in one service;
- ownership depends on “who currently calls it”;
- correctness relies on global callback order;
- null or default values carry several hidden meanings;
- the model requires speculative extension points to appear clean.
