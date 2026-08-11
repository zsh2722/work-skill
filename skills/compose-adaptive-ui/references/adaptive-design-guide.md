# Adaptive design guide

## Contents

1. Principles and breakpoints
2. Signals and decision levels
3. Canonical patterns and back behavior
4. Navigation and state continuity
5. Architecture
6. Foldables, Android 16, and resizable windows
7. Component and primitive selection
8. Verification matrix
9. Official sources

## 1. Principles and breakpoints

### Design the window, not the device

An app renders into a window that can resize during use. The same physical device can cross Compact, Medium, Expanded, Large, and Extra-large because of split screen, freeform desktop windows, rotation, folding, or embedded contexts. Device categories are useful for product discussion and test coverage, but window conditions should drive runtime layout.

Official width breakpoints are Compact `<600dp`, Medium `600–839dp`, Expanded `840–1199dp`, Large `1200–1599dp`, and Extra-large `>=1600dp`. Official height breakpoints are Compact `<480dp`, Medium `480–899dp`, and Expanded `>=900dp`. Prefer `currentWindowAdaptiveInfoV2()` in Material 3 Adaptive 1.3.0-rc01 or newer; it includes L/XL by default. The older Boolean overload can enable L/XL in compatible releases but is deprecated starting in 1.3.0-rc01.

### Adapt information architecture, not only dimensions

Responsive behavior changes sizes and wrapping. Adaptive behavior may also change navigation form, pane count, information density, action placement, or content priority. A large layout should expose useful relationships or reduce navigation cost rather than merely enlarge cards and whitespace.

### Preserve semantics while recomposing structure

List, detail, editor, filters, and actions should retain their meaning across window changes. Recompose them into different arrangements without resetting selection, losing input, or creating separate business flows.

### Treat breakpoints as policy inputs

Window size classes are opinionated high-level breakpoints, not a complete design. A feature may need minimum pane widths, maximum reading widths, aspect ratio, height class, or local constraints. Centralize such decisions and name them by capability (for example, `canShowListAndDetail`) rather than device (`isTablet`).

## 2. Signals and decision levels

Use the least specific signal that correctly solves the problem:

| Decision level | Typical signal | Example |
|---|---|---|
| App shell | Window size class | Choose navigation bar, rail, or drawer |
| Screen composition | Size class + content requirements | Show one or multiple panes |
| Fold-aware placement | Posture and fold/hinge bounds | Keep panes on separate regions |
| Reusable component | Parent constraints | Wrap actions or change card composition |
| Interaction enhancement | Input/focus capabilities | Add hover, keyboard shortcuts, focus order |

Width is often the primary signal, but height matters for short landscape windows, tabletop postures, and dense controls. Exact dimensions are appropriate when a component has a real intrinsic minimum or maximum; avoid replacing every size class with arbitrary pixel thresholds.

## 3. Canonical patterns

### List-detail

Use when selection from a collection reveals a related detail. Small windows usually show one pane at a time; sufficiently large windows can show list and detail together. An optional extra pane can host context or secondary detail.

Define:

- selection behavior when no item is selected;
- whether large windows auto-select, show a placeholder, or preserve no selection;
- back behavior from detail to list in single-pane mode;
- deep-link behavior and restoration;
- pane proportions and minimum usable widths.

Prefer a navigable scaffold when it matches the navigation model because it coordinates pane visibility and back behavior. Use the non-navigable scaffold or a custom layout when navigation is owned elsewhere.

For the official navigable scaffold, choose `BackNavigationBehavior` from product semantics rather than manually guessing pane history:

- `PopUntilScaffoldValueChange` is the recommended default for most list-detail flows: single-pane detail returns to list, while multi-pane item changes do not create meaningless visual back steps.
- `PopUntilContentChange` restores prior selected content when that history is meaningful.
- `PopUntilCurrentDestinationChange` returns until the active pane destination changes.
- `PopLatest` pops exactly one entry and can become unintuitive after runtime window changes; use only with an explicit reason.

Use a saveable/parcelable content key supported by the installed navigator API; pass stable identifiers rather than large domain objects.

### Supporting pane

Use when secondary content supports the primary task: tools, properties, alerts, reference data, participants, or filters. The supporting pane may be adjacent on large windows and move to a sheet, destination, disclosure, or inline region on small windows.

Do not demote essential workflow steps into an inaccessible auxiliary pane. Specify how users open, close, and return from it in every layout mode.

### Feed/grid

Use adaptive grids when items are peers and available width should affect column count. Choose a meaningful minimum item width and cap content width or column count when additional columns would harm scanning. Important items may span more area only when hierarchy warrants it.

### Custom composition

Use a custom policy for dashboards, editors, maps, media, industrial controls, or other domain layouts whose spatial relationships differ from canonical patterns. Reuse the same principles: explicit content priority, minimum viable regions, continuity, and centralized policy.

## 4. Navigation and state continuity

Top-level navigation adaptation and within-feature pane navigation are different concerns. The app shell can change navigation chrome while a feature independently chooses pane visibility.

Keep:

- domain data in repositories/use cases/state holders appropriate to the architecture;
- durable user intent such as selected item or draft in a restorable owner when product semantics require it;
- ephemeral visual state near the composable that owns it;
- window information and derived presentation policy in the UI layer.

When the window crosses a breakpoint, preserve the user's current task. Define focus transfer, selected destination, visible pane, scroll, editor draft, and back-stack behavior. A layout mode itself is normally derived state, not saved business state.

## 5. Architecture

A useful dependency direction is:

```text
domain state + user events
          ↓
feature route / state holder
          ↓
adaptive container ← window environment
          ↓
layout policy
          ↓
reusable content and pane composables
```

An optional shared adaptive module can expose project-wide policy vocabulary, window-context adapters, and app-shell components. Avoid turning it into a universal framework that owns feature semantics. Feature-specific pane rules belong with the feature when they depend on its content.

Prefer policy objects or pure functions that can be unit tested. Avoid passing raw window information through every leaf. Pass semantic capabilities or let a local component respond to its own constraints.

## 6. Foldables, Android 16, and resizable windows

A foldable is not always dual pane, and dual pane is not exclusive to foldables. First choose a layout from available space; then refine placement when a fold or hinge separates or occludes usable regions.

Consider:

- book posture, tabletop posture, and fully open state;
- separating versus non-separating features;
- occlusion and hinge bounds;
- whether content should span, avoid, or align with the feature;
- rapid configuration changes and continuity while folding;
- tri-fold and landscape devices that may not share older posture assumptions.

Resizable desktop and multi-window modes require continuous behavior between preferred breakpoints. Check awkward intermediate sizes and very short windows, not only named device presets.

For target 36 apps running in qualifying Android 16 large-screen environments, the platform can ignore orientation, aspect-ratio, and resizability restrictions. Design edge-to-edge, resize, and all orientations as normal runtime behavior. Predictive back system animations are enabled by default only for target 36+ apps running on Android 16+; do not rely on `onBackPressed` or raw key handling to intercept system back navigation. Prefer Navigation and navigable adaptive scaffolds, and validate started/progressed/cancelled/committed behavior when custom handling is necessary.

## 7. Component and primitive selection

- Make leaf composables stateless where practical: data in, events out.
- Let containers choose placement; let components choose internal wrapping from local constraints.
- Set readable maximum widths for text-heavy content.
- Use `Row`, `Column`, `Box`, local constraints, and stable lazy containers as the conservative baseline.
- Use lazy lists/grids for large homogeneous datasets requiring lazy loading.
- Consider experimental `Grid` for structural two-dimensional layouts; it does not lazy-load.
- Consider experimental `FlexBox` for a small number of one-dimensional items that must grow, shrink, or wrap; prefer it over `FlowRow`/`FlowColumn` only after the project accepts its experimental API.
- Consider experimental `mediaQuery` for dynamic posture, pointer precision, keyboard kind, hardware capability, or viewing-distance conditions. It requires integration enablement and version verification.
- Avoid blanket `fillMaxWidth()` as a large-screen strategy.
- Maintain touch targets and also support keyboard, mouse, stylus, focus traversal, hover/context actions, and accessibility semantics when the product targets large screens.
- Keep dialogs, menus, sheets, and drag targets within usable regions and away from occluding folds.

## 8. Verification matrix

Build a task-specific matrix rather than testing every possible combination:

| Axis | Representative checks |
|---|---|
| Width/height | 599/600, 839/840, 1199/1200, 1599/1600dp; height 479/480 and 899/900dp; intermediate resize |
| Window mode | Full screen, split screen, freeform/desktop |
| Fold | Book, tabletop, separating hinge, unfold during task |
| State | No selection, selected detail, draft/edit, deep link, process restore |
| Content | Empty, loading, error, long list, long text, extreme data |
| Accessibility | Large font/display size, TalkBack semantics, RTL, contrast |
| Input | Touch, keyboard focus, mouse, stylus if applicable |

Test pure layout policy separately from rendering. Use previews for breadth, Compose tests for semantics and interaction, and runtime emulator/device tests for configuration changes and folds.

## 9. Official sources

Consult these pages for current API names, artifacts, platform behavior, and detailed examples:

- [Adaptive apps hub](https://developer.android.com/develop/adaptive-apps)
- [Build adaptive apps with Compose](https://developer.android.com/develop/ui/compose/build-adaptive-apps)
- [Use window size classes](https://developer.android.com/develop/adaptive-apps/guides/use-window-size-classes)
- [Canonical layouts](https://developer.android.com/develop/adaptive-apps/guides/canonical-layouts)
- [Build a list-detail layout](https://developer.android.com/develop/adaptive-apps/guides/list-detail)
- [Support different display sizes](https://developer.android.com/develop/adaptive-apps/guides/support-different-display-sizes)
- [Make your app fold aware](https://developer.android.com/develop/adaptive-apps/guides/foldables/make-your-app-fold-aware)
- [Adaptive do's and don'ts](https://developer.android.com/develop/adaptive-apps/guides/adaptive-dos-and-donts)
- [Build adaptive navigation](https://developer.android.com/develop/adaptive-apps/guides/build-adaptive-navigation)
- [App orientation, aspect ratio, and resizability](https://developer.android.com/develop/adaptive-apps/guides/app-orientation-aspect-ratio-resizability)
- [FlexBox](https://developer.android.com/develop/adaptive-apps/guides/flexbox)
- [Grid](https://developer.android.com/develop/adaptive-apps/guides/grid)
- [MediaQuery](https://developer.android.com/develop/adaptive-apps/guides/mediaquery)
- [Material 3 Adaptive API: currentWindowAdaptiveInfoV2](https://developer.android.com/reference/kotlin/androidx/compose/material3/adaptive/currentWindowAdaptiveInfoV2.composable)

Official guidance evolves. Treat examples and dependency coordinates as version-sensitive; verify them against the project's dependency graph and the latest Android documentation before implementation.
