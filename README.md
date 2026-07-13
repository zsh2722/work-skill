# Work Skill

面向 Codex 的可安装 Skill 集合，提供命名审查、多语言编码规范和中文 Conventional Commit 工作流。

## Included Skills

| Skill | Purpose |
| --- | --- |
| `naming-conventions` | 审查和改进标识符命名，避免含义模糊、过度描述或实现细节泄漏。 |
| `language-coding-style` | 对 C、C++、Java、Kotlin、Dart、Go 和 Python 应用实用的编码与命名规范。 |
| `git-commit-zh` | 安全地拆分、提交并推送变更，使用中文 Conventional Commit 消息。 |

## Install

在已安装 Codex CLI 的环境中添加 marketplace：

```bash
codex plugin marketplace add zsh2722/work-skill --ref main
```

再安装插件：

```bash
codex plugin add work-skill@work-skill
```

安装完成后，新建一个 Codex task，并以 `$naming-conventions`、`$language-coding-style` 或 `$git-commit-zh` 调用对应 Skill。

## Update

```bash
codex plugin marketplace upgrade work-skill
codex plugin add work-skill@work-skill
```

## Repository Layout

```text
.agents/plugins/marketplace.json       # 可发现、可安装的插件目录
plugins/work-skill/.codex-plugin/      # 插件清单
plugins/work-skill/skills/             # 三个独立且可触发的 Skill
```

## License

[MIT](LICENSE)
