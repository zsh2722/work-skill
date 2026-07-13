---
name: git-commit-zh
description: Create safe, reviewable Git commits and Chinese Conventional Commit messages. Use when the user asks to commit, stage, push, generate a commit message, write a PR/MR summary, inspect staged changes, split multiple file changes into logical commits, or apply Chinese Conventional Commit rules.
---

# Git Commit Zh

Use this skill to turn repository changes into small, coherent commits with clear Chinese Conventional Commit messages. Treat committing as an engineering handoff: inspect exactly what will be recorded, verify what is reasonable, and avoid staging unrelated user work.

## Operating Rules

1. Inspect the worktree before any commit-related action: `git status --short --branch`.
2. Prefer already staged content when generating a commit message. If nothing is staged, inspect unstaged changes and stage only the files the user requested.
3. Never stage unrelated files, generated noise, local config, secrets, credentials, build artifacts, or user changes that are outside the requested scope.
4. Do not amend, rebase, reset, force-push, delete branches, or discard changes unless the user explicitly asks for that exact operation.
5. Keep commits logically small. For multiple changed files, first build a commit plan, then stage and commit each logical change set separately. Do not default to one `git add` plus one commit for unrelated edits.
6. Run a proportional verification command before committing when the change touches code or executable behavior. For documentation-only or skill-only changes, validate the artifact format instead.
7. Push only when the user explicitly asks to push or the current request includes push as part of the requested action.
8. Report verification commands that were run and any checks that were skipped.

## Commit Workflow

1. Run `git status --short --branch`.
2. Determine the commit scope:
   - If files are already staged, use `git diff --staged --stat` and `git diff --staged`.
   - If nothing is staged and the user asked to commit the current work, inspect `git diff --stat`, `git diff`, and untracked files.
   - If unrelated changes exist, stage only the requested files or ask for clarification.
3. Build a commit plan before staging:
   - Group files by logical purpose, not by file count.
   - Use one commit for one coherent change, even when it spans several files.
   - Use multiple commits when the diff includes independent fixes, features, refactors, tests, docs, build updates, generated artifacts, or cleanup.
   - If a file contains changes for multiple logical commits, split it with a safe partial staging method or ask the user before continuing.
   - If the grouping is ambiguous, show the proposed commit plan and ask for confirmation.
4. Check for risky content:
   - Secrets, tokens, keys, certificates, passwords
   - Large binaries or generated build outputs
   - Local IDE files, machine paths, temporary files
   - Unintended formatting churn
   - Public API or behavior changes hidden inside documentation or style commits
5. Run the narrowest useful verification for each planned commit or for the final aggregate when per-commit verification is impractical:
   - Code changes: compile, test, lint, analyzer, or target-specific check
   - Skill changes: `quick_validate.py <skill-dir>`
   - Docs-only changes: render/check links if relevant, otherwise inspect diff
6. For each planned commit, stage only that commit's files or hunks, then inspect `git diff --staged --stat` and `git diff --staged`.
7. Generate a commit message from staged content only.
8. Commit using a non-interactive `git commit -m` command. Use multiple `-m` arguments when a body is needed.
9. Repeat staging, inspection, verification as needed, and commit creation until every planned commit is recorded.
10. Confirm the result with `git log -1 --oneline` and `git status --short --branch`.

## Commit Planning

Before committing more than one changed file, decide whether the changes belong to one commit or several.

Prefer one commit when:

- The files implement one feature, one bug fix, one refactor, or one documentation update.
- Tests or docs are directly attached to the code change they validate or explain.
- A mechanical rename or move touches many files but has one purpose.
- Splitting would leave intermediate commits that do not build or do not make sense.

Prefer multiple commits when:

- Changes have different purposes, such as a bug fix plus unrelated cleanup.
- Different modules changed independently.
- Formatting or generated-code updates are mixed with behavior changes.
- Tests cover a separate pre-existing bug or unrelated feature.
- Build/tooling changes are mixed with application logic.
- The commit message would need "and" to describe unrelated outcomes.

Use this planning format when the grouping is not obvious:

```text
Planned commits:
1. <type>(<scope>): <中文摘要>
   Files: <paths>
   Verification: <command>
2. <type>(<scope>): <中文摘要>
   Files: <paths>
   Verification: <command>
```

If the user asked simply to "commit everything" and the worktree contains multiple logical groups, proceed with the plan when the groups are clear. Ask only when grouping or ownership is ambiguous.

## Commit Message Format

Use this exact first-line format:

```text
<type>(<scope>): <中文摘要>
```

The scope is optional:

```text
<type>: <中文摘要>
```

Rules:

- Use one of the approved English types.
- Write the summary in Chinese.
- Keep the first line concise, ideally within 50 characters total.
- Do not end the summary with punctuation.
- Use the imperative/result style: describe what the commit does after it lands.
- Add a body when the diff is non-trivial, has multiple bullets, or needs motivation.
- Use `BREAKING CHANGE: <说明>` in the body for incompatible public API, ABI, protocol, storage, CLI, or config changes.

## Approved Types

- `feat`: New user-visible capability or feature
- `fix`: Bug fix
- `docs`: Documentation-only or skill/documentation content
- `style`: Formatting-only change that does not affect behavior
- `refactor`: Code restructuring without changing behavior
- `perf`: Performance improvement
- `test`: Tests only
- `build`: Build system, dependencies, packaging, generated build config
- `ci`: CI/CD configuration
- `chore`: Maintenance that does not modify source behavior
- `revert`: Revert a previous commit

## Scope Selection

Choose a short lowercase scope when it clarifies ownership:

- Module or feature: `player`, `audio`, `wifi`, `lyrics`, `skill`
- Tooling area: `build`, `ci`, `deps`
- Avoid broad scopes such as `project`, `misc`, `update`, or `code`.
- Omit scope if no clear module owns the change.

## Message Body

Use a body when the summary alone is insufficient.

Format body bullets in Chinese:

```text
<type>(<scope>): <中文摘要>

- 说明修改了什么
- 说明为什么这样改
- 说明验证方式或兼容性影响
```

Keep the body factual. Do not mention the assistant, internal reasoning, or implementation attempts that are not relevant to future maintainers.

## Common Message Choices

- Adding or updating a skill: `docs(skill): 优化命名规范 skill`
- Adding tests: `test(player): 补充播放状态切换测试`
- Fixing a crash: `fix(player): 修复空音频源导致的崩溃`
- Refactoring without behavior change: `refactor(audio): 拆分音频焦点处理逻辑`
- Build config update: `build: 更新 Gradle 构建配置`
- Revert: `revert: 回退播放队列重构`

## Staging Guidance

- Use explicit paths with `git add`.
- Stage new files only after confirming they belong in the commit.
- Stage one logical commit at a time. After each commit, return to `git status --short` before staging the next group.
- If there are partially related edits in one file, prefer an interactive or manual staging strategy only when safe; otherwise ask the user.
- Do not use broad staging such as `git add .` when the worktree contains unrelated changes.
- Re-check `git diff --staged --stat` after staging.

## Push Workflow

When the user asks to push:

1. Confirm the branch with `git status --short --branch`.
2. Ensure the intended commit exists with `git log -1 --oneline`.
3. Push the current branch with the repository's normal upstream.
4. Report the pushed branch, commit hash, and any MR/PR link printed by the remote.

If the branch has no upstream, ask before setting one unless the user explicitly gave the target remote and branch.

## Failure Handling

- If verification fails, do not commit unless the user explicitly asks to commit failing work. Report the failed command and the relevant error.
- If hooks modify files, re-run `git status` and inspect the new diff before committing.
- If `git commit` fails because hooks reject the commit, report the hook output and leave the staged state intact.
- If `git push` is rejected, do not pull/rebase automatically. Report the rejection and ask how to proceed.
- If secrets or suspicious files are staged, stop and ask before continuing.

## Final Response

After committing or pushing, include:

- Commit hash and subject
- Verification command and result
- Final branch status
- Push destination and MR/PR link if available

Do not claim success without fresh command output proving the commit, push, or validation state.
