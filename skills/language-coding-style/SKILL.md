---
name: language-coding-style
description: Apply and enforce a practical coding and naming standard for C, C++, Java, Kotlin, Dart, Go, and Python. Use when creating, editing, refactoring, reviewing, or renaming code; when checking identifiers, file names, comments, API contracts, ownership, formatting, imports, error handling, static analysis, generated code, Git changes, or code review findings against CODING_STYLE_MULTI_LANGUAGE.md and local repository conventions.
---

# Language Coding Style

Use this skill to apply coding style as an engineering constraint, not as cosmetic preference. Preserve local consistency and behavior while improving names, structure, comments, formatting, and review quality.

## Operating Rules

1. Prefer existing repository rules over this skill. Check nearby code, formatter configs, linter configs, and style docs before applying defaults.
2. Treat style as scoped to the user's task. Do not reformat unrelated files, rename unrelated symbols, or mix style cleanup with behavior changes unless requested.
3. Read the relevant section of `references/CODING_STYLE_MULTI_LANGUAGE.md` before enforcing language-specific rules in non-trivial edits or reviews.
4. Preserve public API, ABI, serialization names, database fields, protocol fields, CLI flags, config keys, and externally documented names unless the user explicitly asks for a breaking rename.
5. Do not edit generated, vendored, minified, or third-party code except through its generator or upstream rule set.
6. Prefer automated formatters and linters over manual alignment. If tools are unavailable, state that and apply the repository style manually.
7. Report deliberate deviations from the standard with the reason, especially for compatibility, framework requirements, generated code, or stable local convention.

## Rule Priority

Resolve conflicts in this order:

1. Upstream project, framework, platform, or organization rules
2. Repository style docs such as `CONTRIBUTING.md`, `STYLE.md`, `CODING_STYLE.md`
3. Formatter, analyzer, linter, and CI configuration
4. Stable style in the current module, package, or neighboring files
5. `references/CODING_STYLE_MULTI_LANGUAGE.md`
6. General preference or taste

Use these rule levels when reviewing:

- `Must`: block or fix in new code unless a higher-priority rule overrides it.
- `Recommended`: follow by default; allow a documented exception.
- `Optional`: follow only when the current project has chosen that style.
- `Forbidden`: avoid in new code; flag in review when introduced or expanded.

## Reference Selection

Use `rg -n "^(#|##) " references/CODING_STYLE_MULTI_LANGUAGE.md` to locate sections quickly.

Read these sections as needed:

- Priority, rule levels, universal naming, comments, Git/review/tools: sections 2-5 and 13-16
- C: section 6
- C++: section 7
- Java: section 8
- Kotlin: section 9
- Dart: section 10
- Go: section 11
- Python: section 12
- Default new-project policy: appendix A
- Final execution principles: appendix B

Always read the relevant reference section before:

- Renaming public or widely used identifiers
- Reviewing naming or formatting as the primary task
- Creating new module/package/class/function naming patterns
- Editing C/C++ headers, exported symbols, or ownership-sensitive APIs
- Editing generated-code boundaries, serialization, protocol, or database-facing names
- Applying rules to a language or framework whose local style is unclear

## Edit Workflow

1. Identify the language, file role, and whether the file is source, test, generated, config, API surface, or documentation.
2. Inspect local style in nearby files before changing names or formatting.
3. Apply the smallest coherent change that satisfies the request.
4. Keep behavioral changes separate from style-only changes when practical.
5. Use formatter/analyzer commands already present in the project. Do not install new tools unless asked.
6. Verify with the narrowest useful check: format, lint, compile, unit tests, or static analysis for touched code.
7. Summarize what changed and what was not checked.

## Naming Constraints

Use names that communicate domain meaning and intent.

- Use English identifiers. Do not use pinyin or private abbreviations.
- Avoid vague names such as `data`, `obj`, `thing`, `temp`, `flag`, `manager`, `helper`, `util`, or `common` unless the surrounding domain makes them precise.
- Avoid Hungarian notation and type-encoded prefixes such as `strName`, `iCount`, `bEnabled`, `pszBuffer`, or unnecessary member prefixes.
- Use short names only when conventional and local: `i`, `j`, `x`, `y`, `id`, `fd`, `it`, `ctx`, `err`, `src`, `dst`, `lhs`, `rhs`, `req`, `resp`.
- Name booleans so they answer yes/no naturally: `is`, `has`, `can`, `should`, `needs`, `supports`, `contains`, or `allows`.
- Avoid double negatives such as `isNotDisabled`; prefer positive names such as `isEnabled`.
- Use plural names for collections unless naming a collection type itself.
- Include units when the type does not make units obvious: `timeout_ms`, `buffer_size_bytes`, `connectionTimeoutMillis`.
- Preserve common initialisms according to language convention: Java/Kotlin/Dart `HttpClient`, `userId`; Go `HTTPClient`, `userID`; C/Python `http_client`, `user_id`.

## Structure Constraints

- Keep each function focused on one primary responsibility. Split parse/validate/execute/save chains into named steps when they become distinct responsibilities.
- Avoid names that concatenate implementation phases, such as `parseValidateExecuteAndSaveRequest`.
- Avoid unbounded `Utils`, `Common`, `Helper`, or `Manager` containers. Prefer domain-specific modules or services with clear ownership.
- Keep public APIs minimal and stable. Do not expose internal helpers to make tests or style cleanup easier.
- Make ownership, lifetime, nullability, mutability, and side effects explicit in APIs where the language does not enforce them.
- Prefer early returns, helper functions, polymorphism, or clear state machines over deep nesting.

## Language Defaults

Use these defaults only when local style does not override them.

| Language | Files | Types | Functions and methods | Variables and fields | Constants | Key constraints |
|---|---|---|---|---|---|---|
| C | `lower_snake.c`, `lower_snake.h` | `struct user_info`, `enum state` | `module_action_object` | `lower_snake_case` | `MODULE_UPPER_SNAKE_CASE` | Prefix public APIs, keep internal symbols `static`, make ownership explicit |
| C++ | `lower_snake.cpp`, `lower_snake.h` | `PascalCase` | `PascalCase`; simple getters `lower_snake` | `lower_snake_case`; private `lower_snake_case_` | `kPascalCase` | Prefer RAII, `nullptr`, `enum class`, `constexpr`, self-contained headers |
| Java | `UpperCamel.java` | `UpperCamelCase` | `lowerCamelCase` | `lowerCamelCase` | `UPPER_SNAKE_CASE` | Packages lowercase, no wildcard imports, preserve exception causes |
| Kotlin | `UpperCamel.kt` or local convention | `UpperCamelCase` | `lowerCamelCase` | `lowerCamelCase` | true constants only | Prefer null safety, concise focused functions, official formatting |
| Dart | `lower_snake.dart` | `UpperCamelCase` | `lowerCamelCase` | `lowerCamelCase`; private `_name` | usually `lowerCamelCase` | Follow Effective Dart, sort imports by project convention |
| Go | `lowercase.go` | `MixedCaps`; exported uppercase | `MixedCaps` | `mixedCaps` | `MixedCaps` | Package names short lowercase, export via capitalization, use Go initialisms |
| Python | `lower_snake.py` | `CapWords` | `lower_snake_case` | `lower_snake_case`; non-public `_name` | `UPPER_SNAKE_CASE` | Follow PEP 8, avoid mutable defaults, use clear exceptions and docstrings |

## Comments and Documentation

- Write comments to explain why, constraints, compatibility, tradeoffs, lifecycle, ownership, side effects, and concurrency assumptions.
- Do not write comments that merely restate the code.
- Remove dead commented-out code instead of preserving it.
- Require public API documentation when callers need to know ownership, nullability, units, errors, side effects, threading, or compatibility.
- Use TODO comments only when they include enough context to act on later, such as an issue ID, owner, condition, or removal trigger.

## Generated and External Code

- Do not manually style generated code if it will be overwritten. Change the generator, template, schema, or documented generation step instead.
- Do not rename serialized fields, JSON keys, database columns, RPC fields, environment variables, or config keys for style alone.
- Preserve framework-required names, annotations, reflection hooks, lifecycle methods, and test discovery names.
- When wrapping external names, keep external names at the boundary and convert to project style inside domain code.

## Verification

Choose checks by language and repository tooling:

- C/C++: `clang-format`, `clang-tidy`, compile target, unit tests
- Java: formatter, Checkstyle/Error Prone/SpotBugs if configured, Gradle/Maven test or compile target
- Kotlin: official formatter/ktlint/detekt if configured, Gradle test or compile target
- Dart: `dart format`, `dart analyze`, tests
- Go: `gofmt`, `go vet`, `go test`
- Python: Ruff/Black/isort/mypy/pytest if configured

Run only checks that are available and proportionate to the change. If a check cannot be run, state why.

## Review Output

When the user asks for review, lead with actionable findings ordered by severity. Include file and line references when available.

Use this severity model:

- `P0`: correctness, data loss, security, build break, or incompatible public API change
- `P1`: must-fix style or maintainability violation in touched code that will spread or block tooling
- `P2`: recommended improvement with clear readability or consistency value
- `P3`: optional cleanup or preference; avoid over-reporting

For each finding, explain:

- What violates the standard or local convention
- Why it matters in this codebase
- What concrete change would fix it

If there are no findings, say so and mention any checks not run.
