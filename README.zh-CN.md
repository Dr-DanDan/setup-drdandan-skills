# Setup DrDanDan Skills

[English](README.md) | [简体中文](README.zh-CN.md)

一个为 DrDanDan 工程工作流初始化仓库的 Codex 技能，统一配置 Git 身份、
`src/`、`.gitignore`、Agent 工作规范、实验目录、Issue 管理、分诊标签、
领域文档规则和 `TODO.md`。

## 配置内容

- 仓库级 Git 用户名和邮箱、根目录 `src/` 及 `.gitignore`。
- `AGENTS.md` 中的工作范围约束和工作流入口。
- GitHub、GitLab、本地 Markdown 或用户指定的 Issue 管理方式。
- 分诊标签：仅在安装 `triage` 技能时配置。
- 领域上下文与架构决策记录（ADR）的读取规则。
- 根目录 `TODO.md`：记录当前任务的优先级和开发进度。
- 实验目录选择、文件归属、日志保存和图表来源追溯规则。
- 飞书票据同步约定与初始未绑定的母文档配置，供配套的 `lark-push-tickets` / `lark-pull-tickets` 使用；setup 阶段不要求飞书登录。
- 可选的 `DDDvis` 项目技能及已有共享 HTML 展示服务绑定。

开始实验开发、运行、调试或整理前，生成的规范要求询问：在 `experiments/`
下新建目录，还是复用已有目录。用户已明确指定的目录，或同一任务中已确认的目录，
直接沿用，不重复询问。普通业务代码修改不触发此步骤。

```text
experiments/<实验名称>/
|-- codebase/           # 实验专属代码和配置
|-- numerical_results/ # 数值结果、汇总表和运行清单
|-- illustrations/     # 由数值结果生成的图表
|-- tests/             # 测试及调试辅助文件
|-- log/               # 长时间运行任务的日志和进度记录
`-- README.md          # 图表、数值来源与运行标识的对应关系
```

可复用的生产代码放在根目录 `src/`。数值结果、图表和日志目录加入 Git 忽略。
运行程序自行将日志、时间戳、进度和失败记录持久化到 `log/`，按运行标识区分，
避免覆盖历史记录。

这是由提示词驱动的初始化技能，执行流程为：检查仓库、确认选择、展示草稿、
写入并验证。它保留已有文件和项目约定。领域文档和具体实验在需要时创建；
初始化阶段将相应规范写入 `docs/agents/`。

## 可选的 DDDvis 绑定

选择接入后，setup 写入 `.agents/skills/dddvis/`、AGENTS 中的条件入口和
`docs/agents/dddvis.md`。仅明确调用 `DDDvis` 才发布网站报告；普通 HTML
生成和 `visualize` 保持各自的交付方式。重复 setup 保留绑定与用户修改。

同一服务器复用展示服务，报告按项目归类。项目绑定指向稳定的服务配置位置，
最新网址从服务维护的记录读取，不把临时域名写死在项目规则中。
报告可为自包含 HTML 或带必要资源的静态目录；原始产物保留，同报告更新保留路径。

配套约定包括独立后台恢复：60 秒检查、连续 3 次异常、每轮最多修复 2 次、
失败后冷却 600 秒，并尊重人工停用。**本仓库提供 setup 说明与项目技能模板，
不包含网站服务器、Tunnel 或恢复守护程序。** setup 会说明已有服务缺少的能力，
不会把写入规则当作已经安装实现。临时域名更换后，旧书签和聊天链接不会自动更新。

## 安装

使用 Codex 内置的 `$skill-installer`，指定仓库 `Dr-DanDan/setup-drdandan-skills`
和技能路径 `skills/setup-drdandan-skills`。也可以在 PowerShell 7 中执行：

```powershell
$codexSkillsRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$installerPath = Join-Path $codexSkillsRoot 'skills\.system\skill-installer\scripts\install-skill-from-github.py'
python $installerPath --repo Dr-DanDan/setup-drdandan-skills --path skills/setup-drdandan-skills
if ($LASTEXITCODE -ne 0) { throw "技能安装失败，退出码：$LASTEXITCODE" }
```

安装完成后，新建一个 Codex 任务并输入：

```text
$setup-drdandan-skills
```

技能会在修改目标仓库之前请求确认。

## 使用要求

- 带有内置 `$skill-installer` 的 Codex。
- Git。
- 使用上述安装命令时，需要 Python 和 PowerShell 7。

## 许可证与来源

本项目采用 MIT 许可证。该技能基于 Matt Pocock 的
[`setup-matt-pocock-skills`](https://github.com/mattpocock/skills/tree/5c89081d4bbeb3d039a42093653f90bb698d780e/skills/engineering/setup-matt-pocock-skills)
改编，详见 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)。
