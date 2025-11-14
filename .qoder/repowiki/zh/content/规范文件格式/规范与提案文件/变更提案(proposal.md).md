# 变更提案(proposal.md)

<cite>
**本文档中引用的文件**  
- [proposal.md](file://openspec/changes/add-scaffold-command/proposal.md)
- [proposal.md](file://openspec/changes/archive/2025-01-11-add-update-command/proposal.md)
- [tasks.md](file://openspec/changes/add-scaffold-command/tasks.md)
- [tasks.md](file://openspec/changes/archive/2025-01-11-add-update-command/tasks.md)
- [spec.md](file://openspec/specs/openspec-conventions/spec.md)
- [spec.md](file://openspec/changes/archive/2025-08-19-adopt-delta-based-changes/proposal.md)
- [agents-template.ts](file://src/core/templates/agents-template.ts)
- [validator.ts](file://src/core/validation/validator.ts)
- [change.ts](file://src/commands/change.ts)
</cite>

## 目录
1. [引言](#引言)
2. [提案文件的核心结构](#提案文件的核心结构)
3. [From/To/Reason/Impact 显式变更描述](#fromtoreasonimpact-显式变更描述)
4. [变更生命周期与协同机制](#变更生命周期与协同机制)
5. [完整的 proposal.md 示例](#完整的-proposalmd-示例)
6. [常见错误及规避方法](#常见错误及规避方法)
7. [结论](#结论)

## 引言

在 OpenSpec 框架中，`proposal.md` 是变更提案的核心文档，用于明确描述系统变更的意图、范围和影响。该文件不仅作为人类评审者理解变更的依据，也是 AI 代理生成和验证变更的基础。通过结构化格式，`proposal.md` 替代了传统的内联 diff，确保变更意图清晰可读，并与 `tasks.md` 协同指导实施过程。

## 提案文件的核心结构

根据 `openspec-conventions/spec.md` 的要求，`proposal.md` 必须包含以下核心部分：

- **Why（为什么）**：简明扼要地说明变更的动机或问题背景。
- **What Changes（变更内容）**：列出变更的主要条目，通常以项目符号形式呈现。
- **Impact（影响）**：描述变更对现有系统、规范或代码的影响范围。

这些部分共同构成了变更提案的基本骨架，确保评审者能够快速理解变更的上下文。

**Section sources**
- [spec.md](file://openspec/specs/openspec-conventions/spec.md#L166-L180)
- [proposal.md](file://openspec/changes/add-scaffold-command/proposal.md#L1-L12)

## From/To/Reason/Impact 显式变更描述

### 显式变更结构的重要性

OpenSpec 要求在 `What Changes` 部分使用 **From/To/Reason/Impact** 的显式结构来描述每个变更条目。这种格式替代了内联 diff，原因如下：

- **可读性增强**：避免了 diff 语法（如 `+` 和 `-`）对文档的视觉干扰，使评审者能专注于变更内容本身。
- **意图明确**：通过 `From` 和 `To` 的对比，清晰展示变更前后的状态差异。
- **上下文完整**：`Reason` 和 `Impact` 提供了变更的业务和技术背景，帮助评审者判断变更的必要性和风险。

### 结构规范

每个变更条目必须使用加粗标题标明变更范围，格式如下：

```markdown
**[变更范围]**
- From: [当前状态]
- To: [未来状态]
- Reason: [变更原因]
- Impact: [影响评估]
```

此结构确保了变更描述的一致性和可解析性，便于工具自动化处理。

**Section sources**
- [spec.md](file://openspec/specs/openspec-conventions/spec.md#L166-L180)
- [proposal.md](file://openspec/changes/archive/2025-08-19-adopt-delta-based-changes/proposal.md#L9-L93)

## 变更生命周期与协同机制

### 变更生命周期

根据 `openspec-conventions/spec.md`，变更提案在系统中遵循以下生命周期：

1. **提出（Propose）**：AI 生成 `proposal.md` 和 `tasks.md`，定义变更目标。
2. **评审（Review）**：人工评审提案内容，确认变更合理性。
3. **批准（Approve）**：变更被批准后进入实施阶段。
4. **实施（Implement）**：根据 `tasks.md` 中的检查清单完成开发。
5. **归档（Archive）**：变更完成后，移动至 `archive/` 目录。

### 与 tasks.md 的协同

`proposal.md` 与 `tasks.md` 紧密协作：
- `proposal.md` 定义“**做什么**”和“**为什么做**”。
- `tasks.md` 列出“**如何做**”的具体实施步骤。

这种分离确保了高阶设计与低阶实现的解耦，提高了开发效率和可维护性。

**Section sources**
- [spec.md](file://openspec/specs/openspec-conventions/spec.md#L410-L421)
- [tasks.md](file://openspec/changes/add-scaffold-command/tasks.md#L1-L12)

## 完整的 proposal.md 示例

以下是一个符合 OpenSpec 规范的 `proposal.md` 示例：

```markdown
# Add Scaffold Command

## Why

Manual setup for new changes leads to formatting mistakes in spec deltas and slows agents who must recreate the same file skeletons for every proposal. A built-in scaffold command will generate compliant templates so assistants can focus on the change content instead of structure.

## What Changes

**CLI Command: openspec scaffold <change-id>**
- From: No scaffold command available
- To: Add `openspec scaffold <change-id>` CLI command
- Reason: Reduce manual errors and accelerate proposal creation
- Impact: New CLI command affects `src/cli/index.ts` and `src/commands`

**Documentation Update**
- From: No scaffold usage guidance
- To: Add scaffold instructions to `AGENTS.md` and CLI docs
- Reason: Ensure agents discover the workflow
- Impact: Updates to documentation in `docs/` and `AGENTS.md`

## Impact

- Affected specs: `specs/cli-scaffold`
- Affected code: `src/cli/index.ts`, `src/commands`, `docs/`
```

**Section sources**
- [proposal.md](file://openspec/changes/add-scaffold-command/proposal.md#L1-L12)
- [agents-template.ts](file://src/core/templates/agents-template.ts#L123-L166)

## 常见错误及规避方法

### 常见错误

1. **缺少 From/To 对比**：仅描述未来状态，未说明当前状态。
2. **Reason 不充分**：未提供足够的业务或技术背景支持变更。
3. **Impact 未明确**：未列出受影响的规范或代码文件。
4. **标题未加粗**：变更范围未使用 `**` 标记，导致结构不清晰。

### 规避方法

- 使用 `openspec scaffold` 命令生成模板，确保结构正确。
- 在提交前运行 `openspec validate` 检查格式合规性。
- 参考 `AGENTS.md` 中的快速参考指南，确保符合最新约定。

**Section sources**
- [validator.ts](file://src/core/validation/validator.ts#L153-L204)
- [change.ts](file://src/commands/change.ts#L32-L66)

## 结论

`proposal.md` 是 OpenSpec 变更管理流程的核心文档。通过强制使用 **From/To/Reason/Impact** 的显式结构，它确保了变更意图的清晰传达，替代了难以阅读的内联 diff。结合 `tasks.md` 的实施指导，该体系实现了从提案到归档的完整生命周期管理。遵循这些规范，团队可以更高效、更可靠地推进系统演进。