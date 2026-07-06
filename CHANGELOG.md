# Changelog

## 0.2.0 (2026-07-06)

- 架构重构：采用 Superpowers 模式，仓库即插件
- 新增 Codex CLI 支持（`.codex-plugin/plugin.json`）
- 扁平化目录结构，skills/commands/agents 提至根目录
- 交互改为纯对话模式（移除 AskUserQuestion）
- 追问策略升级：导师级深度追问（非形式追问）
- 日报新增「对话记录」区域 + 计时器
- init 支持自定义输出路径

## 0.1.0 (2026-07-05)

- Initial release
- `shokz-init`: 学员 profile 初始化
- `shokz-diary`: 交互式每日复盘生成
- `shokz-calibrate`: 日报质量校准修改
