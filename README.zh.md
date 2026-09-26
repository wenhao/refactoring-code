# refactoring-code

[English](README.md) | [中文](README.zh.md)

一个面向 ZCode（AI 编程代理）的代码重构**技能（skill）**，由 Martin Fowler《重构：改善既有代码的设计（第二版）》中文版**全书 12 章逐章精读提炼**而成。

技能把书中的方法论——小步安全前进、测试安全网、坏味道诊断、完整重构手法目录——编码为按需加载的指令：当你要求 AI 重构代码时，它才会加载并遵循这些指令。

## 仓库结构

```
refactoring-code/
├── skill/                      # 技能本体
│   ├── SKILL.md                # 入口：五步工作流 + 原则速查 + 索引
│   └── references/             # 按需加载（渐进式披露）
│       ├── principles.md       # 原则与心态（第 1、2、5 章）
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
    └── book-refactoring2/      # 原书（中文版），研读参考
```

共覆盖 **61 个手法**与 **24 种坏味道**。每条目保持原书五段式骨架（名称 → 速写 → 动机 → 做法 → 范例指引），并附**页码锚点**，可回查 `knowledgebase/` 中对应章节的完整示例。

## 技能如何工作

`SKILL.md` 定义了五步工作流：

1. **明确意图、建立基线**——两顶帽子（重构绝不与加功能混在同一提交）；先建测试安全网（遗留代码先补特征测试）。
2. **诊断坏味道**——对照 `smells.md` 识别，一次只攻击最影响理解的那一个。
3. **查阅手法目录**——加载对应的 `catalog-*.md` 按小步清单执行；初次使用某手法时按锚点回读原书范例。
4. **小步执行循环**——一次微小改动 → 编译 → 测试 → 提交；失败时回滚到上一个绿色状态，而不是边修边继续。
5. **收尾汇报**——同步调用方与文档，汇报修了哪些坏味道、用了哪些手法、行为不变的依据。

## 安装启用

ZCode 从以下目录发现技能（优先级从高到低）：

- `<project>/.zcode/skills/<name>/`
- `<project>/.agents/skills/<name>/`
- `~/.zcode/skills/<name>/`
- `~/.agents/skills/<name>/`

把 `skill/` 以 `refactoring-code` 之名符号链接（或复制）到上述任一目录即可：

```bash
# 全局启用（对所有项目生效）
ln -s /path/to/refactoring-code/skill ~/.agents/skills/refactoring-code

# 或仅当前项目
ln -s /path/to/refactoring-code/skill .agents/skills/refactoring-code
```

## 使用方式

安装后，当你说"重构这段代码"、"改善这段代码的设计/可读性"、"清理一下这个烂代码/技术债"之类的话时技能会**自动触发**——即使你没有说出"重构"二字。也可以用 `/refactoring-code` 显式调用。

## 维护约定

本技能由逐章精读全书、每读一章迭代一次而成。后续改进时：

- 保持每条目"动机 + 做法"的结构与原书页码锚点。
- 提炼内容与原书冲突时，**以原书为准**——锚点让你随时能回查验证。
- 知识库让技能自洽：目录条目中的章节引用都解析到 `knowledgebase/book-refactoring2/docs/ch*.md`。

## 版权说明

- `skill/` 是学习笔记性质的方法论提炼，供个人学习使用。
- `knowledgebase/book-refactoring2/` 来自 [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2)，为《重构：改善既有代码的设计（第二版）》中文版书稿，版权归原书作者及译者所有，仅为个人学习目的收录，请勿用于商业再分发。
