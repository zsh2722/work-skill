# Work Skill

A universal collection of Skills based on the [Agent Skills](https://agentskills.io/specification) open format, usable by AI coding tools that support `SKILL.md`. The Codex plugin is only an optional installation entry point; all tools share the single Skill source code under the root `skills/` directory.

## Included Skills

| Skill | Purpose |
| --- | --- |
| `responsibility-driven-development` | Design, implement, review, or progressively refactor minimally maintainable architectures starting from business behaviors, responsibility ownership, and information boundaries. |
| `naming-conventions` | Review and improve identifier naming to avoid ambiguous meanings, over-description, or leakage of implementation details. |
| `language-coding-style` | Apply practical coding and naming conventions for C, C++, Java, Kotlin, Dart, Go, and Python. |
| `java-design-patterns` | Implement, refactor, and optimize Java designs for real-world problems; evaluate and adopt only GoF patterns or modern Java implementations that deliver proven benefits. |
| `git-commit-zh` | Safely split, commit, and push changes using Chinese Conventional Commit messages. |

## Included Agent

| Agent | Purpose |
| --- | --- |
| `work-skill-agent` | Automatically identifies tasks and orchestrates Skills within the repository; adopters do not need to memorize Skill names or trigger syntax. |

The Agent definition is located at `agents/work-skill-agent.md` and uses a Markdown + YAML frontmatter format. Host tools that support Agents within plugins can select `work-skill-agent` after installation, then simply describe the goal:

```text
Help me sort out the business responsibilities of this requirement, design a minimally maintainable plan, implement it in Java, and commit following Chinese conventions.
```

The Agent automatically loads the minimum necessary Skills based on the task and sequentially completes responsibility modeling, design decisions, coding standards, naming checks, verification, and explicitly authorized Git operations. The specific rules are still maintained by each `SKILL.md`; the Agent is only responsible for routing and orchestration.

Tools that support Agent Skills but not custom Agents can still install and use `skills/` directly, and will not be affected by this entry point.

## Install

### Cross-agent CLI

After installing the [skills CLI](https://github.com/vercel-labs/skills), interactively select the target tool and the Skills to install:

```bash
npx skills add zsh2722/work-skill
```

You can also install all Skills at once and explicitly specify multiple target tools:

```bash
npx skills add zsh2722/work-skill --skill '*' -a codex -a claude-code
```

### GitHub CLI

Install using the GitHub CLI to the user-level directory of the specified tool:

```bash
gh skill install zsh2722/work-skill --all --agent codex --scope user
```

Change `codex` to the corresponding tool name, or use `--dir <directory>` to point to a custom Skill directory.

### Manual

After cloning the repository, copy or symlink the entire `skills/<skill-name>/` directory to the target tool's Skill directory. The `SKILL.md` file within the Skill directory is the only required file; `agents/`, `references/`, and other contents should be preserved as well.

## Update

The repository uses `master` as the stable update branch and `dev` as the daily development branch. Installation and updates default to `master`.

Users who installed via the skills CLI can pull updates:

```bash
npx skills update
```

## Optional Codex Plugin

Codex users can optionally install from the native marketplace:

```bash
codex plugin marketplace add zsh2722/work-skill --ref master
codex plugin add work-skill@work-skill
```

## Repository Layout

```text
skills/                               # The single cross-tool Skill source code
├── responsibility-driven-development/
├── naming-conventions/
├── language-coding-style/
├── java-design-patterns/
└── git-commit-zh/
agents/
└── work-skill-agent.md               # Automatically routes and orchestrates the above Skills
.codex-plugin/plugin.json             # Optional Codex plugin manifest
.agents/plugins/marketplace.json      # Optional Codex marketplace catalog
```

## License

[MIT](LICENSE)
