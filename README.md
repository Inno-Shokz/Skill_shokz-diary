# Shokz Diary Skill

实习生每日复盘 Agent 插件。通过固定三轮输入、原始表达保真和导师视角结构化，生成企业级可提交日报，并持续维护学员动态画像和团队问题索引。

## 解决什么问题

- 学员日报容易变成流水账，导师读完仍要追问。
- 学员原话在 AI 润色中被压缩或失真，真实沟通问题被隐藏。
- 团队成员节奏松散，缺少固定检查、复盘和监督机制。
- 复盘中反复出现的问题没有沉淀为可复用团队文档。

## 核心机制

- **开放承接式三轮输入**：先问“今天做了什么”，再承接已出现的卡点，最后承接导师讨论点；不让学员重复叙述。
- **AI 输出区**：自动生成差距识别、思路拓展、明日行动方法和导师沟通要点。
- **附录保真追溯**：完整保留三轮原话，并标注时间归属和事实来源，避免信息失真。
- **草稿确认保存**：AI 先展示草稿，学员回复“确认保存”后才写入日报文件。
- **连续学员画像**：生成画像更新建议，经学员确认后写入 `.shokz/learner-profile.md`。
- **导航索引**：经确认后维护 `.shokz/index.md`，沉淀日报、导师反馈、未关闭事项和团队文档候选。

## 安装

本仓库采用和 `obra/superpowers` 相同的单仓根插件结构：仓库根目录就是 `shokz-diary` 插件，`skills/`、`commands/`、`agents/` 为多平台共享核心，`.claude-plugin/`、`.codex-plugin/`、`.agents/plugins/` 只是平台分发适配层。

### Claude Code

方式一：直接把当前仓库作为 marketplace 加入 Claude Code：

```text
/plugin marketplace add Inno-Shokz/Skill_shokz-diary
/plugin install shokz-diary@shokz-tools
```

方式二：在 `settings.json` 中添加：

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

直接把当前仓库作为 marketplace 加入 Codex，再从该 marketplace 安装根插件：

```bash
codex plugin marketplace add Inno-Shokz/Skill_shokz-diary
codex plugin add shokz-diary@shokz-tools
```

在 Codex CLI 中也可以先打开 `/plugins`，再搜索 `shokz-diary` 安装。

详细发布、订阅和更新流程见 [Marketplace 发布与更新说明](docs/marketplace.md)。

## 项目结构

```text
Skill_shokz-diary/
├── .claude-plugin/                 # Claude Code manifest 与 marketplace 配置
├── .codex-plugin/                  # Codex CLI plugin manifest
├── .agents/plugins/                # Codex marketplace 订阅入口
├── skills/diary-generation/        # 核心日报生成 skill
├── commands/                       # Claude + Codex 共享命令
├── agents/                         # 学员侧访谈代理定义
├── schemas/                        # 本地配置 schema
├── hooks/                          # 可选会话钩子配置
├── README.md
├── CHANGELOG.md
└── package.json                    # 版本与校验脚本
```

仓库中不包含 `plugins/shokz-diary/` 子目录；Claude 与 Codex 的 marketplace `source` 都指向 `./`，即当前仓库根插件。

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
2. 开放承接式三轮收集输入：今天做了什么、哪里卡住/感觉不对、想和导师讨论什么。
3. 如果学员提前说到卡点或导师讨论点，Agent 会复述确认并开放补充，不用选择题收束。
4. 三轮结束后才进入 AI 处理，区分今日发生、跨日补充、历史背景和导师反馈事件。
5. 生成 A/B/C/D 输出区：差距识别、思路拓展、明日行动方法、导师沟通要点。
6. 展示完整草稿，只有学员回复“确认保存”后才写入 Markdown。
7. 在附录中保留三轮原话和事实边界说明。
8. 生成画像和索引更新建议，只有学员回复“确认写入”后才写入长期记录。

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
├── 基本信息
├── 固定锚点：我的实习目标
├── 输入区：三轮原始输入
├── AI 输出区
│   ├── A. 差距识别
│   ├── B. 思路拓展
│   ├── C. 明日行动方法
│   └── D. 导师沟通要点
├── 导师反馈区
├── 附录 A：事实边界与来源说明
└── 附录 B：画像/索引更新建议
```

来源类型固定为：

- `学生原话`
- `历史上下文`
- `Agent 推断`
- `待确认`

时间归属固定为：

- `今日发生`
- `跨日补充`
- `历史背景`
- `导师反馈事件`
- `待确认`

## 质量红线

- 不压缩学生原话为摘要。
- 不在三轮输入阶段提前分析。
- 不用选择题、判断题或原因分析替学员收束表达。
- 不把跨日事件写成今日发生。
- 不把 Agent 推断写成事实。
- 不把历史上下文或 Agent 推断标成学生原话。
- 不为了让日报好看而隐藏学生表达问题。
- 不未经“确认保存”写入日报。
- 不未经确认写入动态画像。
- 不把一次表现永久标签化。

## 支持平台

| 平台 | 安装方式 | 状态 |
|------|---------|------|
| Claude Code | `/plugin` marketplace 安装 | ✅ 支持 |
| Codex CLI | `codex plugin` marketplace 安装 | ✅ 支持 |
| Cursor | 待适配 | 🔜 计划中 |

## 版本

当前版本：`0.4.0`

查看 [CHANGELOG](CHANGELOG.md) 了解版本历史。
