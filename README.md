# TestDSHSuperAgent

本仓库是 [DSH SuperAgent](https://github.com/deepseek-ai/deepseek-harness) 的端到端测试仓库，用于验证 DSH SuperAgent 的各项特性。DSH SuperAgent 是 DeepSeek Harness（DSH）中负责接收任务、调度工作区并交付改动的自动化代理。

本仓库不承载业务代码，专门用来演练和验证「GitHub 任务 → 隔离工作区 → 分支提交 → Pull Request → 人工合并」的完整链路，让接入、调度与交付流程可以在不影响真实项目的前提下反复测试。

## 验证的 DSH SuperAgent 特性

本仓库是 DSH SuperAgent 的功能验证沙盒：每个特性对应一条可重复执行的端到端流程，验证结果以提交和 PR 的形式留痕，便于回归测试和人工复核。

| 验证项 | 验证方式 | 期望结果 |
| --- | --- | --- |
| 仓库接入与任务触发 | 向 DSH SuperAgent 下发一个针对本仓库的任务（如更新 README） | Agent 能识别任务、定位仓库并开始执行 |
| 隔离工作区 | 观察任务执行时使用的分支 / worktree | 每个任务在独立工作区执行，互不影响 |
| 文档与代码修改 | 让 Agent 按约定修改 README 或最小示例 | 改动范围可控，内容符合任务要求 |
| 自动提交 | 检查任务产生的 commit | 提交信息清晰，只包含本次任务的改动 |
| PR 创建与描述 | 检查自动创建的 PR 的 base/head、标题和正文 | PR 可直接 review，正文说明改动内容与验证方式 |
| 自动化标注 | 检查 PR 正文或评论 | 明确标注由自动化 agent 创建，便于与人工操作区分 |
| 人工审核与合并 | 由人工 review 后手动合并或关闭 PR | 流程闭环，不启用 auto-merge |
| 多任务并行 | 同时下发多个互不依赖的文档任务 | 各任务分支独立，互不覆盖 |

> 本仓库只承载文档和最小示例，目的是让每次验证的 diff 足够小、结果足够直观，不用于验证业务功能或性能。

## 项目用途

- 验证 GitHub 仓库接入、任务触发与任务分派是否正常。
- 验证每个任务在独立分支 / worktree 中执行，互不干扰。
- 验证自动提交、PR 创建、PR 描述以及人工 review 的协作流程。
- 作为改动流程的沙盒：可以安全地合入测试提交，也可以直接关闭测试 PR。

## 仓库内容

当前仓库只有本 README，没有源码、依赖或构建脚本。后续如有需要，可以在这里加入最小示例或演示脚本。

## 基本使用

### 1. 克隆仓库

```bash
git clone https://github.com/jpxiong/TestDSHSuperAgent.git
cd TestDSHSuperAgent
```

### 2. 查看当前状态与提交历史

```bash
git status
git log --oneline --decorate -10
```

### 3. 提交一次改动

仓库的改动统一通过「分支 + PR」进行：

```bash
git switch -c docs/update-readme
# 编辑文件……
git add README.md
git commit -m "docs: update README"
git push -u origin docs/update-readme

gh pr create \
  --base main \
  --head docs/update-readme \
  --title "docs: update README" \
  --body "说明本次改动"
```

PR 创建后等待人工审核与合并，不要开启 auto-merge。

### 4. 运行与测试

仓库目前没有可执行程序、依赖安装步骤或自动化测试命令，克隆后直接阅读即可。若后续加入代码，请在这里补充对应的安装、运行和测试命令。

## 协作约定

- `main` 分支保持可读，所有改动通过 PR 提交。
- PR 一律由人工审核并手动合并。
- 由自动化 agent 创建的 PR 和评论会明确标注，避免与人工操作混淆。
- 不要在仓库中提交密钥、访问令牌或其他敏感信息。
