<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg" />
    <img src="assets/logo.svg" alt="refactoring-code logo" width="160" />
  </picture>
</div>

# refactoring-code

以小步、安全、有测试保护的方式重构代码。一个从《重构：改善既有代码的设计（第二版）》逐章提炼而来的技能（skill）。

[English](README.md) | [中文](README.zh.md)

## 概览

- **坏味道诊断**：24 种命名坏味道（过长函数、重复代码、依恋情结……）一一映射到首选重构手法，让代理知道**找什么**、以及找到后**用哪个手法**。
- **61 个手法目录**：原书名录中的每个手法——提炼函数、封装变量、以多态取代条件表达式、以委托取代子类……——都编码为小步"做法"清单，代理一次只做一个微小改动。
- **人来决策**：代理在改动任何一行之前，必须先提交方案（发现清单 → 建议手法 → 风险评估），并获得你对范围与深度的批准。
- **默认增量扫描**：只扫你未提交与已提交未推送的改动，并主动提醒可做全仓库扫描。

> 技能指令为中文，提炼自该书中文版。

## 特性

- **有据可查的手法目录**——逐章精读全书 12 章提炼而成；每条目保持原书五段式骨架（名称 → 速写 → 动机 → 做法），附页码引用供人工溯源。
- **自足安装**——`skill/` 目录就是代理所需的全部；不引用仓库其他路径，无外部依赖。
- **通用 SKILL.md 格式**——兼容 ZCode、Claude Code、Codex（以及任何从 `SKILL.md` 发现技能的代理）。
- **安全网优先**——动遗留代码前先补特征测试；全程红绿纪律（测试失败 → 回滚，不边修边前进）。
- **两顶帽子强制**——重构提交绝不混入新功能。
- **决策关口**——诊断只读不动，方案获批后才动手；执行中的新发现记入下一轮决策，绝不顺手扩大战场。
- **渐进式披露**——精炼的 `SKILL.md` 工作流入口；重量级手法目录只在诊断到对应坏味道时才加载。

## 快速开始

### 1. 安装

把 `skill/` 目录以 `refactoring-code` 之名符号链接（或复制）到所用工具的技能目录：

| 工具 | 项目级 | 全局 |
|------|--------|------|
| ZCode | `<project>/.zcode/skills/` 或 `<project>/.agents/skills/` | `~/.zcode/skills/` 或 `~/.agents/skills/` |
| Claude Code | `<project>/.claude/skills/` | `~/.claude/skills/` |
| Codex | `<project>/.codex/skills/` | `~/.codex/skills/` |

```bash
# 为 Claude Code 全局安装
ln -s /path/to/refactoring-code/skill ~/.claude/skills/refactoring-code

# 为 Codex 项目级安装
ln -s /path/to/refactoring-code/skill .codex/skills/refactoring-code
```

把 `/path/to/refactoring-code` 换成你本地的克隆路径。若克隆目录可能挪动，全局安装建议复制而非符号链接。

### 2. 触发

用自己的话直接说——即使不说"重构"二字也会触发：

```text
帮我重构一下这次改动的代码
Improve the design of the code I'm working on.
这段代码太乱了，帮我清理一下技术债
```

支持显式调用的代理也可以直接调用（如 ZCode 中的 `/refactoring-code`）。

### 3. 审阅方案，然后批准

代理会扫描你的增量改动，报告发现的坏味道，并给出带风险评估的手法建议。你决定范围（只做第一项 / 全部低风险项 / 全部）、深度（局部整理 vs 结构调整）以及"不动"的约束。批准之后它才动手——一次微小改动、一次测试、一次提交。

## 工作原理

```text
1. 圈定范围    git diff @{u} + 未跟踪文件 → 简报范围 → 提醒可全仓库扫描
2. 建立基线    意图（两顶帽子）+ 测试安全网（无测试先补特征测试）
3. 诊断        对照 24 种坏味道识别——只读不动
4. 决策        提交方案（发现/手法/风险）→ 等待用户批准
5. 执行        只做批准项，按目录小步清单执行
6. 循环        微小改动 → 编译 → 测试 → 提交；见红即回滚
7. 收尾        逐项汇报，含被砍掉的项与执行中的新发现
```

## 项目结构

```text
refactoring-code/
├── skill/                      # 技能本体（自足；安装时只需此目录）
│   ├── SKILL.md                # 入口：七步工作流 + 原则速查 + 手法目录索引
│   └── references/
│       ├── principles.md       # 原则与心态（第 1-2 章）：两顶帽子、何时重构、YAGNI
│       ├── testing.md          # 测试安全网（第 4 章）：特征测试、红绿纪律
│       ├── smells.md           # 24 种坏味道 → 首选重构映射（第 3 章）
│       ├── catalog-basic.md            # 提炼/内联、改名、拆分阶段……（第 6 章）
│       ├── catalog-encapsulation.md    # 封装记录/集合、提炼类……（第 7 章）
│       ├── catalog-moving-features.md  # 搬移函数/字段、拆分循环、移除死代码……（第 8 章）
│       ├── catalog-data.md             # 拆分变量、派生变量→查询……（第 9 章）
│       ├── catalog-conditionals.md     # 卫语句、多态取代条件、引入特例……（第 10 章）
│       ├── catalog-api.md              # 查询/修改分离、移除标记参数……（第 11 章）
│       └── catalog-inheritance.md      # 上移/下移、类型码→子类、委托……（第 12 章）
├── knowledgebase/
│   └── book-refactoring2/      # 原书（中文版）——仅开发期校对参考，
│                               # 安装后的技能不依赖它
├── assets/                     # Logo（浅色/深色双主题变体）
├── README.md                   # 本文件（英文）
└── README.zh.md                # 中文版
```

## 维护约定

本技能由逐章精读全书、每读一章迭代一次而成。后续改进时：

- 保持每条目"动机 + 做法"的结构与原书页码锚点。
- 提炼内容与原书冲突时，**以原书为准**——`knowledgebase/` 有全书文本可供校对。
- **可移植性约束**：skill 内不得引用仓库文件（如 `knowledgebase/`）——用户安装时只带走 `skill/` 目录；引用一律只保留章号与页码。

## 版权说明

- `skill/` 是学习笔记性质的方法论提炼，供个人学习使用。
- `knowledgebase/book-refactoring2/` 来自 [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2)，为《重构：改善既有代码的设计（第二版）》中文版书稿，版权归原书作者及译者所有，仅为个人学习目的收录，请勿用于商业再分发。
