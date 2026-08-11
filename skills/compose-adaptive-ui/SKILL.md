---
name: compose-adaptive-ui
description: Design, implement, refactor, or review adaptive Android UIs with Jetpack Compose across phones, tablets, foldables, freeform/desktop windows, and multi-window modes. Use for Compose UI components, app screens, navigation, canonical layouts, pane behavior, window size classes, folding features, state continuity, accessibility, testing, or modular app architecture where the UI must respond to available window space and capabilities without duplicating device-specific screens.
---

# Compose Adaptive UI

## Core stance

Design for the **current window and user task**, not for device labels. Treat phone, tablet, foldable, desktop, portrait, landscape, and split screen as observations and test contexts—not as layout APIs.

Keep these concerns separate:

1. **Content and user task**: what matters, what is selected, and what actions are available.
2. **Window environment**: size classes, exact constraints where needed, posture, folds/hinges, insets, input mode, and accessibility settings.
3. **Presentation policy**: navigation form, pane count, content density, placement, and maximum readable widths.
4. **Rendering**: reusable components placed by the selected policy.

Prefer one state model and one component vocabulary with multiple compositions. Do not create parallel phone/tablet/foldable feature implementations unless their user tasks genuinely differ.

## Workflow

### 1. Inspect before designing

- Locate the app shell, navigation, screen state owner, reusable composables, theme, dependency catalog, and tests.
- Identify supported Compose, Material 3 Adaptive, Navigation, WindowManager, and Android versions from the project; do not guess dependency versions or copy stale API signatures.
- Preserve the project's architecture and dependency conventions unless the task explicitly asks for a migration.
- Read [references/adaptive-design-guide.md](references/adaptive-design-guide.md) before choosing layout policy, pane navigation, fold handling, architecture boundaries, or a test matrix.
- Consult current official Android documentation when exact APIs, artifacts, annotations, breakpoints, or platform behavior matter.

### 2. Model the experience

Describe the screen independently of layout:

- primary task and content hierarchy;
- destinations and navigation depth;
- selection, editing, transient UI, and scroll state;
- relationships among list, detail, main, supporting, feed, tools, and optional content;
- what may move, resize, appear, disappear, or become another interaction surface.

Classify state deliberately:

- Keep domain data and durable user intent in the appropriate state holder/ViewModel.
- Keep window measurements and presentation policy in the UI layer.
- Keep selected content durable when it represents user intent; do not store `isTablet`, pane count, or a device label as business state.
- Preserve continuity across resize, rotation, folding, process recreation, and back navigation according to product needs.

### 3. Derive a layout policy

Use window size classes for high-level layout decisions. Use local constraints for component-level responsiveness. Add posture or separating hinge information only when it changes usability or prevents content from crossing an occlusion.

Choose policy by content needs, not by a universal width table. Consider:

- navigation: bar, rail, drawer, or a custom form;
- content: single pane, list-detail, supporting pane, feed/grid, or a domain-specific composition;
- density: number of visible items, optional metadata, action placement, and spacing;
- bounds: readable maximum width, minimum pane width, resizable-window extremes, and insets;
- continuity: which pane remains visible and where focus/back should go after the policy changes.

Prefer adaptive library policies and canonical scaffolds when their semantics match. Compose lower-level primitives or custom policies when requirements do not fit. Never force a canonical scaffold merely because it exists.

### 4. Implement by responsibility

Structure code around these roles; names and module boundaries may vary:

- **App shell** owns top-level navigation adaptation and supplies window context.
- **Feature route/state holder** owns data, durable selection, and events.
- **Adaptive screen/container** maps window context plus content requirements to presentation policy.
- **Pane/content composables** render reusable content and expose events without querying device type.
- **Small responsive components** use their own constraints rather than inheriting a global phone/tablet branch.

Hoist state and use unidirectional data flow. Prefer stable policy/value objects over scattered `if (width...)` checks. Keep branching near containers; keep leaf components reusable and previewable.

When library support fits, consider:

- `currentWindowAdaptiveInfo()` for current window information;
- navigation-suite APIs for top-level adaptive navigation;
- navigable list-detail or supporting-pane scaffolds for pane navigation, back behavior, and transitions;
- adaptive grids or constraint-driven composition for feed/catalog screens;
- WindowManager or adaptive posture/hinge data for fold-aware placement.

Verify exact symbols against the versions present in the project and official docs before editing.

### 5. Validate behavior, not screenshots alone

Test representative windows and transitions:

- compact, medium, expanded, and any project-specific boundary values;
- portrait/landscape, split screen, freeform resize, unfolded/folded states, and separating hinges when supported;
- empty, loading, error, long text, large font, RTL, keyboard, mouse, stylus, and focus navigation where relevant;
- selection continuity, scroll restoration, pane back behavior, deep links, and resize while a detail or editor is open.

Use focused unit tests for policy functions, Compose tests for semantics and interaction, previews for rapid coverage, and emulator/device tests for fold/posture and runtime resize. Do not claim fold correctness from static width previews alone.

## Decision guardrails

- Do not branch on model name, physical screen size, orientation alone, or `smallestScreenWidthDp` as the primary runtime policy.
- Do not assume compact means phone or expanded means tablet; a resizable app can cross classes at runtime.
- Do not stretch content indefinitely; constrain readable content and use surplus space intentionally.
- Do not hide essential actions merely to fit a narrow window; reprioritize or move them to an accessible surface.
- Do not recompute trivial size-class comparisons with `remember` for performance theater. Cache only expensive derived work or state whose identity matters.
- Do not duplicate complete screens for each class. Duplicate only layout composition when reuse would damage clarity.
- Do not let a hinge split interactive content, dialogs, media controls, or reading flow; treat separating/occluding features as spatial constraints.
- Do not equate a successful build with an adaptive UX; verify state, back, focus, resizing, and accessibility.

## Expected output

For design or architecture requests, provide:

1. content/task model;
2. window-to-layout policy and rationale;
3. state ownership and continuity plan;
4. component/module responsibilities;
5. navigation and pane behavior;
6. validation matrix and notable tradeoffs.

For implementation requests, inspect the repository, make the smallest coherent change, follow local conventions, and run proportionate build/static/test checks. Explain assumptions and any API/version limitation.
