# VELA Harness Builder Spec

> Strict profile: V6-root-workspace-entrypoints-project-code-root

> VELA = **VELA Enables Lazy Architects**  
> 本文件是 **VELA Harness 文件包构建规范**。  
> 它不是 VELA Harness 本体，也不是项目开发协议运行体。  
> agent 根据本文件只能生成 workspace 根入口文件与 `.vela/` harness 运行体，不得继续进入项目需求分析、项目文档生成或代码开发。

---

## 0. Builder Spec 身份

### 0.1 固定文件名

```text
VELA_HARNESS_BUILDER.md
```

### 0.2 唯一职责

本文件只负责：

```text
指导 agent 生成一个结构固定、规则严格、内容充分、关键约束不被弱化、可复现、可直接复制到不同项目 workspace 使用的 VELA Harness 文件包。
```

### 0.3 Builder 阶段禁止事项

agent 读取本文件后，禁止执行以下行为：

```text
1. 不得分析当前项目。
2. 不得生成北极星文档。
3. 不得生成系统边界文档。
4. 不得生成架构规范文档。
5. 不得生成 TODO.md。
6. 不得写入项目代码。
7. 不得根据当前项目名称、技术栈、功能名调整 .vela/ 文件包结构或内容。
8. 不得新增本规范未定义的文件或目录。
9. 不得省略本规范要求的文件。
10. 不得将本 Builder Spec 当作 VELA Harness 运行体。
11. 不得在生成 workspace 根入口文件与 .vela/ 后继续进入 VELA 运行阶段。
12. 不得把本规范中的“必须包含”改写成可选建议。
```

### 0.4 严格生成原则

```text
1. 输出必须可复现。
2. 文件名必须完全一致。
3. 文件数量必须完全一致。
4. 目录结构必须完全一致。
5. 文件职责必须唯一。
6. 核心规则必须只有唯一来源。
7. 适配入口不得覆盖核心入口。
8. 模板不得覆盖规则。
9. schema 高于 template。
10. 自检失败不得报告成功。
11. 关键规则必须完整保留，不得只做概括描述。
12. 非模板规则文件必须可直接指导 agent 行为，不能只提供标题或理念。
```

### 0.5 语言规则

为提高不同 agent 输出一致性，生成 VELA Harness 时必须遵守：

```text
1. 默认使用简体中文生成全部正文。
2. workspace 根目录 AGENTS.md 与 CLAUDE.md 可额外包含英文适配句，但必须同时包含中文说明。
3. 文件名、状态枚举、package id、协议 id、目录名保持英文固定值。
4. 不得生成中英混杂且语义不完整的句子。
5. 不得出现“implementation方案”这类混合词；应写为“实现方案”。
```


### 0.6 空壳占位内容检测规则

为避免自检规则误伤自身，生成 VELA Harness 时必须按以下方式检测空壳占位内容：

```text
1. 不得使用单字“略”作为全局子串检测条件，因为“策略”“省略”“忽略”等合法词包含该字。
2. 不得因为 checklist 中描述检查规则而判定 checklist 自身违规。
3. 非模板文件真正违规的是“占位性正文”，不是对占位行为的规则描述。
4. 占位性正文包括独立行或段落中的：<待填写>、<以后补充>、[TBD]、TODO: later、placeholder only、to be filled、此处略、待补充。
5. 模板文件允许使用 <占位符>，但必须同时具备完整标题结构和填充约束。
6. 非模板文件可以说明“不得出现占位性正文”，但不得把该说明当作实际待填内容。
```

---

## 1. 生成目标

### 1.1 输出位置

agent 必须生成以下输出：

```text
AGENTS.md
CLAUDE.md
.vela/
```

其中：

```text
1. AGENTS.md 与 CLAUDE.md 必须位于目标 workspace 根目录。
2. .vela/ 必须位于目标 workspace 根目录。
3. .vela/ 内部保留 VELA Harness 运行体。
4. 根目录 AGENTS.md 与 CLAUDE.md 只作为不同 agent 的默认发现入口，不得承载第二套规则。
```

### 1.2 VELA Harness 元信息

```yaml
name: "VELA Harness"
recursive_name: "VELA Enables Lazy Architects"
package_id: "vela"
version: "v0.1.2"
runtime_directory: ".vela"
runtime_code_root: "project"
workspace_entrypoints:
  codex: "AGENTS.md"
  claude_code: "CLAUDE.md"
canonical_runtime_entry: ".vela/SKILL.md"
```

### 1.3 目录命名规则

```text
1. 默认运行体目录必须为 .vela/。
2. 默认 workspace 根入口文件必须为 AGENTS.md 与 CLAUDE.md。
3. 不得输出 development-harness/、harness/、project-harness/ 或其他运行体目录名。
4. 如果用户明确要求改名，必须同步修改 .vela/harness.yaml 中的 package.directory 字段和 required_files 中的 workspace 相对路径。
5. 未经用户明确要求，不得改名。
```

### 1.4 VELA 使用阶段代码输出目录规则

```text
1. Builder 阶段只生成 AGENTS.md、CLAUDE.md 与 .vela/，不得创建 project/。
2. VELA 使用阶段进入代码开发后，最终生成的软件代码根目录固定为 workspace 根目录下的 project/。
3. 软件代码包括源码、包管理文件、构建脚本、运行脚本、测试代码、静态资源、配置样例和程序运行所需的工程文件。
4. docs/、TODO.md、决策记录、边界文档、架构规范文档属于项目文档产物，不属于软件代码根目录，不得为了满足 project/ 规则迁入 project/。
5. agent 不得在 workspace 根目录直接生成 main、src、package、Cargo、配置、测试或构建类软件代码文件；这些内容必须位于 project/ 内。
6. 如果使用 VELA 阶段发现已有软件代码位于 project/ 外，必须停止并通过选择题确认迁移、引用或保留策略，不得静默混写。
```

---

## 2. 完整文件树

agent 必须生成以下完整文件树，不得增删改名：

```text
AGENTS.md
CLAUDE.md
.vela/
├── SKILL.md
├── README.md
├── harness.yaml
├── CHANGELOG.md
├── rules/
│   ├── stages/
│   │   ├── 00-entry-validation.md
│   │   ├── 01-north-star-completion.md
│   │   ├── 02-boundary-discovery.md
│   │   ├── 03-boundary-confirmation.md
│   │   ├── 04-system-boundary-generation.md
│   │   ├── 05-architecture-spec-generation.md
│   │   ├── 06-todo-track-generation.md
│   │   ├── 07-development-execution.md
│   │   └── 08-conflict-resolution.md
│   ├── protocols/
│   │   ├── question-protocol.md
│   │   ├── interaction-adapter-protocol.md
│   │   ├── frontend-backend-boundary-protocol.md
│   │   ├── cross-boundary-ownership-protocol.md
│   │   ├── version-progression-protocol.md
│   │   ├── todo-execution-protocol.md
│   │   ├── reference-linking-protocol.md
│   │   ├── state-management-protocol.md
│   │   └── conflict-resolution-protocol.md
│   └── quality/
│       ├── universal-document-quality.md
│       ├── north-star-quality.md
│       ├── system-boundary-quality.md
│       ├── architecture-spec-quality.md
│       └── todo-quality.md
├── templates/
│   ├── north-star-v0.0.1.template.md
│   ├── north-star-v0.1.0.template.md
│   ├── north-star-v0.2.0.template.md
│   ├── frontend-boundary.template.md
│   ├── backend-boundary.template.md
│   ├── cross-boundary-ownership.template.md
│   ├── architecture-spec.template.md
│   ├── todo.template.md
│   ├── todo-plan-not-started.template.md
│   ├── todo-plan-in-progress.template.md
│   ├── state.template.md
│   ├── decision-record.template.md
│   └── project-artifacts-structure.md
├── schemas/
│   ├── harness-manifest.schema.md
│   ├── north-star.schema.md
│   ├── system-boundary.schema.md
│   ├── architecture-spec.schema.md
│   ├── boundary-index.schema.md
│   ├── architecture-index.schema.md
│   ├── todo.schema.md
│   ├── todo-plan.schema.md
│   └── state.schema.md
├── checklists/
│   ├── builder-output-checklist.md
│   ├── runtime-start-checklist.md
│   ├── stage-completion-checklist.md
│   ├── version-progression-checklist.md
│   ├── todo-readiness-checklist.md
│   ├── todo-completion-checklist.md
│   └── conflict-resolution-checklist.md
└── extensions/
    └── README.md
```

### 2.1 未授权文件规则

除上述文件外，agent 不得生成任何额外文件。

明确禁止：

```text
.vela/AGENTS.md
.vela/CLAUDE.md
.vela/examples/
.vela/docs/
.vela/project/
.vela/sample-project/
.vela/generated/
.vela/TODO.md
.vela/north-star/
.vela/system-boundary/
.vela/architecture/
.vela/todo-plans/
.vela/project-artifacts/
```

### 2.2 复制到项目 workspace 的规则

生成完成后，用户可以只复制以下内容到目标项目 workspace：

```text
AGENTS.md
CLAUDE.md
.vela/
```

必须满足：

```text
1. 用户无需复制 Builder 源文件 VELA_HARNESS_BUILDER.md。
2. 用户无需复制 Builder 包说明 README.md。
3. AGENTS.md 与 CLAUDE.md 位于 workspace 根目录后，Codex 与 Claude Code 可按默认入口发现 harness。
4. AGENTS.md 与 CLAUDE.md 必须转发到 .vela/SKILL.md，不得复制 .vela/SKILL.md 的完整规则正文。
5. .vela/SKILL.md 仍是 VELA Harness 的唯一核心运行入口。
```

### 2.3 内容充分性规则

为避免生成“只有标题但不可运行”的空壳 harness，agent 必须遵守：

```text
1. workspace 根入口文件、.vela 根文件、规则文件、协议文件、质量文件、schema 文件、checklist 文件不得只写标题。
2. rules/stages/ 每个阶段文件必须写清进入条件、输入、执行规则、输出、停止条件、禁止事项、引用规则。
3. rules/protocols/ 每个协议文件必须写清协议目的、适用阶段、强制规则、输出要求、禁止事项。
4. rules/quality/ 每个质量文件必须写清适用对象、质量属性、禁止内容、验收标准。
5. schemas/ 每个 schema 文件必须写清适用对象、必填字段、字段顺序、禁止字段、校验规则。
6. checklists/ 每个 checklist 文件必须写清使用场景、检查项、失败处理。
7. templates/ 可以包含占位符，但必须包含完整标题结构和填充约束。
8. 非模板文件禁止出现占位性正文。生成到 `.vela/` 内部与 workspace 根入口文件中的检测规则必须自包含写明：不得使用单字“略”作为全局子串检测条件；真正违规的是独立行或段落中的 `<待填写>`、`<以后补充>`、`[TBD]`、`TODO: later`、`placeholder only`、`to be filled`、`此处略`、`待补充`。
9. “TODO.md”“TODO plan”“TODO 轨道”等术语允许出现，不视为占位内容。
10. 所有非模板规则文件必须包含足够的强制规则，不能只做概括性描述。
11. checklist 中描述“如何检查占位性正文”不视为占位性正文。
```

---

## 3. 生成顺序

agent 必须按以下顺序生成：

```text
1. 创建 workspace 根入口文件 AGENTS.md 与 CLAUDE.md。
2. 创建 .vela/ 运行体根目录。
3. 创建 .vela/ 根文件：SKILL.md、README.md、harness.yaml、CHANGELOG.md。
4. 创建 .vela/rules/stages/ 阶段规则文件。
5. 创建 .vela/rules/protocols/ 协议文件。
6. 创建 .vela/rules/quality/ 质量标准文件。
7. 创建 .vela/templates/ 模板文件。
8. 创建 .vela/schemas/ 结构约束文件。
9. 创建 .vela/checklists/ 检查清单文件。
10. 创建 .vela/extensions/README.md。
11. 依据 .vela/harness.yaml 执行结构自检。
12. 执行内容职责自检。
13. 执行禁止事项自检。
14. 执行引用关系自检。
15. 执行内容充分性自检。
16. 执行关键短语自检。
17. 执行外部依赖表达自检。
18. 如发现可修复问题，修复后重新自检。
19. 如发现不可修复问题，停止并报告缺失项。
```

---

## 4. harness.yaml 严格字段约束

`.vela/harness.yaml` 是 workspace 输出结构自检最高基准。  
它必须使用 workspace 根目录相对路径完整列出所有必需文件，不得只列 `.vela/` 内部文件。

### 4.1 必填内容

`.vela/harness.yaml` 必须包含以下字段，并完整列出全部 59 个文件：

```yaml
name: "VELA Harness"
recursive_name: "VELA Enables Lazy Architects"
package:
  id: "vela"
  directory: ".vela"
  version: "v0.1.2"
entrypoints:
  canonical_runtime: ".vela/SKILL.md"
  codex: "AGENTS.md"
  claude_code: "CLAUDE.md"
  human: ".vela/README.md"
entrypoint_policy:
  adapters_outside_runtime_directory: true
  adapters_are_workspace_root_files: true
  adapters_must_delegate_to: ".vela/SKILL.md"
runtime_project_policy:
  code_root: "project"
  builder_must_not_create_code_root: true
  generated_software_code_must_stay_under_code_root: true
  project_documents_remain_outside_code_root: true
required_files:
  - "AGENTS.md"
  - "CLAUDE.md"
  - ".vela/SKILL.md"
  - ".vela/README.md"
  - ".vela/harness.yaml"
  - ".vela/CHANGELOG.md"
  - ".vela/rules/stages/00-entry-validation.md"
  - ".vela/rules/stages/01-north-star-completion.md"
  - ".vela/rules/stages/02-boundary-discovery.md"
  - ".vela/rules/stages/03-boundary-confirmation.md"
  - ".vela/rules/stages/04-system-boundary-generation.md"
  - ".vela/rules/stages/05-architecture-spec-generation.md"
  - ".vela/rules/stages/06-todo-track-generation.md"
  - ".vela/rules/stages/07-development-execution.md"
  - ".vela/rules/stages/08-conflict-resolution.md"
  - ".vela/rules/protocols/question-protocol.md"
  - ".vela/rules/protocols/interaction-adapter-protocol.md"
  - ".vela/rules/protocols/frontend-backend-boundary-protocol.md"
  - ".vela/rules/protocols/cross-boundary-ownership-protocol.md"
  - ".vela/rules/protocols/version-progression-protocol.md"
  - ".vela/rules/protocols/todo-execution-protocol.md"
  - ".vela/rules/protocols/reference-linking-protocol.md"
  - ".vela/rules/protocols/state-management-protocol.md"
  - ".vela/rules/protocols/conflict-resolution-protocol.md"
  - ".vela/rules/quality/universal-document-quality.md"
  - ".vela/rules/quality/north-star-quality.md"
  - ".vela/rules/quality/system-boundary-quality.md"
  - ".vela/rules/quality/architecture-spec-quality.md"
  - ".vela/rules/quality/todo-quality.md"
  - ".vela/templates/north-star-v0.0.1.template.md"
  - ".vela/templates/north-star-v0.1.0.template.md"
  - ".vela/templates/north-star-v0.2.0.template.md"
  - ".vela/templates/frontend-boundary.template.md"
  - ".vela/templates/backend-boundary.template.md"
  - ".vela/templates/cross-boundary-ownership.template.md"
  - ".vela/templates/architecture-spec.template.md"
  - ".vela/templates/todo.template.md"
  - ".vela/templates/todo-plan-not-started.template.md"
  - ".vela/templates/todo-plan-in-progress.template.md"
  - ".vela/templates/state.template.md"
  - ".vela/templates/decision-record.template.md"
  - ".vela/templates/project-artifacts-structure.md"
  - ".vela/schemas/harness-manifest.schema.md"
  - ".vela/schemas/north-star.schema.md"
  - ".vela/schemas/system-boundary.schema.md"
  - ".vela/schemas/architecture-spec.schema.md"
  - ".vela/schemas/boundary-index.schema.md"
  - ".vela/schemas/architecture-index.schema.md"
  - ".vela/schemas/todo.schema.md"
  - ".vela/schemas/todo-plan.schema.md"
  - ".vela/schemas/state.schema.md"
  - ".vela/checklists/builder-output-checklist.md"
  - ".vela/checklists/runtime-start-checklist.md"
  - ".vela/checklists/stage-completion-checklist.md"
  - ".vela/checklists/version-progression-checklist.md"
  - ".vela/checklists/todo-readiness-checklist.md"
  - ".vela/checklists/todo-completion-checklist.md"
  - ".vela/checklists/conflict-resolution-checklist.md"
  - ".vela/extensions/README.md"
required_directories:
  - ".vela/rules/stages"
  - ".vela/rules/protocols"
  - ".vela/rules/quality"
  - ".vela/templates"
  - ".vela/schemas"
  - ".vela/checklists"
  - ".vela/extensions"
extension_slots:
  - ".vela/extensions"
forbidden_paths:
  - ".vela/AGENTS.md"
  - ".vela/CLAUDE.md"
forbidden_content:
  - "project-specific names"
  - "technology stacks"
  - "concrete feature names"
  - "example project documents"
  - "generated project artifacts"
  - "unauthorized files"
  - "placeholder-only rule files"
  - "weakened critical rules"
priority:
  structure: ".vela/harness.yaml"
  canonical_runtime: ".vela/SKILL.md"
  stage_rules: ".vela/rules/stages"
  protocols: ".vela/rules/protocols"
  quality_rules: ".vela/rules/quality"
  schemas: ".vela/schemas"
  templates: ".vela/templates"
  checklists: ".vela/checklists"
  adapters:
    - "AGENTS.md"
    - "CLAUDE.md"
  human_docs:
    - ".vela/README.md"
```

### 4.2 禁止字段

`.vela/harness.yaml` 禁止包含：

```text
1. 真实生成时间。
2. 当前项目名。
3. 当前技术栈。
4. 当前用户信息。
5. 当前项目功能名。
6. 当前项目开发状态。
```

### 4.3 路径解释规则

```text
1. .vela/harness.yaml 中的 required_files 必须全部使用 workspace 根目录相对路径。
2. AGENTS.md 与 CLAUDE.md 必须在 workspace 根目录。
3. .vela/SKILL.md 是核心运行入口。
4. .vela/README.md 是人类说明文件。
5. .vela/AGENTS.md 与 .vela/CLAUDE.md 禁止生成，避免 agent 只扫描 workspace 根目录时无法发现入口。
6. runtime_project_policy.code_root 必须固定为 project，表示 VELA 使用阶段最终生成的软件代码根目录为 workspace/project/。
7. runtime_project_policy.builder_must_not_create_code_root 必须为 true，表示 Builder 阶段不得提前创建 project/。
```

---

## 5. 入口与根文件内容蓝图

以下蓝图规定 workspace 根入口文件与 `.vela/` 根文件必须包含的一级标题、核心职责、禁止内容。

### 5.1 .vela/SKILL.md

#### 职责

```text
VELA Harness 的核心运行入口。
负责说明 VELA 的运行身份、阶段顺序、读取路径、强制行为和禁止行为。
不承载详细模板正文。
```

#### 必备一级标题

```text
# VELA Harness
## 1. 身份
## 2. 唯一合法入口
## 3. 运行阶段状态机
## 4. 必须读取的规则路径
## 5. 文档驱动开发原则
## 6. 前端/后端边界原则
## 7. 选择题协议入口
## 8. 版本推进规则入口
## 9. TODO 执行规则入口
## 10. 冲突处理入口
## 11. 禁止事项
## 12. 文件优先级
```

#### 必须包含的精确规则

`.vela/SKILL.md` 必须包含下列规则，不得只概括表达：

```text
1. VELA 的运行入口是用户提供的北极星文档 v0.0.1。
2. VELA 不负责从零创造项目。
3. 文档构建完成前禁止进入代码开发。
4. 代码开发只能由 TODO.md 当前任务驱动。
5. 任何边界不清晰处必须使用选择题协议。
6. 当前 agent 支持 TUI/GUI/菜单时优先使用原生交互；不可用时回退为文本编号选择题。
7. AGENTS.md、CLAUDE.md、.vela/README.md 不得覆盖 .vela/SKILL.md。
8. .vela/rules/stages/ 是阶段规则来源。
9. .vela/rules/protocols/ 是跨阶段协议来源。
10. .vela/rules/quality/ 是文档质量验收来源。
11. .vela/schemas/ 是结构约束来源。
12. .vela/templates/ 只提供格式，不覆盖规则。
13. AGENTS.md 与 CLAUDE.md 必须位于 workspace 根目录。
14. Builder 阶段不得创建 project/。
15. VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内。
16. docs/、TODO.md、决策记录、边界文档和架构规范文档不得迁入 project/。
```

#### 禁止内容

```text
不得包含具体项目名称、技术栈、功能名。
不得包含示例项目。
不得直接生成项目产物。
不得把当前仓库代码状态写入本文件。
```

### 5.2 AGENTS.md（workspace 根目录）

#### 职责

```text
Codex 默认发现入口。
要求 Codex 读取并遵守 .vela/SKILL.md。
不得形成第二套规则。
```

#### 必备一级标题

```text
# VELA Codex Adapter
## 1. 目的
## 2. 必读入口
## 3. 规则优先级
## 4. 禁止分叉
```

#### 必须声明

```text
AGENTS.md 只是 Codex 默认发现入口。
AGENTS.md 必须位于 workspace 根目录。
Codex 必须把 .vela/SKILL.md 视为 VELA Harness 的唯一核心入口。
如果 AGENTS.md 与 .vela/SKILL.md 冲突，以 .vela/SKILL.md 为准。
AGENTS.md 不得引入 Codex 专属替代阶段。
```

### 5.3 CLAUDE.md（workspace 根目录）

#### 职责

```text
Claude Code 默认发现入口。
要求 Claude Code 读取并遵守 .vela/SKILL.md。
不得形成第二套规则。
```

#### 必备一级标题

```text
# VELA Claude Code Adapter
## 1. 目的
## 2. 必读入口
## 3. 规则优先级
## 4. 禁止分叉
```

#### 必须声明

```text
CLAUDE.md 只是 Claude Code 默认发现入口。
CLAUDE.md 必须位于 workspace 根目录。
Claude Code 必须把 .vela/SKILL.md 视为 VELA Harness 的唯一核心入口。
如果 CLAUDE.md 与 .vela/SKILL.md 冲突，以 .vela/SKILL.md 为准。
CLAUDE.md 不得引入 Claude 专属替代阶段。
```

### 5.4 .vela/README.md

#### 职责

```text
面向人类用户说明 VELA Harness 是什么、如何使用、入口是什么。
不作为 agent 执行规则来源。
```

#### 必备一级标题

```text
# VELA Harness
## 1. VELA 是什么
## 2. VELA 不是什么
## 3. 必需输入
## 4. 使用方式
## 5. 运行时项目产物
## 6. 文件结构
## 7. 规则优先级
```

#### 必须声明

```text
.vela/README.md 只用于人类说明。
.vela/SKILL.md 是 agent 的唯一核心入口。
VELA 使用阶段最终生成的软件代码必须位于 workspace/project/。
Builder 阶段不得创建 project/。
```

### 5.5 .vela/CHANGELOG.md

#### 职责

```text
记录 VELA Harness 文件包本身的版本变更。
不记录具体项目开发日志。
```

#### 必备一级标题

```text
# CHANGELOG
## v0.1.2
## v0.1.1
## v0.1.0
```

#### v0.1.2 必须说明

```text
增加 project/ 软件代码根目录规则：Builder 阶段不得创建 project/；VELA 使用阶段进入代码开发后，最终生成的软件代码必须位于 workspace/project/ 内；项目文档产物仍按文档结构存放。
```

#### v0.1.1 必须说明

```text
将 AGENTS.md 与 CLAUDE.md 移动到 workspace 根目录，使用户复制 AGENTS.md、CLAUDE.md 与 .vela/ 后即可让 Codex 与 Claude Code 按默认入口发现 harness。
```

#### v0.1.0 必须说明

```text
初始 VELA Harness 文件包结构。
包含核心入口、适配入口、阶段规则、复用协议、质量标准、模板、schema、checklist 与扩展槽位。
```

---

## 6. 阶段规则文件严格蓝图

`rules/stages/` 中每个文件都必须包含：

```text
# <阶段名>
## 1. 阶段身份
## 2. 进入条件
## 3. 输入
## 4. 执行规则
## 5. 输出
## 6. 停止条件
## 7. 禁止事项
## 8. 引用规则
```

每个阶段文件不得少于 8 个二级标题。  
每个阶段文件的“执行规则”不得少于 4 条。  
每个阶段文件的“禁止事项”不得少于 2 条。

### 6.1 固定阶段状态机

`.vela/` 运行时必须采用以下阶段，不得跳阶段：

```text
S0：入口验收阶段
S1：北极星补齐阶段
S2：前端/后端边界发现阶段
S3：系统边界确认阶段
S4：系统边界文档生成阶段
S5：架构规范文档生成阶段
S6：TODO 执行轨道生成阶段
S7：代码开发执行阶段
S8：冲突处理阶段
```

### 6.2 各阶段必须写入的核心规则

#### S0：入口验收阶段

必须写入：

```text
1. 输入只能是用户提供的北极星文档 v0.0.1。
2. 不得从代码、项目名、技术栈反向生成入口。
3. 必须检查 v0.0.1 是否包含项目目标、目标用户、核心场景、MVP、非目标、约束、功能方向。
4. 不完整时进入 S1，不得自动补齐。
5. 合格时进入 S2。
```

#### S1：北极星补齐阶段

必须写入：

```text
1. 只能通过选择题补齐缺失信息。
2. 补齐后仍保持 v0.0.1 身份。
3. 补齐决策必须写入决策记录。
4. 不得提前升级到 v0.1.0。
5. 不得引入未经用户确认的项目外幻想内容。
```

#### S2：前端/后端边界发现阶段

必须写入：

```text
1. 前端候选和后端候选必须并列识别。
2. 前端定义为用户接触层、表现层、操作层。
3. 后端定义为核心能力层、数据层、执行层。
4. 必须识别跨端候选边界。
5. 候选边界不得直接写成最终结论。
```

#### S3：系统边界确认阶段

必须写入：

```text
1. 候选边界必须通过选择题确认。
2. 必须确认前端边界、后端边界、跨端归属。
3. 跨端系统采用主归属文档 + 镜像边界。
4. 未确认边界不得进入 S4。
5. 高风险边界必须二次确认。
```

#### S4：系统边界文档生成阶段

必须写入：

```text
1. 只能根据已确认边界生成文档。
2. 必须生成前端边界文档组。
3. 必须生成后端边界文档组。
4. 必须生成跨端归属表。
5. 必须同步生成北极星文档 v0.1.0。
6. v0.1.0 必须包含系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接。
7. 系统边界文档不得写代码实现方案。
```

#### S5：架构规范文档生成阶段

必须写入：

```text
1. 必须基于系统边界文档生成。
2. 不得绕过系统边界文档直接根据北极星生成。
3. 每个已确认系统边界都必须生成对应架构规范文档。
4. 架构规范文档不得修改系统边界。
5. 发现边界不足时必须回到选择题协议。
6. 完成后生成北极星文档 v0.2.0。
7. v0.2.0 必须包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明。
```

#### S6：TODO 执行轨道生成阶段

必须写入：

```text
1. 只能在 v0.2.0 和全部架构规范完成后生成。
2. TODO.md 是唯一代码开发执行轨道。
3. TODO.md 必须采用主阶段 → 子任务 → 独立 plan 链接结构。
4. 每个 TODO 必须包含状态、任务名、所属阶段、plan 链接、依赖项、验收标准摘要。
5. 每个 TODO 必须生成独立 plan。
6. 未开始 plan 只能写概述和正式开发前重新生成详细 plan 的声明。
7. TODO.md 必须区分当前阶段必做、后续扩展、待确认。
8. TODO.md 不得脱离架构规范文档生成。
9. TODO plan 的软件代码影响范围必须使用 project/ 下的 workspace 相对路径表达。
10. TODO.md 不得把 project/ 外的软件代码路径列为新生成目标。
```

#### S7：代码开发执行阶段

必须写入：

```text
1. 默认一次只推进一个当前 TODO。
2. 开始 TODO 前必须检查状态、读取 plan、读取相关规范、检查 project/ 内的当前代码状态。
3. 概述 plan 必须在开始前升级为详细 plan。
4. 依赖或上一阶段 TODO 未完成时必须先补齐。
5. 跨系统 TODO 必须标注涉及的系统边界文档和架构规范文档。
6. 完成后必须更新 TODO.md、对应 plan 文档、完成记录和必要文档影响说明。
7. 不得在没有 TODO 驱动时写代码。
8. 每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环。
9. VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内。
10. 不得在 workspace 根目录直接生成 main、src、package、Cargo、配置、测试或构建类软件代码文件。
11. 如果发现已有软件代码位于 project/ 外，必须停止并通过选择题确认迁移、引用或保留策略。
```

#### S8：冲突处理阶段

必须写入：

```text
1. 冲突包括代码/文档/TODO/规范不一致。
2. agent 必须停止继续开发。
3. agent 必须报告冲突项。
4. agent 必须通过选择题让用户选择修复路径。
5. 不得自行静默修复边界。
6. 修复后必须检查受影响文档并回到正确阶段。
```

### 6.3 阶段文件关键短语要求

以下短语必须出现在对应阶段文件中。缺失即 Builder 自检失败：

| 文件 | 必须出现的关键短语 |
|---|---|
| `rules/stages/04-system-boundary-generation.md` | `v0.1.0 必须包含系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接` |
| `rules/stages/05-architecture-spec-generation.md` | `v0.2.0 必须包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明` |
| `rules/stages/06-todo-track-generation.md` | `TODO.md 不得脱离架构规范文档生成` |
| `rules/stages/07-development-execution.md` | `跨系统 TODO 必须标注涉及的系统边界文档与架构规范文档` |
| `rules/stages/07-development-execution.md` | `每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环` |
| `rules/stages/07-development-execution.md` | `VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内` |

### 6.4 阶段规则不得弱化

```text
1. 不得把 v0.1.0 的必备链接概括成“相关链接”。
2. 不得把 v0.2.0 的必备链接概括成“架构索引表等内容”。
3. 不得省略 TODO 轨道生成阶段对架构规范文档的依赖。
4. 不得省略跨系统 TODO 的标注责任。
5. 不得把 TODO 验收闭环只写在停止条件中，必须写入执行规则。
6. 不得把 project/ 软件代码根目录规则写成建议，必须作为代码开发执行阶段的强制规则。
```

---

## 7. 协议文件严格蓝图

`rules/protocols/` 中每个文件都必须包含：

```text
# <协议名>
## 1. 协议目的
## 2. 适用阶段
## 3. 强制规则
## 4. 输出要求
## 5. 禁止事项
```

每个协议文件不得少于 5 个二级标题。  
每个协议文件“强制规则”不得少于 5 条。  
非模板协议文件禁止只写概念描述。

### 7.1 question-protocol.md 必须包含

```text
1. 适用于所有需要用户确认边界的阶段。
2. 每轮问一组数量受限的问题。
3. 推荐每轮 5–12 题；不足 5 题时允许少于 5 题。
4. 每题必须有推荐选项。
5. 推荐项必须基于已有文档和当前阶段目标。
6. 允许用户回复“全部使用推荐选项”。
7. 必须标注单选/多选。
8. “其他/自定义”必须作为最后一个选项。
9. 回答不完整时只追问缺失项。
10. 高风险边界必须二次确认。
11. 高风险边界包括数据删除、权限、安全、联网、插件执行、不可逆操作、用户隐私、外部服务依赖、代码自动修改。
12. 选择题结果必须写回文档或状态记录。
13. 必须保留问题-答案记录作为边界决策记录。
14. 可以引入项目外常识补充候选项，但不能覆盖北极星文档。
15. 完成后必须判断问题是否完全解决；未解决继续选择题，已解决再生成或更新文档。
16. 冲突修复必须报告冲突项并提供有限修复选项。
17. 达到边界完整条件后停止。
```

### 7.2 interaction-adapter-protocol.md 必须包含

```text
1. 选择题协议是逻辑层。
2. TUI/GUI/弹窗/菜单是呈现层。
3. 文本编号选择题是通用回退层。
4. 若当前 agent 支持原生交互，优先使用原生交互形式。
5. 若不可用或不确定，回退为文本编号选择题。
6. 若当前对话环境不适合多轮问答，可生成待用户填写的选择题表单文档。
7. 交互形式不得改变选择题协议本身。
```

### 7.3 frontend-backend-boundary-protocol.md 必须包含

```text
1. 前端 = 用户接触层 / 表现层 / 操作层。
2. 后端 = 核心能力层 / 数据层 / 执行层。
3. 桌面软件、CLI、浏览器插件也必须映射到该二分法。
4. CLI 中命令参数、输出文本、交互提示属于前端；命令执行逻辑、数据处理、文件操作属于后端。
5. 不得把系统边界直接按功能平铺。
6. 必须先生成前端候选和后端候选，再确认跨端归属。
```

### 7.4 cross-boundary-ownership-protocol.md 必须包含

```text
1. 跨端系统采用主归属文档 + 镜像边界。
2. 主归属文档定义系统本体职责。
3. 镜像边界定义另一端如何展示、调用、消费或反馈。
4. 必须记录主归属理由。
5. 必须记录前端状态与后端状态的责任分界。
6. 必须记录错误状态由谁定义、谁展示。
7. 不得把跨端系统混写成一份职责不清的文档。
```

### 7.5 version-progression-protocol.md 必须包含

```text
1. 北极星文档固定推进为 v0.0.1 → v0.1.0 → v0.2.0。
2. v0.0.1 是用户提供入口。
3. v0.0.1 不完整时通过选择题补齐后仍保持 v0.0.1。
4. v0.1.0 只能在前端边界、后端边界、跨端归属确认并生成系统边界文档组后生成。
5. v0.1.0 必须包含系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接。
6. v0.2.0 只能在每个已确认系统边界都生成对应架构规范文档后生成。
7. v0.2.0 必须包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明。
8. 系统边界文档与架构规范文档必须记录来源版本与自身变更记录。
9. TODO.md 不通过文件名版本化，但必须保留状态变更记录。
10. 更新不得静默覆盖，必须记录变更原因。
11. 版本推进前必须通过阶段完成 checklist。
12. 推进失败时停止并报告缺失项。
```

### 7.6 todo-execution-protocol.md 必须包含

```text
1. TODO.md 是项目开发唯一执行轨道。
2. TODO.md 只能在北极星 v0.2.0 与全部架构规范文档完成后生成。
3. TODO.md 不得脱离架构规范文档生成。
4. TODO.md 组织方式必须为主阶段 → 子任务 → 每个子任务链接独立 plan 文档。
5. 每个 TODO 必须包含状态、任务名、所属阶段、plan 链接、依赖项、验收标准摘要。
6. TODO 状态枚举固定为 not_started / planning / in_progress / blocked / done。
7. 复选框只作为可读状态辅助，真实状态以状态字段为准。
8. 每个 TODO 必须有独立 plan，项目文档产物中默认存放于 docs/todo-plans/。
9. 未开始 plan 必须包含概述、正式开发前重新生成详细 plan 声明、生成依据。
10. 进行中 plan 必须包含当前代码状态分析、输入依据、详细开发步骤、文件影响范围、验收标准、禁止事项、回写要求。
11. 开始 TODO 前必须检查状态、读取 plan、读取相关规范、检查 project/ 内的当前代码，必要时升级 plan。
12. 依赖或上一阶段 TODO 未完成时必须补齐，不能继续推进依赖它的 TODO。
13. 跨系统 TODO 必须标注涉及的系统边界文档与架构规范文档。
14. 默认一次只推进一个当前 TODO。
15. 完成后必须更新 TODO.md、对应 plan 文档、完成记录和必要文档影响说明。
16. TODO 设计不合理或需新增任务时必须停止并通过选择题确认。
17. TODO.md 必须区分当前阶段必做、后续扩展、待确认。
18. 每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环。
19. VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内。
20. TODO plan 的文件影响范围中，软件代码路径必须以 project/ 开头。
21. TODO plan 不得把 docs/、TODO.md、决策记录、边界文档或架构规范文档当作软件代码路径。
22. 如果已有软件代码位于 project/ 外，必须在继续开发前通过选择题确认迁移、引用或保留策略。
```

### 7.7 reference-linking-protocol.md 必须包含

```text
1. 北极星 v0.1.0 必须链接系统边界文档。
2. 北极星 v0.2.0 必须链接系统边界文档和架构规范文档。
3. 子文档必须反向引用来源北极星版本。
4. 架构规范文档必须引用来源系统边界文档。
5. TODO 条目必须链接独立 plan。
6. TODO plan 必须引用相关北极星、边界、架构规范文档。
7. 链接不得只列名称，必须使用相对路径。
```

### 7.8 state-management-protocol.md 必须包含

```text
1. Builder 阶段只提供 state.template.md。
2. 使用 VELA 阶段才生成项目状态文件。
3. 状态文件必须记录当前阶段、当前北极星版本、边界完成状态、架构规范完成状态、TODO 轨道状态。
4. 阶段推进必须更新状态文件。
5. 状态文件不得替代正式文档。
6. 状态文件与正式文档冲突时，必须进入冲突处理协议。
```

### 7.9 conflict-resolution-protocol.md 必须包含

```text
1. 冲突包括文档与代码、TODO 与代码、架构规范与系统边界、状态文件与正式文档不一致。
2. 发现冲突时必须停止当前推进。
3. 必须报告冲突项。
4. 必须判断冲突类型：代码错误、文档过时、需求边界不足、TODO 不合理、状态记录错误。
5. 必须通过选择题让用户选择修复路径。
6. 不得静默选择以代码为准或以文档为准。
7. 修复后必须检查受影响版本和 TODO。
```

---

## 8. 质量标准文件严格蓝图

`rules/quality/` 中每个文件都必须包含：

```text
# <质量标准名>
## 1. 适用对象
## 2. 必须满足的质量属性
## 3. 禁止内容
## 4. 验收标准
```

每个质量文件必须包含可检查的验收标准。

### 8.1 universal-document-quality.md 必须包含

```text
1. 避免项目外幻想内容，除非作为候选项等待用户选择。
2. 区分已确认内容与待确认内容。
3. 开放性内容只能放在“非当前阶段 / 后续扩展 / 待确认”区域。
4. 必须包含非目标 / 禁止事项。
5. 必须包含可检查验收标准。
6. 必须包含最小变更记录。
7. 更新不得静默覆盖，必须保留变更原因。
8. 文档不得使用模糊责任词替代明确边界。
9. 文档不得把推荐项写成用户已确认结论。
```

### 8.2 north-star-quality.md 必须包含

```text
质量属性：客观、边界明确、可回归、可作为后续文档依据。
验收标准：
1. 包含项目目标、目标用户、核心场景、MVP、非目标、技术/平台约束、核心功能方向。
2. v0.1.0 包含系统边界索引。
3. v0.2.0 包含架构规范索引。
4. 不包含代码实现细节。
```

### 8.3 system-boundary-quality.md 必须包含

```text
质量属性：客观、完整、回归、独立、描述性、定义性。
验收标准：
1. 定义系统职责。
2. 定义输入与输出，或定义用户可见范围与操作路径。
3. 定义状态、失败场景或错误反馈。
4. 定义与另一端边界关系。
5. 定义非目标与禁止事项。
6. 定义验收标准。
7. 不写代码实现方案。
8. 不修改北极星目标。
```

### 8.4 architecture-spec-quality.md 必须包含

```text
质量属性：客观、完整、回归、独立、架构性、规范性、可开发、可验收。
验收标准：
1. 引用来源系统边界文档。
2. 包含目录设计、模块职责、数据结构、接口、状态流、伪代码、错误处理、测试策略。
3. 代码目录设计必须以 project/ 作为软件代码根目录。
4. 不修改系统边界。
5. 发现边界不足时回到选择题协议。
```

### 8.5 todo-quality.md 必须包含

```text
质量属性：阶段化、可执行、可追踪、可回写、每项链接独立 plan。
验收标准：
1. 每个 TODO 有状态字段。
2. 每个 TODO 有独立 plan 链接。
3. 每个 TODO 有依赖项。
4. 每个 TODO 有验收标准摘要。
5. TODO.md 区分当前阶段必做、后续扩展、待确认。
6. TODO 完成后有回写记录。
7. 复选框不得替代状态字段。
```

---

## 9. 模板文件严格蓝图

`templates/` 中每个模板必须包含固定标题结构。  
模板可以使用 `<占位符>`，但不得使用项目具体内容。

### 9.1 北极星文档模板

`north-star-v0.0.1.template.md` 必须包含：

```text
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

`north-star-v0.1.0.template.md` 必须包含 v0.0.1 全部标题，并增加：

```text
## 11. 系统边界索引表
## 12. 前端边界文档链接
## 13. 后端边界文档链接
## 14. 跨端归属表链接
```

`north-star-v0.2.0.template.md` 必须包含 v0.1.0 全部标题，并增加：

```text
## 15. 架构规范索引表
## 16. 架构规范文档链接
## 17. TODO 生成依据说明
```

### 9.2 前端边界模板

`frontend-boundary.template.md` 必须包含：

```text
# <前端边界名称>
## 1. 来源北极星版本
## 2. 主体职责
## 3. 用户可见范围
## 4. 用户操作路径
## 5. 页面/界面/命令呈现
## 6. 状态呈现
## 7. 错误反馈
## 8. 与后端边界关系
## 9. 非目标
## 10. 禁止事项
## 11. 验收标准
## 12. 决策记录
## 13. 变更记录
```

### 9.3 后端边界模板

`backend-boundary.template.md` 必须包含：

```text
# <后端边界名称>
## 1. 来源北极星版本
## 2. 主体职责
## 3. 核心能力范围
## 4. 输入
## 5. 输出
## 6. 状态
## 7. 失败场景
## 8. 权限与安全
## 9. 与前端边界关系
## 10. 非目标
## 11. 禁止事项
## 12. 验收标准
## 13. 决策记录
## 14. 变更记录
```

### 9.4 跨端归属表模板

`cross-boundary-ownership.template.md` 必须包含：

```text
# 跨端归属表
## 1. 来源北极星版本
## 2. 归属原则
## 3. 主归属表
## 4. 镜像边界表
## 5. 冲突边界
## 6. 待确认项
## 7. 决策记录
## 8. 变更记录
```

### 9.5 架构规范模板

`architecture-spec.template.md` 必须包含：

```text
# <系统名称>_开发规范及架构设计文档
## 1. 来源文档
## 2. 功能目标
## 3. 非目标
## 4. 代码目录设计

必须声明软件代码根目录为 `project/`，并以 `project/` 下的相对路径描述源码、包管理文件、构建脚本、测试代码、静态资源和运行配置。

## 5. 模块职责
## 6. 数据结构设计
## 7. 接口设计
## 8. 状态流设计
## 9. 详细伪代码设计
## 10. 错误处理策略
## 11. 日志与调试策略
## 12. 测试策略
## 13. 与其他系统的依赖关系
## 14. 禁止事项
## 15. MVP 实现范围
## 16. 后续扩展点
## 17. 验收标准
## 18. 决策记录
## 19. 变更记录
```

### 9.6 TODO 模板

`todo.template.md` 必须包含：

```text
# TODO
## 1. 说明
## 2. 状态枚举
## 3. 当前阶段必做
## 4. 后续扩展
## 5. 待确认
## 6. 状态变更记录
```

每个 TODO 最小字段：

```text
- 状态
- 任务名
- 所属阶段
- plan 链接
- 依赖项
- 验收标准摘要
```

状态枚举固定为：

```text
not_started
planning
in_progress
blocked
done
```

必须声明：

```text
复选框只作为可读状态辅助，真实状态以状态字段为准。
```

### 9.7 TODO plan 模板

`todo-plan-not-started.template.md` 必须包含：

```text
# <TODO 名称>
## 当前状态
## 概述性目标
## 正式开发前必须重新生成详细 plan
## 生成详细 plan 时必须参考
## 禁止事项
```

其中“生成详细 plan 时必须参考”必须列出：

```text
- project/ 内的当前代码进度
- 北极星文档
- 系统边界文档
- 架构规范文档
- 已完成 TODO 的实现结果
```

`todo-plan-not-started.template.md` 禁止包含：

```text
1. 详细开发步骤。
2. 文件影响范围。
3. 伪装成可直接执行的实现计划。
```

`todo-plan-in-progress.template.md` 必须包含：

```text
# <TODO 名称>
## 当前状态
## 当前代码状态分析
## 输入依据
## 详细开发步骤
## 文件影响范围

软件代码路径必须以 `project/` 开头；文档路径可以指向 `docs/`、`TODO.md` 或决策记录，但不得混同为软件代码路径。

## 验收标准
## 禁止事项
## 回写要求
## 完成记录
```

### 9.8 project-artifacts-structure.md 必须包含

`project-artifacts-structure.md` 不是“待填写模板”。它必须使用以下结构：

```text
# Project Artifacts Structure
## 1. 用途
## 2. 推荐项目产物结构
## 3. Builder 阶段禁止事项
## 4. VELA 运行阶段创建规则
## 5. 链接规则
```

“推荐项目产物结构”必须包含：

```text
project/
docs/
├── north-star/
├── frontend-boundaries/
├── backend-boundaries/
├── cross-boundary/
├── architecture-specs/
├── decisions/
└── todo-plans/
TODO.md
```

并必须说明：

```text
1. Builder 阶段不得创建这些目录。
2. 使用 VELA 阶段根据需要创建这些目录。
3. project/ 是最终生成软件代码的唯一根目录。
4. docs/ 与 TODO.md 是项目文档产物，不属于软件代码根目录。
5. 北极星 v0.1.0/v0.2.0 必须通过相对链接引用子文档。
6. TODO.md 的 plan 链接默认指向 docs/todo-plans/。
7. TODO plan 中的软件代码文件影响范围必须使用 project/ 下的路径。
```

---

## 10. Schema 文件严格蓝图

`schemas/` 中每个文件都必须包含：

```text
# <Schema 名称>
## 1. 适用对象
## 2. 必填字段
## 3. 字段顺序
## 4. 禁止字段
## 5. 校验规则
```

每个 schema 文件必须至少包含 5 个二级标题。  
每个 schema 文件必须明确字段顺序。  
schema 不得写成一句话说明。

### 10.1 schema 统一原则

```text
1. schema 高于 template。
2. template 必须符合 schema。
3. schema 不描述具体项目内容，只描述结构约束。
4. schema 中禁止出现项目名称、技术栈、具体功能名。
```

### 10.2 必须覆盖的结构

```text
1. harness manifest，且必须包含 runtime_project_policy.code_root = project
2. 北极星文档
3. 系统边界文档
4. 架构规范文档
5. 系统边界索引表
6. 架构规范索引表
7. TODO.md
8. TODO plan
9. 项目状态文件
```

### 10.3 north-star.schema.md 必须约束

```text
项目目标、目标用户、核心使用场景、MVP 边界、非目标、约束、核心功能方向、待确认内容、决策记录、变更记录。
v0.1.0 必须增加系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接。
v0.2.0 必须增加架构规范索引表、架构规范文档链接、TODO 生成依据说明。
```

### 10.4 system-boundary.schema.md 必须分三类约束

`system-boundary.schema.md` 不得只写一套混合字段。  
它必须包含三个子结构：

```text
### 前端边界文档字段
- 来源北极星版本
- 主体职责
- 用户可见范围
- 用户操作路径
- 页面/界面/命令呈现
- 状态呈现
- 错误反馈
- 与后端边界关系
- 非目标
- 禁止事项
- 验收标准
- 决策记录
- 变更记录

### 后端边界文档字段
- 来源北极星版本
- 主体职责
- 核心能力范围
- 输入
- 输出
- 状态
- 失败场景
- 权限与安全
- 与前端边界关系
- 非目标
- 禁止事项
- 验收标准
- 决策记录
- 变更记录

### 跨端归属表字段
- 来源北极星版本
- 归属原则
- 主归属表
- 镜像边界表
- 冲突边界
- 待确认项
- 决策记录
- 变更记录
```

### 10.5 architecture-spec.schema.md 必须约束

```text
来源文档、功能目标、非目标、目录设计、模块职责、数据结构、接口设计、状态流、伪代码、错误处理、日志与调试、测试策略、依赖关系、禁止事项、MVP 范围、扩展点、验收标准、决策记录、变更记录。
目录设计中的软件代码路径必须以 project/ 作为根目录。
```

### 10.6 todo.schema.md 必须约束

```text
状态、任务名、所属阶段、plan 链接、依赖项、验收标准摘要。
状态枚举固定为 not_started / planning / in_progress / blocked / done。
复选框不得替代状态字段。
```

### 10.7 todo-plan.schema.md 必须区分两个变体

`todo-plan.schema.md` 不得用同一套必填字段同时约束未开始 plan 和进行中 plan。

#### 未开始 plan 必填字段

```text
- 当前状态
- 概述性目标
- 正式开发前必须重新生成详细 plan
- 生成详细 plan 时必须参考
- 禁止事项
```

未开始 plan 禁止字段：

```text
- 详细开发步骤
- 文件影响范围
- 完成记录
- 伪装成可直接执行的实现计划
```

#### 进行中 plan 必填字段

```text
- 当前状态
- 当前代码状态分析
- 输入依据
- 详细开发步骤
- 文件影响范围
- 验收标准
- 禁止事项
- 回写要求
- 完成记录

进行中 plan 的文件影响范围中，软件代码路径必须以 project/ 开头。
```

---

## 11. Checklist 文件严格蓝图

`checklists/` 中每个文件都必须包含：

```text
# <Checklist 名称>
## 1. 使用场景
## 2. 检查项
## 3. 失败处理
```

每个 checklist 文件必须至少包含 3 个二级标题。  
“检查项”不得少于 5 条，除非该 checklist 的适用范围天然小于 5 项。  
“失败处理”必须说明停止、修复、回退或重新检查策略。

### 11.1 builder-output-checklist.md 必须检查

```text
1. workspace 根目录是否存在 AGENTS.md。
2. workspace 根目录是否存在 CLAUDE.md。
3. 运行体目录是否为 .vela/。
4. .vela/harness.yaml 是否存在。
5. .vela/harness.yaml required_files 是否使用 workspace 根目录相对路径完整列出所有 59 个文件，而不只是 .vela 内部文件。
6. .vela/harness.yaml 中 required_files 是否全部存在。
7. .vela/harness.yaml 中 required_directories 是否全部存在。
8. 是否存在未授权文件。
9. 是否错误生成 .vela/AGENTS.md 或 .vela/CLAUDE.md。
10. 是否存在项目名称、技术栈、具体功能名。
11. 是否存在示例项目文档。
12. 是否提前生成项目产物。
13. Builder 阶段是否错误创建 project/。
14. .vela/harness.yaml 是否包含 runtime_project_policy.code_root = project。
15. .vela/SKILL.md 是否声明 VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内。
16. rules/stages/07-development-execution.md 是否包含 project/ 软件代码根目录强制规则。
17. todo-execution-protocol.md 是否包含 TODO plan 软件代码路径必须以 project/ 开头的规则。
18. project-artifacts-structure.md 是否说明 project/ 是最终生成软件代码的唯一根目录。
19. 是否存在 placeholder-only 规则文件。
20. 非模板文件是否包含占位性正文；该 checklist 必须在本文件内部直接写明精确检测规则，不得引用 Builder Spec 的章节号。
21. .vela/SKILL.md 是否为核心运行入口。
22. AGENTS.md、CLAUDE.md 是否只作为 workspace 根默认发现入口并转发到 .vela/SKILL.md。
23. .vela/README.md 是否只作为人类说明。
24. schema 是否高于 template。
25. todo-execution-protocol.md 是否包含复选框辅助规则。
26. todo-execution-protocol.md 是否包含依赖 TODO 未完成时必须补齐的规则。
27. todo-execution-protocol.md 是否包含 TODO 完成后必须验收闭环的规则。
28. version-progression-protocol.md 是否明确列出 v0.1.0 与 v0.2.0 的必备内容。
29. version-progression-protocol.md 是否包含 TODO.md 不通过文件名版本化但保留状态变更记录。
30. system-boundary.schema.md 是否区分前端、后端、跨端三类字段。
31. todo-plan.schema.md 是否区分未开始 plan 与进行中 plan。
32. project-artifacts-structure.md 是否包含 5 个二级标题和推荐项目产物结构。
33. rules/stages/04-system-boundary-generation.md 是否包含 v0.1.0 必备链接完整短语。
34. rules/stages/05-architecture-spec-generation.md 是否包含 v0.2.0 必备链接完整短语。
35. rules/stages/06-todo-track-generation.md 是否包含 TODO.md 不得脱离架构规范文档生成。
36. rules/stages/07-development-execution.md 是否包含跨系统 TODO 标注规则。
37. rules/stages/07-development-execution.md 是否包含 TODO 验收闭环规则。
38. question-protocol.md 是否包含“全部使用推荐选项”、项目外常识候选项限制、边界完整停止条件。
39. 占位性正文检测是否没有误判“策略”“省略”“忽略”等合法词。
40. 检查清单是否没有引用 Builder Spec 章节号，例如“遵守 0.6”。
41. 关键短语是否原样出现，没有被反引号、同义词、换行或标点拆分。
42. 自检失败时是否已停止或修复后重检。
```
### 11.2 runtime-start-checklist.md 必须检查

```text
1. 用户是否提供北极星文档 v0.0.1。
2. 是否误把当前项目代码当作入口。
3. v0.0.1 是否具备入口最低字段。
4. 不完整时是否进入选择题协议。
5. 是否生成项目状态文件。
```

### 11.3 stage-completion-checklist.md 必须检查

```text
1. 阶段进入条件是否满足。
2. 阶段输出是否完成。
3. 待确认项是否处理。
4. 决策记录是否更新。
5. 状态文件是否更新。
```

### 11.4 version-progression-checklist.md 必须检查

```text
1. v0.1.0 是否只在系统边界完成后生成。
2. v0.1.0 是否包含系统边界索引表、前端边界链接、后端边界链接、跨端归属表链接。
3. v0.2.0 是否只在架构规范完成后生成。
4. v0.2.0 是否包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明。
5. 子文档链接是否为相对路径。
6. 变更记录是否更新。
```

### 11.5 todo-readiness-checklist.md 必须检查

```text
1. v0.2.0 是否完成。
2. 全部架构规范是否完成。
3. TODO 是否有独立 plan。
4. TODO 依赖是否明确。
5. TODO 验收摘要是否明确。
6. TODO 状态字段是否存在。
7. 是否没有用复选框替代状态字段。
8. TODO plan 的软件代码文件影响范围是否均位于 project/ 下。
```

### 11.6 todo-completion-checklist.md 必须检查

```text
1. 当前 TODO 是否已完成。
2. 验收标准是否满足。
3. TODO.md 状态是否更新。
4. plan 完成记录是否更新。
5. 文档影响说明是否更新。
6. 是否依据 plan 和相关规范完成验收闭环。
7. 新增或修改的软件代码是否全部位于 project/ 下。
8. 是否没有在 workspace 根目录直接生成软件代码文件。
```

### 11.7 conflict-resolution-checklist.md 必须检查

```text
1. 冲突项是否明确报告。
2. 冲突类型是否分类。
3. 是否通过选择题让用户选择修复路径。
4. 修复后是否更新受影响文档。
5. 是否重新检查阶段状态。
```

---

## 12. Extensions 目录规则

`extensions/README.md` 必须包含：

```text
# VELA Extensions
## 1. 用途
## 2. 可在此处加入什么
## 3. 不得在此处加入什么
## 4. 项目特定内容使用阶段
```

必须声明：

```text
1. extensions/ 是固定扩展槽位。
2. 默认不得包含项目特定内容。
3. 项目名称、技术栈、具体功能名只能在使用 VELA 阶段写入。
4. Builder 阶段不得填充 extensions/。
5. extensions/ 中的内容不得覆盖 .vela/SKILL.md、.vela/rules/、.vela/schemas/、.vela/templates/ 的核心规则。
```

---

## 13. VELA 运行阶段核心规则摘要

生成 VELA Harness 时，agent 必须将以下规则写入对应文件。

### 13.1 入口规则

```text
用户使用 VELA 时，唯一合法入口是用户提供的北极星文档 v0.0.1。
VELA 不负责凭空创造项目。
如果 v0.0.1 不完整，只能通过选择题补齐。
```

### 13.2 前端/后端规则

```text
前端 = 用户接触层 / 表现层 / 操作层。
后端 = 核心能力层 / 数据层 / 执行层。
系统边界层必须先按前端和后端分类。
跨端系统必须采用主归属文档 + 镜像边界。
```

### 13.3 版本推进规则

```text
v0.0.1 = 用户提供入口。
v0.1.0 = 系统边界层完成。
v0.2.0 = 架构规范层完成。
TODO.md 只能在 v0.2.0 与全部架构规范完成后生成。
```

### 13.4 TODO 规则

```text
TODO.md 是项目开发唯一执行轨道。
每个 TODO 必须有独立 plan。
未开始 plan 只写概述。
进行中 plan 必须基于 project/ 内的当前代码状态重新生成。
默认一次只推进一个当前 TODO。
依赖 TODO 未完成时，必须补齐后再继续。
复选框只作为可读状态辅助，真实状态以状态字段为准。
每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环。
软件代码路径必须位于 project/ 下。
```

### 13.5 代码输出目录规则

```text
Builder 阶段不得创建 project/。
VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内。
docs/、TODO.md、决策记录、边界文档和架构规范文档属于项目文档产物，不属于软件代码根目录。
不得在 workspace 根目录直接生成 main、src、package、Cargo、配置、测试或构建类软件代码文件。
```

### 13.6 冲突规则

```text
代码、文档、TODO、规范冲突时，必须停止。
agent 必须报告冲突项。
agent 必须通过选择题让用户选择修复路径。
不得自行静默修复边界。
```

---

## 14. 优先级规则

生成 VELA Harness 时，必须写入以下优先级规则：

```text
1. .vela/harness.yaml 是结构和文件清单最高基准。
2. .vela/SKILL.md 是 VELA 运行入口最高基准。
3. .vela/rules/stages/ 是阶段执行规则来源。
4. .vela/rules/protocols/ 是跨阶段协议规则来源。
5. .vela/rules/quality/ 是文档质量验收来源。
6. .vela/schemas/ 高于 .vela/templates/。
7. .vela/templates/ 只表达格式，不覆盖规则。
8. runtime_project_policy.code_root 固定为 project，是 VELA 使用阶段软件代码输出根目录规则来源。
9. AGENTS.md 和 CLAUDE.md 位于 workspace 根目录，只做默认发现入口，不覆盖 .vela/SKILL.md。
10. .vela/README.md 只做解释，不覆盖核心规则。
11. .vela/extensions/ 默认不含项目特定内容，且不得覆盖核心规则。
```

---

## 15. 自检协议

生成 VELA Harness 后，agent 必须执行自检。

### 15.1 结构自检

检查：

```text
1. workspace 根目录是否存在 AGENTS.md。
2. workspace 根目录是否存在 CLAUDE.md。
3. 运行体目录名是否为 .vela/。
4. .vela/harness.yaml 是否存在。
5. .vela/harness.yaml required_files 是否使用 workspace 根目录相对路径完整列出所有 59 个文件。
6. required_files 中的每个文件是否存在。
7. required_directories 中的每个目录是否存在。
8. 文件命名是否与本规范一致。
9. 是否存在未授权文件。
10. 是否错误生成 .vela/AGENTS.md 或 .vela/CLAUDE.md。
11. .vela/harness.yaml 是否与实际文件树一致。
12. Builder 阶段是否没有创建 project/。
13. .vela/harness.yaml 是否包含 runtime_project_policy.code_root = project。
```

### 15.2 内容职责自检

检查：

```text
1. .vela/SKILL.md 是否只作为核心运行入口。
2. AGENTS.md 是否只作为 Codex workspace 根默认发现入口。
3. CLAUDE.md 是否只作为 Claude Code workspace 根默认发现入口。
4. .vela/README.md 是否只作为人类说明。
5. .vela/rules/stages/ 是否只存放阶段规则。
6. .vela/rules/protocols/ 是否只存放复用协议。
7. .vela/rules/quality/ 是否只存放质量标准。
8. .vela/templates/ 是否只存放模板。
9. .vela/schemas/ 是否只存放结构约束。
10. .vela/checklists/ 是否只存放检查清单。
11. .vela/extensions/ 是否默认不包含项目特定内容。
```

### 15.3 禁止事项自检

检查是否出现：

```text
1. 具体项目名称。
2. 具体技术栈。
3. 具体功能名。
4. 示例项目文档。
5. 自由扩展文件。
6. 提前生成的项目产物。
7. TODO.md。
8. 北极星文档实例。
9. 系统边界文档实例。
10. 架构规范文档实例。
11. 英文-only 正文文件，AGENTS.md 与 CLAUDE.md 的适配句除外。
12. .vela/AGENTS.md。
13. .vela/CLAUDE.md。
```

### 15.4 引用关系自检

检查：

```text
1. AGENTS.md 是否引用 .vela/SKILL.md。
2. CLAUDE.md 是否引用 .vela/SKILL.md。
3. .vela/README.md 是否声明 .vela/SKILL.md 是 agent 入口。
4. .vela/SKILL.md 是否引用 .vela/rules/stages/。
5. .vela/SKILL.md 是否引用 .vela/rules/protocols/。
6. .vela/SKILL.md 是否引用 .vela/rules/quality/。
7. 阶段规则是否引用相关协议。
8. 模板是否符合 schema。
9. checklist 是否覆盖 Builder 输出与运行阶段。
10. workspace 根入口是否没有复制完整规则正文而只委托 .vela/SKILL.md。
```

### 15.5 内容充分性自检

检查：

```text
1. 非模板文件是否包含占位性正文；检测规则必须自包含写明，不得引用 Builder Spec 的章节号。
2. 非模板文件是否只有标题没有规则。
3. 阶段文件是否包含 8 个二级标题。
4. 协议文件是否包含 5 个二级标题。
5. 质量文件是否包含 4 个二级标题。
6. schema 文件是否包含 5 个二级标题。
7. checklist 文件是否包含 3 个二级标题。
8. .vela/harness.yaml 是否完整列出所有 59 个文件。
9. project-artifacts-structure.md 是否包含 5 个二级标题。
10. 非模板文件是否可直接指导 agent 行为。
11. 自检是否没有因“策略”“省略”“忽略”等合法词误判失败。
12. `.vela/` 内部文件与 workspace 根入口文件是否没有引用 Builder Spec 章节号。
13. 关键短语是否以普通文本原样出现。
14. 代码输出目录规则是否在 SKILL、S7、TODO 协议、项目产物结构、schema 和 checklist 中同步出现。
```

### 15.6 关键短语自检

agent 必须检查以下短语是否出现在对应文件中。缺失即自检失败。

关键短语复制规则：

```text
1. 下表中的关键短语必须原样出现在目标文件正文中。
2. 不得用同义词替换关键短语，例如不得把“和”改成“与”，也不得把“必须”改成“应当”。
3. 不得在关键短语内部插入 Markdown 反引号、粗体、斜体或换行。
4. 目标文件可以在关键短语之外补充说明，但不得破坏关键短语本身。
5. checklist 中用于满足关键短语自检的句子必须以普通文本出现，不得只作为代码块、注释或示例出现。
```

| 文件 | 必须出现的关键短语 |
|---|---|
| `.vela/rules/protocols/todo-execution-protocol.md` | `复选框只作为可读状态辅助，真实状态以状态字段为准` |
| `.vela/rules/protocols/todo-execution-protocol.md` | `依赖或上一阶段 TODO 未完成时必须补齐` |
| `.vela/rules/protocols/todo-execution-protocol.md` | `每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环` |
| `.vela/rules/protocols/version-progression-protocol.md` | `v0.1.0 必须包含系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接` |
| `.vela/rules/protocols/version-progression-protocol.md` | `v0.2.0 必须包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明` |
| `.vela/schemas/system-boundary.schema.md` | `前端边界文档字段` |
| `.vela/schemas/system-boundary.schema.md` | `后端边界文档字段` |
| `.vela/schemas/system-boundary.schema.md` | `跨端归属表字段` |
| `.vela/schemas/todo-plan.schema.md` | `未开始 plan 必填字段` |
| `.vela/schemas/todo-plan.schema.md` | `进行中 plan 必填字段` |
| `.vela/templates/project-artifacts-structure.md` | `## 2. 推荐项目产物结构` |
| `.vela/checklists/builder-output-checklist.md` | `.vela/harness.yaml required_files 是否使用 workspace 根目录相对路径完整列出所有 59 个文件` |
| `.vela/rules/stages/04-system-boundary-generation.md` | `v0.1.0 必须包含系统边界索引表、前端边界文档链接、后端边界文档链接、跨端归属表链接` |
| `.vela/rules/stages/05-architecture-spec-generation.md` | `v0.2.0 必须包含架构规范索引表、所有系统边界文档链接、所有架构规范文档链接、TODO 生成依据说明` |
| `.vela/rules/stages/06-todo-track-generation.md` | `TODO.md 不得脱离架构规范文档生成` |
| `.vela/rules/stages/07-development-execution.md` | `跨系统 TODO 必须标注涉及的系统边界文档与架构规范文档` |
| `.vela/rules/stages/07-development-execution.md` | `每个 TODO 完成后必须依据 plan 与相关规范进行验收闭环` |
| `.vela/rules/stages/07-development-execution.md` | `VELA 使用阶段生成或修改的软件代码必须位于 workspace 根目录的 project/ 内` |
| `.vela/rules/protocols/todo-execution-protocol.md` | `TODO plan 的文件影响范围中，软件代码路径必须以 project/ 开头` |
| `.vela/templates/project-artifacts-structure.md` | `project/ 是最终生成软件代码的唯一根目录` |
| `.vela/rules/protocols/version-progression-protocol.md` | `TODO.md 不通过文件名版本化，但必须保留状态变更记录` |
| `.vela/rules/protocols/question-protocol.md` | `允许用户回复“全部使用推荐选项”` |
| `.vela/rules/protocols/question-protocol.md` | `可以引入项目外常识补充候选项，但不能覆盖北极星文档` |
| `.vela/rules/protocols/question-protocol.md` | `达到边界完整条件后停止` |
| `AGENTS.md` | `Codex 必须把 .vela/SKILL.md 视为 VELA Harness 的唯一核心入口` |
| `CLAUDE.md` | `Claude Code 必须把 .vela/SKILL.md 视为 VELA Harness 的唯一核心入口` |

### 15.7 外部依赖表达自检

agent 必须检查 `.vela/` 内部文件与 workspace 根入口文件是否出现以下外部依赖表达。出现任一项即自检失败：

```text
1. 遵守 0.6
2. 见 0.6
3. 参见 0.6
4. 见 Builder Spec
5. 遵守 Builder Spec 第
6. 按上文
7. 如前所述
8. 见前文
9. 根据上一节
```

允许出现的表达：

```text
1. .vela/README.md 可以说明 `.vela/` 由 Builder Spec 生成。
2. .vela/checklists/builder-output-checklist.md 可以说明检查 Builder 输出结果。
3. 所有运行规则必须在 `.vela/` 内部自包含或引用 `.vela/` 内部相对路径。
4. AGENTS.md 与 CLAUDE.md 可以说明自身必须读取 `.vela/SKILL.md`。
```

### 15.8 自检失败处理

```text
1. 可修复问题：自动修复，然后重新自检。
2. 不可修复问题：停止，报告缺失项。
3. 不得在自检失败时报告成功。
4. 不得通过删除检查项来制造通过。
5. 不得通过改写关键短语自检表来制造通过。
```

### 15.9 自检报告

自检报告只在 agent 最终回复中展示，不写入 `.vela/`，避免污染固定结构。

---

## 16. 最终回复格式

生成 VELA Harness 后，agent 最终回复必须包含：

```text
1. 生成结果：成功 / 失败
2. 输出内容：AGENTS.md、CLAUDE.md、.vela/
3. 文件数量：应为 59 个文件
4. 自检结果：通过 / 未通过
5. 根入口检查：AGENTS.md 与 CLAUDE.md 是否位于 workspace 根目录
6. 运行体检查：.vela/SKILL.md 是否为唯一核心运行入口
7. 修复情况：如有
8. 失败项：如有
9. 后续使用方式
```

不得输出全部文件正文，除非用户明确要求。

不得继续进入项目开发阶段。

---

## 17. 失败处理规则

如果 agent 无法完整生成 AGENTS.md、CLAUDE.md 与 `.vela/`，必须：

```text
1. 停止。
2. 报告无法生成的文件或目录。
3. 报告失败原因。
4. 不得生成部分结果后声称成功。
5. 不得进入项目开发。
```

---

## 18. 维护规则

```text
1. VELA_HARNESS_BUILDER.md 是便携构建源。
2. AGENTS.md 与 CLAUDE.md 是 workspace 根默认发现入口。
3. .vela/ 是运行体。
4. .vela/ 生成后可以独立升级。
5. .vela/ 的升级必须记录在 .vela/CHANGELOG.md。
6. 如果 Builder Spec 与已生成 AGENTS.md、CLAUDE.md 或 .vela/ 出现差异，重新生成时以 Builder Spec 为准。
7. 使用时用户可以只复制 AGENTS.md、CLAUDE.md 与 .vela/ 到目标项目 workspace；无需复制 Builder 源文件和 Builder 包说明 README。
```

---

## 19. 最小调用语义

用户可以用类似指令调用：

```text
根据 VELA_HARNESS_BUILDER.md 构建 harness。
```

agent 必须理解为：

```text
读取 VELA_HARNESS_BUILDER.md
↓
生成 workspace 根入口 AGENTS.md、CLAUDE.md
↓
生成 .vela/ 运行体
↓
执行结构自检、内容职责自检、禁止事项自检、引用关系自检、内容充分性自检、关键短语自检、外部依赖表达自检
↓
报告结果
```

而不是：

```text
生成项目文档
写代码
进入开发
```
