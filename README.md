# chi-paper + essay-exam for Claude Code

[中文说明](README.zh-CN.md)

## Purpose / 用途

**English**

This public repository contains the overview. The private core contains the usable skills.

**中文**

本公开仓库提供介绍。私有核心仓库提供可使用的技能。

## Capability / 能力

**English**

Two skills. chi-paper: ACM SIGCHI venue rules, compliance and quality passes, mock review, revise-and-resubmit. essay-exam: a calibrated six-dimension prose examination with a pass/review/fail gate.

**中文**

人机交互论文。聚焦研究目标、证据、论证、稿件修订和会议规则。 学术文本检查。使用校准资源和可定位的检查结果。

## Repository pair / 双仓

**English**

[Public overview](https://github.com/Shawn200212/ClaudeEssaySkill) · [Private core](https://github.com/Shawn200212/ClaudeEssaySkill-core)

**中文**

[公开介绍](https://github.com/Shawn200212/ClaudeEssaySkill) · [私有核心](https://github.com/Shawn200212/ClaudeEssaySkill-core)

## Installation / 安装

**English**

Clone the private core with an account that has access. Run the included setup preview before installation.

```text
git clone https://github.com/Shawn200212/ClaudeEssaySkill-core.git
cd ClaudeEssaySkill-core
python install.py
python install.py --apply
```

**中文**

使用有权限的账户拉取私有核心仓库。先预览安装，再应用更改。

## Invocation / 调用

**English**

After installation, invoke `/chi-paper-essay-exam:chi-paper, /chi-paper-essay-exam:essay-exam` in Claude Code. Read the setup output for unresolved dependencies.

**中文**

安装后，在 Claude Code 中调用 `/chi-paper-essay-exam:chi-paper, /chi-paper-essay-exam:essay-exam`。检查安装输出中的未满足依赖。

## Prerequisites / 依赖

**English**

The package includes instructions and required small resources. External applications, project files and paper corpora remain separate prerequisites.

**中文**

包内含技能指令和必要的小型资源。外部应用、项目文件和论文库需要另行准备。

## Verification / 核查

**English**

The release records structural checks, relevant local tests and bounded writing trials. These checks do not certify every workflow or model.

**中文**

本次发布记录结构检查、相关本地测试和有限写作试验。这些检查不能证明全部流程或模型均已通过。

## Limits / 边界

**English**

No acceptance, authorship or universal quality guarantee is made. Live application operations require their own prerequisites and verification.

**中文**

本技能不保证录用、作者身份判断或通用写作质量。实际应用操作需要满足依赖并另行验证。

## Plugin / 插件

**English**

After setup, load this checkout as a plugin: `claude --plugin-dir .`.

**中文**

完成配置后，用 `claude --plugin-dir .` 加载此目录。
