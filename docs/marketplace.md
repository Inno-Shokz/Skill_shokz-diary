# Marketplace 发布与更新说明

本文说明如何让其他用户通过市场订阅、下载、使用和更新 `shokz-diary`。

## 发布状态

- Claude Code：本仓库已经包含 `.claude-plugin/marketplace.json` 和 `.claude-plugin/plugin.json`，可作为 GitHub marketplace 源使用。
- Codex：本仓库已经包含通过校验的 `.codex-plugin/plugin.json`。若要在 Codex `/plugins` 市场中展示，需要由个人或团队 marketplace 指向该插件源码。

## Claude Code 市场订阅

用户在 Claude Code `settings.json` 中添加：

```json
{
  "extraKnownMarketplaces": {
    "shokz-tools": {
      "source": {
        "source": "github",
        "repo": "Inno-Shokz/Skill_shokz-diary"
      },
      "autoUpdate": true
    }
  },
  "enabledPlugins": {
    "shokz-diary@shokz-tools": true
  }
}
```

更新方式：

1. 维护者更新本仓库版本。
2. 用户重启 Claude Code 或等待 marketplace autoUpdate。
3. 新线程使用更新后的 commands、skills 和 agents。

## Codex 市场订阅

Codex marketplace 需要一个 marketplace 根目录，其结构应类似：

```text
shokz-marketplace/
├── .agents/
│   └── plugins/
│       └── marketplace.json
└── plugins/
    └── shokz-diary/
        ├── .codex-plugin/
        │   └── plugin.json
        ├── skills/
        ├── commands/
        ├── agents/
        └── README.md
```

`shokz-marketplace/.agents/plugins/marketplace.json` 示例：

```json
{
  "name": "shokz-tools",
  "interface": {
    "displayName": "Shokz Tools"
  },
  "plugins": [
    {
      "name": "shokz-diary",
      "source": {
        "source": "local",
        "path": "./plugins/shokz-diary"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

用户首次订阅非默认团队 marketplace：

```bash
codex plugin marketplace add <path-to-shokz-marketplace>
codex plugin add shokz-diary@shokz-tools
```

默认个人 marketplace 位于 `~/.agents/plugins/marketplace.json`，Codex 会隐式发现该路径；个人 marketplace 不需要执行 `codex plugin marketplace add`。

## Codex 更新流程

维护者发布新版本后：

1. 确认 `.codex-plugin/plugin.json` 版本已更新。
2. 确认 marketplace 指向的 `plugins/shokz-diary` 已更新到新源码。
3. 用户运行：
   ```bash
   codex plugin add shokz-diary@shokz-tools
   ```
4. 用户开启新线程验证更新后的 skill 和 commands。

本地开发迭代时，如果版本号不变，可使用 Codex cachebuster 版本，例如 `0.3.0+codex.local-YYYYMMDD-HHMMSS`，再重新安装插件。

## 发布前检查清单

每次发布前运行：

```bash
python "%USERPROFILE%\.codex\skills\.system\plugin-creator\scripts\validate_plugin.py" .
python "%USERPROFILE%\.codex\skills\.system\skill-creator\scripts\quick_validate.py" skills\diary-generation
git diff --check
```

检查项：

- `.codex-plugin/plugin.json` 通过插件校验。
- `.claude-plugin/marketplace.json`、`.claude-plugin/plugin.json`、`.codex-plugin/plugin.json` 版本一致。
- README 和 CHANGELOG 中的当前版本一致。
- 不提交 `.shokz/` 中的真实学员数据。
- 新用户使用 `/shokz-diary:shokz-init` 可创建本地 profile、learner-profile 和 index。
