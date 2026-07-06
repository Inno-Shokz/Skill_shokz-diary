# Changelog

## 0.3.0 (2026-07-06)

- 升级日报形态：从旧版 A/B/C/D 复盘改为企业级可提交日报正文
- 新增原始对话附录：完整保留学员原话、事实确认和 Agent 追问
- 新增事实清单确认：生成日报前先确认事实边界，降低信息失真
- 新增高风险追问：遇到“无卡点”“很顺利”“没问题”等回答时必须追问一次
- 新增连续学员画像：通过 `.shokz/learner-profile.md` 维护可修正的动态画像
- 新增导航索引：通过 `.shokz/index.md` 维护日报、问题类型、导师反馈、未关闭事项和沉淀候选
- 新增画像/索引更新建议：默认只提出建议，学员确认后才写入长期记录
- 校准流程升级：支持日报正文校准和画像校准，禁止反向改写历史原话
- Codex manifest 升级为可通过插件校验的市场元数据结构
- 新增 `docs/marketplace.md`，说明 Claude/Codex 市场订阅、下载、使用和更新流程
- 更新 README、日报模板、访谈代理和插件版本号

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
