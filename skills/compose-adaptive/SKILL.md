---
name: compose-adaptive
description: Design, implement, or refactor adaptive Android UIs with Jetpack Compose across phones, tablets, foldables, freeform/desktop windows, and multi-window modes. Use for creating Compose UI components, app screens, navigation, canonical layouts, pane behavior, state continuity, testing strategy, or modular app architecture where the UI must respond to available window space and capabilities without duplicating device-specific screens. For legacy API migration or official-compliance audits of an existing implementation, use an adaptive-layout audit skill when available.
---

# Compose Adaptive

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
- Read only the relevant sections of [references/adaptive-design-guide.md](references/adaptive-design-guide.md): canonical panes/navigation for multi-pane work, foldables for hinge/posture work, architecture for module/state design, and verification for test planning.
- Treat the stable rules and API families in this skill as the default. Consult current official Android documentation when adding/upgrading dependencies, using an experimental API, or when the project's installed API differs from this skill.

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

Use window size classes for high-level layout decisions. Account for width and height; a medium-width but compact-height window may not support multiple panes. Use local constraints for component-level responsiveness. Add posture or separating hinge information only when it changes usability or prevents content from crossing an occlusion.

Use the official width classes as capability breakpoints, not device labels:

- Compact: `< 600dp`
- Medium: `600–839dp`
- Expanded: `840–1199dp`
- Large: `1200–1599dp`
- Extra-large: `>= 1600dp`

Large and Extra-large require a supported library API. Prefer `currentWindowAdaptiveInfoV2()` in Material 3 Adaptive 1.3.0-rc01 or newer; it includes L/XL by default. Older compatible versions may use `currentWindowAdaptiveInfo(supportLargeAndXLargeWidth = true)`, which is deprecated in 1.3.0-rc01. If the project supports neither, derive additional desktop policy from explicit content constraints without redefining the official classes.

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

Prefer these stable official API families when their semantics fit:

- the installed version's non-deprecated adaptive-info API—prefer `currentWindowAdaptiveInfoV2()` where available—and `WindowSizeClass` for high-level window policy;
- `NavigationSuiteScaffold` for top-level navigation bar/rail adaptation; override `layoutType` only when product semantics justify a drawer or custom navigation form;
- `NavigableListDetailPaneScaffold` / `NavigableSupportingPaneScaffold` plus their remembered `ThreePaneScaffoldNavigator` for pane navigation, saved content keys, predictive-back transitions, and configurable `BackNavigationBehavior`;
- their non-navigable scaffold variants when navigation state is deliberately owned elsewhere;
- WindowManager `WindowLayoutInfo` / `FoldingFeature`, or compatible Material 3 Adaptive posture and hinge information, for fold-aware placement.

Choose lower-level layout primitives by workload:

- `Row`, `Column`, `Box`, constraints, and stable lazy lists/grids remain valid defaults.
- Use `LazyVerticalGrid` / related lazy layouts for large homogeneous collections that require lazy loading.
- Consider experimental `Grid` for structural two-dimensional screen/component layout; it is not lazy.
- Consider experimental `FlexBox` for a small number of one-dimensional items that must grow, shrink, or wrap. In projects that adopt it, prefer it over `FlowRow` / `FlowColumn` for that use case.
- Consider experimental `mediaQuery` only when dynamic posture, pointer, keyboard, hardware, or viewing-distance queries materially simplify policy. It requires explicit integration enablement.

Do not introduce experimental APIs merely to appear current. Verify Compose version, artifact, opt-in annotation, and project acceptance before using `Grid`, `FlexBox`, or `mediaQuery`.

Before using a version-sensitive symbol, confirm it exists and is not deprecated in the project's installed dependencies. Consult official API reference when adding/upgrading the dependency or when local APIs differ.

### 5. Validate behavior, not screenshots alone

Test representative windows and transitions:

- compact, medium, expanded, large, extra-large, and each enabled boundary (`600`, `840`, `1200`, `1600dp`) immediately below/at/above;
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
- Do not lock orientation, restrict aspect ratio, or disable resizing as an adaptation strategy. For target 36 apps in qualifying Android 16 large-screen environments, the platform can ignore these restrictions. Predictive back system animations are enabled by default only when the app targets API 36+ and runs on Android 16+; migrate legacy system-back interception.

## Expected output

For design or architecture requests, provide:

1. content/task model;
2. window-to-layout policy and rationale;
3. state ownership and continuity plan;
4. component/module responsibilities;
5. navigation and pane behavior;
6. validation matrix and notable tradeoffs.

For implementation requests, inspect the repository, make the smallest coherent change, follow local conventions, and run proportionate build/static/test checks. Explain assumptions and any API/version limitation.
