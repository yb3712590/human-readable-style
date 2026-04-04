---
name: human-readable-style
description: Always use this skill for ANY Chinese-language communication regarding software development, coding, code explanations, or project status. It ensures natural wording and prevents awkward translations.
---

# English Translation (for environments where Chinese characters render as garbled text)

## Overview

When AI coding assistants report development progress in Chinese, three common problems arise: invented jargon (made-up metaphors instead of plain terms), European-style long sentences (subject-clause nesting), and unnecessary mixing of English verbs into Chinese sentences. These make reports hard to read for both managers and developers.

**Core principle:** Write Chinese the way a normal person speaks. Technical nouns in English are fine, but sentence structure, verbs, and connectors must be natural Chinese.

This skill addresses specific issues in Chinese development communication. It does NOT take over all Chinese output. General Chinese expression preferences should go in the global `AGENTS.md`. This skill triggers only in these scenarios:
- Development progress reports
- Code change explanations
- Comparing options and asking for decisions
- Chinese project communication aimed at managers or collaborators

## Applicable Scenarios

- Progress reports (for project managers, tech leads)
- Code change descriptions (PR descriptions, commit summaries)
- Asking users to choose between options
- Chinese project communication where collaborators need to quickly grasp key points

## Not Applicable

- Default style control for all Chinese conversations
- Casual chat, greetings, pure Q&A Chinese replies
- English communication for English-only projects
- Code comments (follow the project's language)
- Internal technical docs if the team convention is English

## Quick Check

Use this skill if the current output meets any of these conditions:
- Need to report "what was done, where things stand, what's next"
- Need to explain a round of code changes: content, reasons, and scope of impact
- Need to present multiple options with their applicable scenarios, benefits, and costs
- Need to translate technical details into Chinese that collaborators can quickly understand

If it's just a normal Chinese answer without reporting, explaining, or comparing options, this skill is not needed.

## Prohibited Patterns

### 1. No invented metaphors replacing plain language

| Prohibited | Should write | Why |
|-----------|-------------|-----|
| Made-up metaphorical terms | Plain, direct descriptions | Invented terms are not standard Chinese dev terminology |
| Stiff literal translations of English concepts | Natural Chinese equivalents or keep the English term | Forced translations are harder to understand than the original English |
| Slang/jargon ("write to DB" as slang) | "Write to database" or "Save to database" | Slang is inappropriate for written reports |

### 2. No European-style Chinese

Characteristics: overly long subjects, excessive passive voice, three or more consecutive "de" (的) modifiers.

**Rule:** If a sentence exceeds ~40 characters, break it up. If it doesn't sound natural when read aloud, rewrite it.

### 3. No pointless Chinese-English mixing

Technical nouns in English are normal (SQLite, Repository, bundle, manifest), but verbs and connectors must be in Chinese.

**Allowed English:**
- Function names, class names, package names: `build_compile_request`, `ExecutionPackage`
- Widely-used English technical terms: `bundle`, `manifest`, `provenance`, `SQLite`
- Terms where the English is clearer than any Chinese translation: `Repository` (clearer than forced Chinese translations)

## Reporting Structure

### Progress Report Template

Each report should let a manager know three things within 10 seconds: **what was done, current status, what's next.**

```markdown
## Progress

**Completed:**
- Defined internal interfaces for compilation artifacts (ExecutionBundle, CompileManifest)
- Added two new tables in SQLite database for storing compilation records
- Compiler module now follows "compile and store first, then execute" workflow

**Current status:** Core workflow tests passed, running regression tests

**Next step:** Update README and design docs to document this change
```

### When Asking for Decisions, Be Specific

When the user needs to make a choice, don't just list technical options — explain what real-world scenario each option fits, its benefits, and its costs.

```
Bad (vague):
"Option A: Use SQLite WAL mode
 Option B: Use normal mode
 Please choose."

Good (specific):
"Two choices for database write mode:

Option A: Enable WAL mode
→ Fits: Multiple tasks reading/writing simultaneously (e.g., compiling and querying at the same time)
→ Benefit: Reads and writes don't block each other, good concurrency
→ Cost: Extra WAL file on disk, ~a few MB

Option B: Keep default mode
→ Fits: Tasks are mostly sequential, only one writing at a time
→ Benefit: Simple, no extra configuration
→ Cost: Writes block reads

Our current usage is [specific description], I recommend Option X. What do you think?"
```

### Code Change Descriptions

Explain what files changed, why, and whether anything else is affected.

```markdown
## Changes

**What changed:**
- `compiler.py`: Added bundle packaging and manifest generation to the compilation flow
- `repository.py`: Added read/write methods for two new tables (compile_bundles, compile_manifests)
- `runtime.py`: Now reads compilation artifacts from database before execution, no longer calls compiler directly

**Why:**
Previously, compilation results were temporary and discarded after execution. Now we persist them for post-hoc auditing and troubleshooting.

**Impact scope:**
- External interfaces unchanged — existing commands and APIs are not affected
- Database will auto-create new tables, no manual migration needed
```

## Self-Check Checklist

After writing Chinese content, check each item:

1. **Does it sound natural when read aloud?** — If you stumble, rewrite it
2. **Are there three consecutive "de" (的)?** — If yes, split the sentence
3. **Are verbs in Chinese?** — Function names can be English, but action words like "do/change/add/delete" must be Chinese
4. **Can a manager understand it?** — Can a non-coder grasp the key point within 10 seconds?
5. **Any self-invented terms?** — Check if anyone else actually uses that word
6. **Are options clearly explained?** — Each option should state its real-world scenario, benefits, and costs

## Common Mistakes Reference

| Original | Problem | Fixed |
|----------|---------|-------|
| Invented metaphorical jargon | Made-up terms no one understands | Plain, direct language |
| Stiff literal translations + invented words | Forced translations | Define interfaces clearly, specify storage approach |
| Slang terms for database operations | Slang inappropriate for reports | "Compile and save to database, then execute" |
| "Command protocol" | Unnatural phrasing | "Command format" |
| Piling up jargon in one phrase | Term overload | Break into clear items: packaging artifacts, generating manifests, recording provenance |
| Completely opaque jargon | Incomprehensible | "Started writing the database layer code" |
| Overly technical follow-up | Needs plain language | "Testing focuses on three things: 1. Artifacts are actually generated 2. Database reads/writes work 3. Records aren't lost on execution failure" |
| Casual/metaphorical language in reports | Informal tone | "Confirm API and approval workflow are not affected by this change" |
| Overly literary phrasing | Too poetic for technical writing | "Append this change record to memory.txt" |

---

# 面向管理者的中文开发沟通规范

## 概述

AI 编码助手在用中文汇报开发进展时，常见三类问题：生造术语（"落点""契约""切片"）、欧式长句（主语从句套从句）、土洋夹杂（中文句子里随意插英文动词短语）。这些写法让管理者读不懂，开发者也觉得别扭。

**核心原则：** 用正常人说话的方式写中文。技术名词保留原文没问题，但句子结构、动词、连接词必须是地道中文。

这个 skill 解决的是中文开发沟通里的专项问题，不负责接管所有中文输出。通用的中文表达偏好应该放在全局 `AGENTS.md`，这个 skill 只在下面这些场景触发：
- 开发进度汇报
- 代码改动说明
- 方案比较与征求决定
- 面向管理者或协作者的中文项目推进沟通

## 适用场景

- 开发进度汇报（给项目经理、技术负责人看的）
- 代码变更说明（PR 描述、commit 总结）
- 方案选择时向用户征求意见
- 需要让协作者快速看懂重点的中文项目推进沟通

## 不适用

- 所有中文对话的默认文风控制
- 闲聊、寒暄、纯问答式中文回复
- 纯英文项目的英文沟通
- 代码注释（跟着项目语言走）
- 内部技术文档如果团队约定用英文

## 快速判断

如果当前输出满足下面任一条件，就应该使用这个 skill：

- 需要向人汇报“做了什么、现在到哪了、下一步是什么”
- 需要解释一轮代码改动的内容、原因和影响范围
- 需要给出多个方案，并说明各自适合什么场景、好处和代价
- 需要把技术细节翻成协作者能快速看懂的中文

如果只是普通中文回答，没有汇报、说明或方案比较任务，就不需要用这个 skill。

## 禁止清单

### 1. 禁止生造隐喻代替直白说法

| 禁止写法 | 应该写 | 为什么 |
|----------|--------|--------|
| "落地这条切片" | "开始做这个功能" | "切片"不是中文开发术语 |
| "补内部契约" | "定义内部接口" 或 "写接口约定" | "契约"是 Design by Contract 的生硬直译 |
| "仓储落点" | "数据库存储" 或 "Repository 层" | "仓储落点"没人看得懂 |
| "落库" | "写入数据库" 或 "存到数据库" | "落库"是口语黑话，书面不通 |
| "接落点" | "对接存储层" 或 "接上数据库" | 同上 |
| "带歪" | "影响到" 或 "破坏" | 口语化，不适合书面汇报 |
| "散着改" | "到处改" 或 "改动分散" | 同上 |
| "跑通" | "测试通过" 或 "运行正常" | 口语，书面用"通过"或"正常" |
| "说得太虚" | "描述不具体" | 口语 |

### 2. 禁止欧式中文

欧式中文的特征：主语拖很长、被动句泛滥、"的"字连用三层以上。

```
? 欧式：
"编译器文件会是这轮改动最多的地方，但我还是沿着现有结构做：
build_compile_request 保留，compile_execution_package 也保留兼容，
只是在中间插入 bundle、manifest 和落库。"

? 正常中文：
"这次改动主要集中在编译器模块。我会保持现有的 build_compile_request
和 compile_execution_package 不变，在中间加入 bundle 打包、生成
manifest、写入数据库这三步。"
```

**判断标准：** 一句话超过 40 个字就该断句。读出来不顺口就该改。

### 3. 禁止无意义的土洋结合

技术名词用英文是正常的（`SQLite`、`Repository`、`bundle`、`manifest`），但动词和连接词必须用中文。

```
? "我现在 compile 一下然后 persist 到 DB"
? "我现在编译一下，然后存到数据库"

? "这个 change 会 affect 到 existing 的 API"
? "这个改动会影响现有的 API"
```

**允许保留英文的情况：**
- 函数名、类名、包名：`build_compile_request`、`ExecutionPackage`
- 技术概念的通用英文名：`bundle`、`manifest`、`provenance`、`SQLite`
- 没有对应中文译名或译名反而更难懂的：`Repository`（比"仓储"清楚）

## 汇报结构规范

### 进度汇报模板

每次汇报应该让管理者在 10 秒内知道三件事：**做了什么、现在到哪了、下一步是什么。**

```markdown
## 进度

**已完成：**
- 定义了编译产物的内部接口（ExecutionBundle、CompileManifest）
- 在 SQLite 数据库中新增了两张表，用于存储编译记录
- 编译器模块已改为"先编译存库，再执行"的流程

**当前状态：** 核心流程测试通过，正在跑回归测试

**下一步：** 更新 README 和设计文档，记录本次改动
```

### 征求决定时必须写清楚

当需要用户做选择时，不能只列技术选项，必须说清楚每个选项对应什么业务场景。

```
? 模糊的选项：
"方案 A：用 SQLite WAL 模式
 方案 B：用普通模式
 请选择。"

? 写清楚的选项：
"数据库写入方式有两个选择：

方案 A：开启 WAL 模式
→ 适合场景：多个任务同时读写数据库，比如编译和查询同时进行
→ 好处：读写互不阻塞，并发性能好
→ 代价：磁盘多占一个 WAL 文件，约几 MB

方案 B：保持默认模式
→ 适合场景：任务基本是串行的，同一时间只有一个在写
→ 好处：简单，不用额外配置
→ 代价：写入时会阻塞读取

目前我们的使用场景是 [具体描述]，我建议选方案 X。你觉得呢？"
```

### 代码变更说明

说清楚改了什么文件、为什么改、有没有影响到别的地方。

```markdown
## 本次改动

**改了什么：**
- `compiler.py`：在编译流程中加入了 bundle 打包和 manifest 生成
- `repository.py`：新增两张表的读写方法（compile_bundles、compile_manifests）
- `runtime.py`：执行前先从数据库读取编译产物，不再直接调编译器

**为什么改：**
之前编译结果是临时的，执行完就丢了。现在把编译产物存下来，
方便事后审计和问题排查。

**影响范围：**
- 对外接口没有变化，现有的命令和 API 都不受影响
- 数据库会自动建新表，不需要手动迁移
```

## 句式自检清单

写完中文内容后，逐条检查：

1. **读出来顺口吗？** — 念一遍，卡壳的地方就该改
2. **有没有连续三个"的"？** — 有就拆句
3. **动词是中文吗？** — 函数名可以英文，但"做""改""加""删"这些必须中文
4. **管理者能看懂吗？** — 不写代码的人能不能在 10 秒内抓到重点
5. **有没有自造词？** — 查一下这个词是不是只有你在用
6. **选项写清楚了吗？** — 每个选项对应什么现实场景，好处和代价是什么

## 常见错误对照

| 原文 | 问题 | 改后 |
|------|------|------|
| "我开始落地这条切片" | 生造隐喻 | "我开始做这个功能" |
| "先补内部契约和仓储落点" | 生硬直译 + 自造词 | "先定义内部接口，确定数据库存储方案" |
| "再把运行时改成'先编译落库、再执行'" | "落库"是黑话 | "把运行流程改成'先编译并存入数据库，再执行'" |
| "不加新接口、不改现有命令协议" | "命令协议"不自然 | "不加新接口，现有的命令格式也不动" |
| "只在内部补'编译审计产物 + 落库 + 最小 provenance'" | 堆砌术语 | "只在内部加上编译记录的保存功能，包括打包产物、生成清单、记录来源信息（provenance）" |
| "数据库层开始接落点了" | 完全看不懂 | "开始写数据库层的代码了" |
| "这里我会优先验证三件事" | 还行，但后面太技术 | "测试重点是三件事：1. 编译产物确实生成了 2. 数据库能正常存取 3. 执行失败时记录不会丢" |
| "确认 API 和审批链没被这次内部改造带歪" | 口语 + 隐喻 | "确认 API 和审批流程没有被这次改动影响" |
| "memory.txt 追加连续性记录，不重写历史" | 过度文学化 | "在 memory.txt 里追加这次的改动记录" |