# Herdr Coordinator

一个由用户显式启用的多 Agent 协调技能。协调者通过 Herdr Core 安排有依赖的任务，把实现和验证交给交互式执行者，再按证据签收。任务记录和稳定终端地址支持会话恢复与接班。

## 安装

准备 Herdr Core 的 `hc` 命令，以及已配置的 Oh My Pi 或 Claude Code。用 `hc --skill` 查看当前版本的命令。需要在桌面查看执行者时，可连接 Herdr.app。[Herdr Core](https://github.com/herdrdev/herdr-core) · [Herdr.app](https://github.com/herdrdev/herdr-app)

克隆或下载本仓库，然后把技能复制到要协调的项目中。替换下面的两个绝对路径。Claude Code 用户将 `skill_root` 改为 `.claude/skills`。

```sh
(
  set -eu
  repo=/path/to/herdr-coordinator-skill
  project=/path/to/your-project
  skill_root=.omp/skills
  dest="$project/$skill_root/herdr-coordinator"
  mkdir -p "$(dirname "$dest")"
  mkdir "$dest"
  cp "$repo/SKILL.md" "$dest/"
  cp -R "$repo/references" "$dest/"
)
```

目标技能目录须尚不存在。已有同名技能时，先检查宿主的加载位置，避免调用到旧版。

## 使用

在 Herdr Core 管理的终端中打开项目并启动宿主。从外部终端启动时，以下命令会返回稳定终端地址。用 `hc attach` 连接该地址。Claude Code 用户将 `omp` 换成 `claude`。

```sh
hc run --bg --name coordinator --cwd "/path/to/your-project" -- omp
```

在宿主交互界面中调用技能，并附上任务或计划路径。

| 宿主 | 输入示例 | 文档 |
| --- | --- | --- |
| Oh My Pi | `/skill:herdr-coordinator 协调 docs/export-plan.md 中的工作` | [Skills](https://github.com/can1357/oh-my-pi/blob/main/docs/skills.md) |
| Claude Code | `/herdr-coordinator 协调 docs/export-plan.md 中的工作` | [Skills](https://code.claude.com/docs/en/skills) |

Oh My Pi 的斜杠入口需要启用 `skills.enableSkillCommands`。也可以在提示中明确写「读取并启用 `herdr-coordinator`，协调 `docs/export-plan.md` 中的工作」。需要指定执行程序、模型或推理级别时，把它们写进同一条提示。

协调者用 `brief.md` 记录目标、依赖和分工，用 `status.md` 记录回报地址与任务状态。执行者通过 `hc send` 回报摘要路径。恢复或换人时，把工作记录目录的绝对路径交给协调者，先核对在途任务及其地址，再继续派发。[协调流程](SKILL.md) · [工作记录模板](references/work-records.md)

## 原始分享

> Tinyhost · 《Herdr Coordinator：一份“只协调、不执行”的多 Agent 编排提示词》。[原始分享与提示词全文](https://tinyhost.space/herdr-coordinator/)

本仓库沿用了原文中的协调者分工思路，按 Herdr Core 的 `hc` 重新编写了派发、等待、签收和交接流程。公开发布前需确认改编内容的再分发授权和本仓库的许可证。
