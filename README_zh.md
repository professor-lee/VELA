<div align="center">
  <img src="./assets/vela-logo.svg" alt="VELA logo" width="160" />

# VELA

**VELA Enables Lazy Architects**  
一个面向 AI coding agent 的文档驱动开发 harness 规范仓库。

<div align="center">
  <a href="https://github.com/professor-lee/VELA">
    <img src="https://img.shields.io/badge/repository-professor--lee%2FVELA-181717?logo=github&logoColor=white" alt="Repository" />
  </a>
  <a href="https://github.com/professor-lee/VELA/stargazers">
    <img src="https://img.shields.io/github/stars/professor-lee/VELA?style=flat&logo=github&logoColor=white" alt="Stars" />
  </a>
  <a href="https://github.com/professor-lee/VELA/forks">
    <img src="https://img.shields.io/github/forks/professor-lee/VELA?style=flat&logo=github&logoColor=white" alt="Forks" />
  </a>
  <a href="https://github.com/professor-lee/VELA/issues">
    <img src="https://img.shields.io/github/issues/professor-lee/VELA?logo=github&logoColor=white" alt="Issues" />
  </a>
  <a href="https://github.com/professor-lee/VELA/pulls">
    <img src="https://img.shields.io/github/issues-pr/professor-lee/VELA?logo=github&logoColor=white" alt="Pull Requests" />
  </a>
  <a href="https://github.com/professor-lee/VELA/commits">
    <img src="https://img.shields.io/github/last-commit/professor-lee/VELA?logo=git&logoColor=white" alt="Last Commit" />
  </a>
  <a href="https://github.com/professor-lee/VELA">
    <img src="https://img.shields.io/github/repo-size/professor-lee/VELA?logo=github&logoColor=white" alt="Repo Size" />
  </a>
  <a href="https://github.com/professor-lee/VELA/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/professor-lee/VELA?logo=opensourceinitiative&logoColor=white" alt="License" />
  </a>
  <img src="https://img.shields.io/badge/harness-v0.1.2-2563EB?logo=gitbook&logoColor=white" alt="Harness Version" />
  <img src="https://img.shields.io/badge/runtime-.vela%2F-7C3AED?logo=gnubash&logoColor=white" alt="Runtime Directory" />
  <img src="https://img.shields.io/badge/code%20root-project%2F-0F766E?logo=files&logoColor=white" alt="Code Root" />
  <img src="https://img.shields.io/badge/Codex-AGENTS.md-10A37F?logo=openai&logoColor=white" alt="Codex Adapter" />
  <img src="https://img.shields.io/badge/Claude%20Code-CLAUDE.md-D97757?logo=anthropic&logoColor=white" alt="Claude Code Adapter" />
  <img src="https://img.shields.io/badge/docs-bilingual-0EA5E9?logo=markdown&logoColor=white" alt="Bilingual README" />
</div>

**语言：[English](./README.md) · 简体中文**

</div>

---

## 目录

- [项目定位](#项目定位)
- [这个仓库包含什么](#这个仓库包含什么)
- [这个仓库不包含什么](#这个仓库不包含什么)
- [核心理念](#核心理念)
- [快速开始](#快速开始)
- [生成后的 Harness 会产出什么](#生成后的-harness-会产出什么)
- [如何使用生成后的 Harness](#如何使用生成后的-harness)
- [运行阶段](#运行阶段)
- [关键约束](#关键约束)
- [规则优先级](#规则优先级)
- [FAQ](#faq)

---

## 项目定位

VELA 是一个 **harness builder 规范项目**。

它的目标不是直接提供一个已经构建完成的 harness 包，而是提供一份严格的 `BUILDER` 规范，让 agent 能够稳定、可复现地生成完整 VELA Harness 运行体。

换句话说：

- **这个仓库主要承载的是“如何生成 VELA Harness”的规则。**
- **真正的 harness 运行体由 agent 根据 BUILDER 生成。**
- **README 负责向人说明 VELA 的设计目的、使用方法和产物结构。**

---

## 这个仓库包含什么

当前仓库结构如下：

```text
README.md
README_zh.md
VELA_HARNESS_BUILDER.md
assets/
└── vela-logo.svg
```

其中：

- `VELA_HARNESS_BUILDER.md`：核心构建规范文件，定义如何生成 `AGENTS.md`、`CLAUDE.md` 与 `.vela/`。
- `README.md`：英文说明文档。
- `README_zh.md`：简体中文说明文档。
- `assets/vela-logo.svg`：项目 logo。

---

## 这个仓库不包含什么

本仓库 **不会直接上传已经构建好的 harness 包**。

也就是说，仓库默认不包含：

```text
AGENTS.md
CLAUDE.md
.vela/
```

原因很简单：

1. **单一事实来源**：以 `BUILDER` 为唯一生成依据，避免维护两套内容。
2. **可复现**：任何人都可以基于同一份 BUILDER 重新生成完整 harness。
3. **避免漂移**：已构建产物容易与规范正文发生偏移，而 BUILDER 更适合作为权威源。
4. **更适合迭代**：修改规则时，只需更新 BUILDER 与 README，不必手动维护一整份运行体包。

---

## 核心理念

VELA 的核心设计思想可以概括为 5 条：

1. **先文档，后代码。**
2. **先目标，后边界；先边界，后架构；先架构，后开发。**
3. **北极星文档 v0.0.1 是唯一合法入口。**
4. **边界不清晰时必须走选择题协议，而不是靠 agent 自行脑补。**
5. **进入开发阶段后，软件代码必须统一收敛到 `project/`。**

这让 VELA 更像一个“开发流程约束层”，而不是一个普通模板仓库。

---

## 快速开始

根据 `VELA_HARNESS_BUILDER.md` 构建 harness。

### 1. 获取本仓库内容

克隆或下载本仓库，获取以下内容：

```text
README.md
README_zh.md
VELA_HARNESS_BUILDER.md
assets/vela-logo.svg
```

### 2. 把 BUILDER 提供给 agent

你需要把 `VELA_HARNESS_BUILDER.md` 提供给支持文件输入的 AI coding agent，并明确要求它：

- 按 BUILDER 规范生成完整 VELA Harness。
- 仅生成规范中定义的文件。
- 不得跳过自检。
- 不得在 Builder 阶段继续进入项目开发。

### 3. 由 agent 生成 Harness 运行体

根据 BUILDER，agent 应生成：

```text
AGENTS.md
CLAUDE.md
.vela/
```

注意：**这是生成结果，不是本仓库默认直接提交的内容。**

### 4. 将生成结果放入目标 workspace

生成完成后，把以下内容放到你的目标项目 workspace 根目录：

```text
AGENTS.md
CLAUDE.md
.vela/
```

然后再让 Codex 或 Claude Code 在该 workspace 中运行。

---

## 生成后的 Harness 会产出什么

根据 BUILDER，agent 生成的 VELA Harness 运行体会包含：

```text
AGENTS.md
CLAUDE.md
.vela/
├── SKILL.md
├── README.md
├── harness.yaml
├── CHANGELOG.md
├── rules/
├── templates/
├── schemas/
├── checklists/
└── extensions/
```

其中关键职责如下：

- `AGENTS.md`：Codex 默认发现入口，负责转发到 `.vela/SKILL.md`。
- `CLAUDE.md`：Claude Code 默认发现入口，负责转发到 `.vela/SKILL.md`。
- `.vela/SKILL.md`：唯一核心运行入口。
- `.vela/harness.yaml`：结构与文件清单最高基准。
- `.vela/rules/`：阶段规则、协议规则、质量规则。
- `.vela/templates/`：文档模板。
- `.vela/schemas/`：结构约束，高于模板。
- `.vela/checklists/`：自检与阶段完成检查。

---

## 如何使用生成后的 Harness

### 1. 提供北极星文档 v0.0.1

VELA 运行时的唯一合法输入入口，是你提供的 **北极星文档 v0.0.1**。

最低建议结构：

```markdown
# 北极星文档 v0.0.1
## 1. 项目目标
## 2. 目标用户
## 3. 核心使用场景
## 4. MVP 边界
## 5. 非目标
## 6. 技术/平台约束
## 7. 核心功能方向
## 8. 待确认内容
## 9. 决策记录
## 10. 变更记录
```

### 2. 让 agent 读取入口文件

- 对 Codex：读取 workspace 根目录的 `AGENTS.md`
- 对 Claude Code：读取 workspace 根目录的 `CLAUDE.md`

它们都必须继续指向 `.vela/SKILL.md`，不得分叉为第二套规则。

### 3. 按阶段推进

VELA 不允许直接跳到编码，而是要求按固定阶段推进，直到可以合法生成 `TODO.md` 并进入开发执行。

---

## 运行阶段

VELA 使用固定阶段状态机：

| 阶段 | 含义 |
|---|---|
| S0 | 入口验收阶段 |
| S1 | 北极星补齐阶段 |
| S2 | 前端 / 后端边界发现阶段 |
| S3 | 系统边界确认阶段 |
| S4 | 系统边界文档生成阶段 |
| S5 | 架构规范文档生成阶段 |
| S6 | TODO 执行轨道生成阶段 |
| S7 | 代码开发执行阶段 |
| S8 | 冲突处理阶段 |

不能跳阶段，不能把 README 当作执行规则，也不能跳过系统边界和架构规范直接开始开发。

---

## 关键约束

### 唯一合法入口

- 运行入口必须是用户提供的北极星文档 `v0.0.1`。
- 不得从代码、项目名称、技术栈反推项目目标。

### 选择题协议

- 边界不清晰时必须用选择题协议补齐。
- 每题应提供推荐选项。
- 允许用户回复“全部使用推荐选项”。
- 高风险边界必须二次确认。

### TODO 轨道

- `TODO.md` 是项目开发唯一执行轨道。
- 每个 TODO 必须有独立 plan。
- 默认一次只推进一个当前 TODO。

### `project/` 代码根目录规则

- Builder 阶段不得创建 `project/`。
- 进入使用阶段后，最终生成或修改的软件代码必须位于 workspace 根目录的 `project/` 内。
- `docs/`、`TODO.md`、决策记录、边界文档、架构规范文档不属于软件代码根目录。

### 适配入口规则

- `AGENTS.md` 与 `CLAUDE.md` 必须位于 workspace 根目录。
- 它们只作为默认发现入口。
- 真正的核心入口始终是 `.vela/SKILL.md`。

---

## 规则优先级

从高到低，推荐在 README 中这样理解 VELA 的规则层次：

1. `.vela/harness.yaml`：结构与文件清单最高基准。
2. `.vela/SKILL.md`：运行入口最高基准。
3. `.vela/rules/stages/`：阶段执行规则。
4. `.vela/rules/protocols/`：跨阶段协议规则。
5. `.vela/rules/quality/`：质量验收规则。
6. `.vela/schemas/`：结构约束，高于模板。
7. `.vela/templates/`：只提供格式，不覆盖规则。
8. `AGENTS.md` / `CLAUDE.md`：适配入口，不覆盖核心规则。
9. `.vela/README.md`：人类说明文档，不是规则源。

---

## FAQ

### 为什么仓库里不直接放 `.vela/`？

因为这个仓库的目标是维护 **构建规范**，而不是维护一份可能逐渐漂移的构建产物。

### 我应该复制什么到实际项目中？

不是复制本仓库全部内容，而是先让 agent 根据 `VELA_HARNESS_BUILDER.md` 生成 harness，然后将生成的：

```text
AGENTS.md
CLAUDE.md
.vela/
```

放入你的目标 workspace。

### README 是规则源吗？

不是。README 用于向人说明项目。真正的规则源是 BUILDER，以及最终生成的 `.vela/SKILL.md`、`.vela/rules/`、`.vela/schemas/`、`.vela/checklists/`。

### 这个仓库适合谁？

适合希望把 AI coding agent 约束进稳定、文档优先、阶段清晰的开发流程中的用户。

---

如果你更习惯英文说明，请查看 [README.md](./README.md)。
