# chi-paper + essay-exam：Claude Code 版

[双语说明](README.md)

## 用途

本公开仓库提供介绍。私有核心仓库提供可使用的技能。

## 能力

人机交互论文。聚焦研究目标、证据、论证、稿件修订和会议规则。 学术文本检查。使用校准资源和可定位的检查结果。

## 双仓

[公开介绍](https://github.com/Shawn200212/ClaudeEssaySkill) · [私有核心](https://github.com/Shawn200212/ClaudeEssaySkill-core)

## 安装

使用有权限的账户拉取私有核心仓库。先预览安装，再应用更改。

```text
git clone https://github.com/Shawn200212/ClaudeEssaySkill-core.git
cd ClaudeEssaySkill-core
python install.py
python install.py --apply
```

## 调用

安装后，在 Claude Code 中调用 `/chi-paper-essay-exam:chi-paper, /chi-paper-essay-exam:essay-exam`。检查安装输出中的未满足依赖。

## 依赖

包内含技能指令和必要的小型资源。外部应用、项目文件和论文库需要另行准备。

## 核查

本次发布记录结构检查、相关本地测试和有限写作试验。这些检查不能证明全部流程或模型均已通过。

## 边界

本技能不保证录用、作者身份判断或通用写作质量。实际应用操作需要满足依赖并另行验证。

## 插件

完成配置后，用 `claude --plugin-dir .` 加载此目录。
