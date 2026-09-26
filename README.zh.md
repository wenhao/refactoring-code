# refactoring-code

[English](README.md) | [中文](README.zh.md)

一个面向 AI 编程代理的代码重构**技能（skill）**，由 Martin Fowler《重构：改善既有代码的设计（第二版）》中文版**全书 12 章逐章精读提炼**而成。

技能把书中的方法论——小步安全前进、测试安全网、坏味道诊断、完整重构手法目录——编码为按需加载的指令：当你要求 AI 重构代码时，它才会加载并遵循这些指令。技能**自足**：安装后只需 `skill/` 目录，不依赖仓库里的其他内容。

## 仓库结构

```
refactoring-code/
├── skill/                      # 技能本体（自足，可独立安装）
│   ├── SKILL.md                # 入口：六步工作流 + 原则速查 + 索引
│   └── references/             # 按需加载（渐进式披露）
│       ├── principles.md       # 原则与心态（第 1、2 章）
│       ├── testing.md          # 测试安全网（第 4 章）
│       ├── smells.md           # 24 种坏味道 → 首选重构映射（第 3 章）
│       ├── catalog-basic.md            # 第一组重构（第 6 章，11 个手法）
│       ├── catalog-encapsulation.md    # 封装（第 7 章，9 个）
│       ├── catalog-moving-features.md  # 搬移特性（第 8 章，9 个）
│       ├── catalog-data.md             # 重新组织数据（第 9 章，5 个）
│       ├── catalog-conditionals.md     # 简化条件逻辑（第 10 章，6 个）
│       ├── catalog-api.md              # 重构 API（第 11 章，10 个）
│       └── catalog-inheritance.md      # 处理继承关系（第 12 章，11 个）
└── knowledgebase/
    └── book-refactoring2/      # 原书（中文版）——仅开发期校对参考，
                                # 安装后的技能不依赖它
```

共覆盖 **61 个手法**与 **24 种坏味道**。每条目保持原书五段式骨架（名称 → 速写 → 动机 → 做法 → 范例指引）。章号与页码是**供人工溯源的引用信息**，不是文件路径。

## 技能如何工作

`SKILL.md` 定义了七步工作流：

1. **圈定扫描范围（默认增量）**——用户未指定目标时，默认只扫描未提交与已提交未推送的改动（`git diff @{u}` + 未跟踪文件），向用户简报范围，并主动提醒可做全仓库扫描；用户明确指定目标或明确要求全仓库时以用户为准。
2. **明确意图、建立基线**——两顶帽子（重构绝不与加功能混在同一提交）；先建测试安全网（遗留代码先补特征测试）。
3. **诊断坏味道**——对照 `smells.md` 识别，此阶段只读不动。
4. **提出重构方案，等待用户决策**——提交发现清单（坏味道、位置、严重度）、建议手法（按影响排序）、风险评估（局部 vs 触碰接口），并请求用户确认范围、深度与"不动"约束；**没有用户批准不得改动任何一行代码**。
5. **按批准的方案执行**——加载对应的 `catalog-*.md` 按小步清单执行；技能自足，不依赖任何外部文件。
6. **小步执行循环**——一次微小改动 → 编译 → 测试 → 提交；失败时回滚到上一个绿色状态。执行中新发现不扩大战场，记录下来纳入下次决策。
7. **收尾汇报**——同步调用方与文档，汇报批准项的完成情况（含被砍掉的项）与行为不变的依据。

## 安装

技能采用通用的 `SKILL.md` 格式（目录 + 带 `name`/`description` YAML frontmatter 的 SKILL.md），任何支持技能机制的代理都能用。把 `skill/` 目录以 `refactoring-code` 之名符号链接（或复制）到你所用手具的技能目录：

| 工具 | 项目级 | 全局 |
|------|--------|------|
| ZCode | `<project>/.zcode/skills/` 或 `<project>/.agents/skills/` | `~/.zcode/skills/` 或 `~/.agents/skills/` |
| Claude Code | `<project>/.claude/skills/` | `~/.claude/skills/` |
| Codex | `<project>/.codex/skills/` | `~/.codex/skills/` |

```bash
# 示例：为 Claude Code 全局启用
ln -s /path/to/refactoring-code/skill ~/.claude/skills/refactoring-code

# 示例：为 Codex 在当前项目启用
ln -s /path/to/refactoring-code/skill .codex/skills/refactoring-code
```

把 `/path/to/refactoring-code` 换成你本地的克隆路径。若克隆目录可能挪动，全局安装建议复制而非符号链接。

## 使用方式

安装后，当你说"重构这段代码"、"改善这段代码的设计/可读性"、"清理一下这个烂代码/技术债"之类的话时技能会**自动触发**——即使你没有说出"重构"二字。支持显式调用的代理也可以直接调用（如 ZCode 中的 `/refactoring-code`）。

## 维护约定

本技能由逐章精读全书、每读一章迭代一次而成。后续改进时：

- 保持每条目"动机 + 做法"的结构与原书页码锚点。
- 提炼内容与原书冲突时，**以原书为准**——开发仓库的 `knowledgebase/` 有全书文本可供校对。
- **可移植性约束**：skill 内不得引用仓库文件（如 `knowledgebase/`）——用户安装时只带走 `skill/` 目录；引用一律只保留章号与页码。

## 版权说明

- `skill/` 是学习笔记性质的方法论提炼，供个人学习使用。
- `knowledgebase/book-refactoring2/` 来自 [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2)，为《重构：改善既有代码的设计（第二版）》中文版书稿，版权归原书作者及译者所有，仅为个人学习目的收录，请勿用于商业再分发。
