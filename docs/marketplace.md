# Marketplace 发布与更新说明

本文说明如何让学员直接通过当前仓库订阅、下载、使用和更新 `shokz-diary`。

## 结构原则

当前仓库是 **plugin root**，不是“marketplace 包含 plugin”的二层结构。

```text
Skill_shokz-diary/
├── .claude-plugin/plugin.json
├── .claude-plugin/marketplace.json
├── .codex-plugin/plugin.json
├── .agents/plugins/marketplace.json
├── skills/
├── commands/
└── agents/
```

关键约束：

- 插件源码位于仓库根目录，不放入 `plugins/shokz-diary/`。
- `.claude-plugin/marketplace.json` 是 Claude Code 订阅入口，`plugins[].source` 指向 `./`。
- `.agents/plugins/marketplace.json` 是 Codex CLI 订阅入口，`plugins[].source.url` 指向 `./`。
- `.codex-plugin/plugin.json` 是 Codex 插件 manifest。
- `skills/`、`commands/`、`agents/` 是 Claude Code 与 Codex CLI 共享能力。
- `.shokz/`、`.claude/` 是本地运行期数据或工具缓存，不提交到仓库。

## Claude Code 安装

学员执行：

```text
/plugin marketplace add Inno-Shokz/Skill_shokz-diary
/plugin install shokz-diary@shokz-tools
```

或在 `settings.json` 中配置：

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

## Codex CLI / Codex App 安装

学员执行：

```bash
codex plugin marketplace add Inno-Shokz/Skill_shokz-diary
codex plugin add shokz-diary@shokz-tools
```

也可以在 Codex CLI 中打开：

```text
/plugins
```

然后搜索 `shokz-diary` 并安装。

## 使用

新线程中使用：

```text
/shokz-diary:shokz-init
/shokz-diary:shokz-diary
/shokz-diary:shokz-calibrate
```

## 更新流程

维护者发布新版本：

1. 修改 `skills/`、`commands/`、`agents/` 等插件源码。
2. 同步更新 `package.json`、`.codex-plugin/plugin.json`、`.claude-plugin/plugin.json`、`.claude-plugin/marketplace.json` 版本号。
3. 如 Codex marketplace 元数据变化，同步更新 `.agents/plugins/marketplace.json`。
4. 提交并推送到 GitHub。

学员更新：

```bash
codex plugin marketplace upgrade shokz-tools
codex plugin add shokz-diary@shokz-tools
```

Claude Code 用户若配置了 `autoUpdate`，通常重启后会获取更新；否则重新执行：

```text
/plugin install shokz-diary@shokz-tools
```

## 什么时候才需要 `plugins/shokz-diary/`

只有在维护一个集中 marketplace 仓库、并且该仓库同时分发多个插件时，才需要：

```text
some-marketplace/
└── plugins/
    └── shokz-diary/
```

当前仓库只分发 `shokz-diary` 一个插件，应保持和 `obra/superpowers` 一样的根插件结构。

## 发布前检查清单

每次发布前运行：

```bash
npm run validate:plugin
npm run validate:skill
git diff --check
```

检查项：

- 不存在 `plugins/shokz-diary/` 子目录。
- `.claude-plugin/marketplace.json` 的 `source` 指向 `./`。
- `.agents/plugins/marketplace.json` 的 `source.url` 指向 `./`。
- `.codex-plugin/plugin.json` 通过插件校验。
- `package.json`、README、CHANGELOG 和 manifest 版本一致。
- 不提交 `.shokz/` 中的真实学员数据。
