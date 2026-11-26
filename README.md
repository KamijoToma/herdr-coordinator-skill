# Herdr Coordinator

由用户显式启用的协调技能。协调者用 Herdr Core 派发交互式执行者，记录任务依赖和文件归属，以交付证据验收，在会话恢复或接班后按稳定终端地址继续工作。

## 安装

需要 Herdr Core 的 `hc` 命令和一个已配置的交互式 agent 宿主。需要在桌面展示执行者时，可使用 Herdr.app。[Herdr Core](https://github.com/herdrdev/herdr-core) · [Herdr.app](https://github.com/herdrdev/herdr-app)

将技能装进要协调的项目。把下方的 `repo` 和 `project` 换成实际绝对路径。以下以 Oh My Pi 为例。Claude Code 使用 `.claude/skills` 作为安装目录。

```sh
(
  set -eu
  repo=/path/to/herdr-coordinator
  project=/path/to/your-project
  dest="$project/.omp/skills/herdr-coordinator"
  mkdir -p "$(dirname "$dest")"
  mkdir "$dest"
  cp "$repo/SKILL.md" "$dest/"
  cp -R "$repo/references" "$dest/"
)
```

安装命令要求目标技能目录尚不存在。已有同名副本时，先确认宿主将从哪个目录加载。

## 使用

在项目目录内，从 Herdr Core 管理的交互式终端启动宿主。若从外部启动，可用 `hc run --bg --name coordinator --cwd "$project" -- omp`，再用 `hc attach` 连接返回的地址。Claude Code 用户将启动程序换成 `claude`。

| 宿主 | 输入示例 | 文档 |
| --- | --- | --- |
| Oh My Pi | `/skill:herdr-coordinator 协调 docs/export-plan.md 中的工作` | [Skills](https://github.com/can1357/oh-my-pi/blob/main/docs/skills.md) |
| Claude Code | `/herdr-coordinator 协调 docs/export-plan.md 中的工作` | [Skills](https://code.claude.com/docs/en/skills) |

Oh My Pi 的斜杠入口需要启用 `skills.enableSkillCommands`。也可直接告诉宿主读取 `herdr-coordinator` 技能并显式启用，附上真实目标或计划路径。没有指定模型时，沿用用户已有的交互式配置。接班时把工作记录目录的绝对路径交给继任协调者。

[SKILL.md](SKILL.md) 定义协调流程。[工作记录模板](references/work-records.md) 提供任务单、状态和执行者摘要格式。

## 来源

协调者角色的设计参考了 Tinyhost 的 Herdr Coordinator。这里的运行流程对应 Herdr Core 的 `hc`，具体命令以安装版本的 `hc --skill` 为准。[Herdr Coordinator 原始分享](https://tinyhost.space/herdr-coordinator/)
