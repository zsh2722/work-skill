# Work Skill

基于 [Agent Skills](https://agentskills.io/specification) 开放格式的通用 Skill 集合，可供支持 `SKILL.md` 的 AI 编程工具使用。Codex 插件仅作为可选安装入口，所有工具共用根目录 `skills/` 下的唯一 Skill 源码。

## Included Skills

| Skill | Purpose |
| --- | --- |
| `naming-conventions` | 审查和改进标识符命名，避免含义模糊、过度描述或实现细节泄漏。 |
| `language-coding-style` | 对 C、C++、Java、Kotlin、Dart、Go 和 Python 应用实用的编码与命名规范。 |
| `git-commit-zh` | 安全地拆分、提交并推送变更，使用中文 Conventional Commit 消息。 |

## Install

### Cross-agent CLI

安装 [skills CLI](https://github.com/vercel-labs/skills) 后，交互式选择目标工具与要安装的 Skill：

```bash
npx skills add zsh2722/work-skill
```

也可以一次安装全部 Skill，并明确指定多个目标工具：

```bash
npx skills add zsh2722/work-skill --skill '*' -a codex -a claude-code
```

### GitHub CLI

使用 GitHub CLI 安装到指定工具的用户级目录：

```bash
gh skill install zsh2722/work-skill --all --agent codex --scope user
```

将 `codex` 改为对应工具名称，或使用 `--dir <目录>` 指向自定义 Skill 目录。

### Manual

克隆仓库后，将所需的 `skills/<skill-name>/` 整个目录复制或软链接到目标工具的 Skill 目录。Skill 目录中的 `SKILL.md` 是唯一必需文件，`agents/`、`references/` 等内容应一并保留。

## Update

通过 skills CLI 安装的用户可拉取更新：

```bash
npx skills update
```

## Optional Codex Plugin

Codex 用户可选择原生 marketplace 安装：

```bash
codex plugin marketplace add zsh2722/work-skill --ref main
codex plugin add work-skill@work-skill
```

## Repository Layout

```text
skills/                               # 跨工具的唯一 Skill 源码
├── naming-conventions/
├── language-coding-style/
└── git-commit-zh/
.codex-plugin/plugin.json             # 可选 Codex 插件清单
.agents/plugins/marketplace.json      # 可选 Codex marketplace 目录
```

## License

[MIT](LICENSE)
