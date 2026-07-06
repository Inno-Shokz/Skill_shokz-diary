# Shokz Diary Skill

实习生每日复盘 Agent 插件。通过 AI 交互式访谈、事实确认和导师视角结构化，生成企业级可提交日报，并持续维护学员动态画像和团队问题索引。

## 解决什么问题

- 学员日报容易变成流水账，导师读完仍要追问。
- 学员原话在 AI 润色中被压缩或失真，真实沟通问题被隐藏。
- 团队成员节奏松散，缺少固定检查、复盘和监督机制。
- 复盘中反复出现的问题没有沉淀为可复用团队文档。

## 核心机制

- **企业级日报正文**：给导师看的提交版，聚焦推进、结论、风险、明日计划和需确认事项。
- **附录原始对话**：完整保留学员原话、事实确认和 Agent 追问，避免信息失真。
- **事实清单确认**：日报生成前先让学员确认事实边界，防止 Agent 脑补。
- **高风险追问**：遇到“无卡点”“很顺利”“没问题”等回答时，必须追问认知卡点或待验证结论。
- **连续学员画像**：生成画像更新建议，经学员确认后写入 `.shokz/learner-profile.md`。
- **导航索引**：经确认后维护 `.shokz/index.md`，沉淀日报、导师反馈、未关闭事项和团队文档候选。

## 安装

### Claude Code

在 `settings.json` 中添加：

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

settings.json 位置：

- Windows: `C:\Users\<用户名>\.claude\settings.json`
- macOS: `~/.claude/settings.json`

添加后重启 Claude Code，插件自动生效。后续更新自动拉取。

### Codex CLI

Codex 插件市场需要一个个人或团队 marketplace 指向本插件源码。团队发布时，将本仓库放入 marketplace 根目录的 `plugins/shokz-diary/`，并在 `.agents/plugins/marketplace.json` 中添加 `shokz-diary` 条目。

用户安装：

```bash
codex plugin add shokz-diary@shokz-tools
```

详细发布、订阅和更新流程见 [Marketplace 发布与更新说明](docs/marketplace.md)。

## 使用

### 1. 初始化（首次使用）

```text
/shokz-diary:shokz-init
```

设置姓名、导师、入职日期、核心实习目标和日报输出路径。

初始化会创建本地用户数据：

- `.shokz/profile.json`：稳定配置，只存姓名、导师、目标等基础信息。
- `.shokz/learner-profile.md`：动态学员画像模板。
- `.shokz/index.md`：日报导航和问题索引模板。
- `.shokz/reports/`：日报输出目录。

`.shokz/` 是本地数据目录，不属于插件仓库提交内容。

### 2. 每日复盘

```text
/shokz-diary:shokz-diary
```

Agent 会：

1. 读取 profile、动态画像、索引、最近日报和未关闭事项。
2. 收集学员当天原始输入，并完整保留原话。
3. 抽取事实清单，让学员确认是否遗漏或失真。
4. 遇到“无卡点 / 很顺利 / 没问题”等高风险回答时追问一次。
5. 生成企业级日报正文并保存为 Markdown。
6. 在附录中保留完整原始对话。
7. 生成画像和索引更新建议，只有学员确认后才写入长期记录。

### 3. 校准修改

```text
/shokz-diary:shokz-calibrate
```

用于校准当天日报或动态画像：

- 修改日报正文中的事实、措辞、来源标记、风险和明日计划。
- 修改、删除、降级或标记过期画像观察。
- 不允许反向改写历史原话；附录原始对话保持保真。

## 日报输出结构

```text
├── 1. 基本信息
├── 2. 今日推进
├── 3. 关键结论
├── 4. 卡点与风险
├── 5. 明日计划
├── 6. 需导师确认
├── 7. 管理视角补充
├── 8. 问题沉淀候选
├── 9. 导师反馈区
├── 附录 A：原始对话记录
└── 附录 B：画像/索引更新建议
```

正文来源类型固定为：

- `学生原话`
- `追问补充`
- `Agent 推断`
- `待确认`

## 质量红线

- 不压缩学生原话为摘要。
- 不把 Agent 推断写成事实。
- 不接受空泛输入直接生成日报。
- 不为了让日报好看而隐藏学生表达问题。
- 不未经确认写入动态画像。
- 不把一次表现永久标签化。

## 支持平台

| 平台 | 安装方式 | 状态 |
|------|---------|------|
| Claude Code | settings.json 配置 + autoUpdate | ✅ 支持 |
| Codex CLI | /plugins 安装 | ✅ 支持 |
| Cursor | 待适配 | 🔜 计划中 |

## 版本

当前版本：`0.3.0`

查看 [CHANGELOG](CHANGELOG.md) 了解版本历史。
