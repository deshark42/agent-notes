# Agent Notes

Agent Note 是 agent 编写的 RFC 式文档，记录一项长期决策背后的理由和被否决的备选方案。

本仓库把 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（DSH）的 Agent Notes 系统整理成模板，供其他代码项目引入。契约见 [`.agents/notes/README.md`](.agents/notes/README.md)。

相比 DSH 原版，这里删去了 DSH 专用内容（包路径、pnpm/run-gates、GH stack，技能去 `dsh-` 前缀），改用 `npm run verify` 聚合门禁，只保留中文契约，根 `AGENTS.md` 重写为中立版本。

## 模板包含

```text
AGENTS.md       agent 指令：契约指针、命令、检查选择
.agents/notes/  笔记树、契约与生命周期/类别说明
.agents/skills/ 归档工作流、推送前检查选择策略
scripts/        门禁：笔记路径/类别、头部/章节、封存归档
package.json    npm run verify 聚合脚本，每个门禁一个脚本
```

笔记路径为 `{lifecycle}/{class}/yyyy-mm-dd-topic-title.md`：

- `proposed/` —— 实施前评审中的提案。
- `implemented/` —— 已交付的决策，并与代码事实保持同步。
- `rejected/` —— 被否决的提案；仅当其理由仍能阻止一个诱人的错误时保留。
- `archived/{class}/` —— 已封存的 implemented 笔记；永久冻结，记录在 `manifest.json`。

## 使用方法

有两种方式：

1. **从模板新建仓库**：点击 **Use this template** → **Create a new repository**，或直接访问 [github.com/deshark42/agent-notes/generate](https://github.com/deshark42/agent-notes/generate)，得到一个包含完整 Agent Notes 系统的新仓库。
2. **复制到已有仓库**：把本仓库的所有文件复制进目标仓库；目标仓库已有 `README.md` 或 `AGENTS.md` 时合并内容，不要直接覆盖。

完成后：

1. 重写 `AGENTS.md` 的开头段落，描述目标项目；把本 README 替换为目标项目自己的说明。
2. 运行 `npm run verify`。需要 Node 22.18 或更新版本，因为门禁直接执行 TypeScript。
3. 随着项目出现自己的检查，扩展 `.agents/skills/pre-push-checks/SKILL.md` 的改动范围表格，并在项目具备 CI 后接入。

## 命令

```sh
npm run verify   # 全部门禁：路径/类别、头部/章节、封存归档
```

- `npm run verify-agent-note-classification` —— 笔记路径与类别。
- `npm run verify-agent-note-format` —— 笔记头部与必需章节。
- `npm run verify-archived-agent-notes` —— 封闭归档树、元数据与仅追加的 manifest。
- `npm run verify-archived-agent-notes -- --write` —— 归档后把新的封存记录追加到 `.agents/notes/archived/manifest.json`；不要手改该文件。

## 编写与归档

写笔记前先读契约：[何时需要写一份](.agents/notes/README.md#when-to-write-one)、[分类](.agents/notes/README.md#classification)、[文件格式](.agents/notes/README.md#the-file-format)。

归档遵循 [`.agents/skills/archive-agent-notes/SKILL.md`](.agents/skills/archive-agent-notes/SKILL.md)，最后执行 `--write` 和 `npm run verify`。归档后的笔记永久冻结；不要编辑、移动或删除，也不要将其视为当前行为的权威依据。

推送前遵循 [`.agents/skills/pre-push-checks/SKILL.md`](.agents/skills/pre-push-checks/SKILL.md)，只选择覆盖本次改动的检查，并只报告实际执行过的命令。

## 维护本模板

Agent Notes 系统从 deepseek-harness vendor 而来。`source` 分支按上游路径保存上游原文；`master` 保存模板适配。同步时，先把新的上游文件 vendor 到 `source`，再把 `source` merge 进 `master`，然后单独提交重新应用适配。不要直接编辑 `source` 上的上游副本。
