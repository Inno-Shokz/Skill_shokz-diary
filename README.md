# Shokz Diary Skill

实习生每日复盘 Agent 插件。通过 AI 交互式提问引导学员完成每日复盘，自动生成结构化日报文档。

## 解决什么问题

- 学员不知道怎么写日报，写出来的是流水账
- 导师每天花大量时间读流水账、追问细节
- Agent 代替导师完成第一轮 coaching，输出导师可直接批阅的结构化日报

## 安装

在 Claude Code 的 `settings.json` 中添加以下配置：

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

添加后重启 Claude Code，插件自动生效。后续更新会在每次启动时自动拉取。

## 使用

### 1. 初始化（首次使用）

```
/shokz-diary:shokz-init
```

设置姓名、导师、实习目标、日报输出路径。配置保存在当前目录的 `.shokz/profile.json`。

### 2. 每日复盘

```
/shokz-diary:shokz-diary
```

Agent 会：
1. 一次性提出三个问题（做了什么 / 卡在哪里 / 想讨论什么）
2. 根据你的回答生成导师级深度追问
3. 自动生成结构化日报并保存为 .md 文件

### 3. 校准修改

```
/shokz-diary:shokz-calibrate
```

对当天已生成的日报不满意时，指出需要修改的部分，Agent 重新生成。

## 日报输出结构

```
├── 元信息（姓名/导师/日期/用时）
├── 对话记录（Agent 提问 + 学员回答完整原文）
├── AI 输出区
│   ├── A. 差距识别
│   ├── B. 思路拓展
│   ├── C. 明日行动方法
│   └── D. 导师沟通要点
└── 导师反馈区
```

## 版本

当前版本：`0.1.0`

查看 [CHANGELOG](plugins/shokz-diary/CHANGELOG.md) 了解版本历史。
