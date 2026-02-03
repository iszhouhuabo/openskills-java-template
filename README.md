# AI 技能库与治理规范 (AI Skills & Governance)

本项目是团队的代码规范、架构原则和业务知识的 **Single Source of Truth (唯一真理源)**。
它不仅仅是给人看的文档，更是 **AI Agent (如 Gemini, Claude, Cursor)** 可直接读取和执行的指令集。

通过将隐性的“团队惯例”显性化为结构化的 Markdown/XML，确保 AI 辅助生成的代码严格符合团队标准。

---

## 📂 目录结构全景图

```text
ai-skills/
├── AGENT.md                 # [核心] AI 角色设定 (Persona)，定义了"你是谁"
├── RULE.md                  # [核心] 全局红线规则 (Redlines)，所有 Skill 都必须遵守的底线
├── README.md                # 本文档
└── skills/                  # 具体技能库
    ├── architecture/        # 架构级规则
    │   └── layer-rules/     # 分层架构规范 (Controller/Service/Mapper 边界)
    ├── business/            # 特定业务领域的知识
    │   └── payment-logic/   # 支付与金额计算逻辑
    ├── global/              # 全局通用规范
    │   └── java-style/      # Java 编码风格 (命名、注释、Lombok)
    ├── tech-stack/          # 具体技术栈的使用规范
    │   ├── api-contract/    # Web 接口契约 (DTO/VO, 异常处理)
    │   ├── hutool-utils/    # 工具类库规范 (优先使用 Hutool)
    │   ├── mybatis-plus/    # ORM 框架规范 (LambdaWrapper, SQL安全)
    │   └── spring-boot/     # 框架基础规范 (DI, JSON配置)
    └── workflow/            # 标准作业程序 (SOP)
        └── feature-dev/     # 功能开发标准工作流
```

---

## 📖 模块详细说明

### 1. 核心文件 (Root)
*   **`AGENT.md`**: 定义了 AI 的“人设”。例如：“你是一个资深 Java 架构师，精通 Spring Boot”。它还规定了 AI 的沟通风格（中文沟通，英文代码）和最高指令（Prime Directive）。
*   **`RULE.md`**: 定义了 **Blocker 级别** 的全局规则。无论 AI 当前在执行什么任务，都严禁违反这些规则。例如：禁止硬编码密钥、禁止 SQL 注入、禁止循环查库。

### 2. 架构规范 (`skills/architecture/`)
定义系统的宏观结构。
*   **`layer-rules`**: 规定了代码的分层方式。例如：本项目采用 **Service 无接口模式**（直接实现类），严禁 Controller 直接调 Mapper。

### 3. 技术栈规范 (`skills/tech-stack/`)
针对特定框架或库的最佳实践。
*   **`spring-boot`**: 规定了依赖注入必须用构造器注入（`@RequiredArgsConstructor`），严禁 `@Autowired` 字段注入。
*   **`mybatis-plus`**: 强制使用 `LambdaQueryWrapper` 以避免字段名硬编码，规定了逻辑删除的处理方式。
*   **`hutool-utils`**: 强制优先使用 Hutool 工具类，禁止引入 Apache Commons 或手写工具类。
*   **`api-contract`**: 定义了前后端交互标准，包括统一响应结构 `HttpResult<T>` 和全局异常处理机制。

### 4. 业务领域知识 (`skills/business/`)
沉淀团队特有的业务逻辑，防止 AI 产生幻觉或使用通用但错误的逻辑。
*   **`payment-logic`**: 这里的规则非常具体，例如：“金额必须以 **分(Cent)** 为单位存储”、“税率计算公式”、“禁止使用 Double 计算金额”。

### 5. 全局规范 (`skills/global/`)
跨技术栈的通用编码标准。
*   **`java-style`**: 命名规范（驼峰/蛇形）、Lombok 的使用要求、以及 AI 生成代码时的注释标记规范。

### 6. 工作流 (`skills/workflow/`)
指导 AI 如何一步步完成复杂的任务。
*   **`feature-dev`**: 定义了从“数据库设计”到“自检”的完整步骤，确保 AI 不会写完代码就跑，而是会检查质量。

---

## 🏷️ 标签系统使用指南

为了让 LLM (大语言模型) 更精准地理解规则，我们采用了 **XML 风格的语义化标签**。
这些标签不是标准的 HTML，而是为了给 AI 提供上下文结构的 Prompt Engineering 技巧。

### 常用标签结构

#### 1. 规则集合 `<rules>` / `<constraints>`
用于包裹一组相关的规则。
*   `type`: 可选属性，说明规则类型（如 `mandatory` 强制, `suggested` 建议）。

```xml
<framework_rules>
    <rule type="mandatory">
        <!-- 规则内容 -->
    </rule>
</framework_rules>
```

#### 2. 具体规则 `<rule>`
最小的规则单元。描述要简练、直接。
*   **正向引导**：说明“应该做什么”。
*   **反向禁止**：说明“严禁做什么”。

```xml
<rule id="SEC-01" level="blocker">
    **禁止硬编码密钥**。必须使用配置中心。
</rule>
```

#### 3. 步骤 `<step>` (用于 Workflow)
定义顺序执行的步骤。
*   `sequence`: 步骤序号。

```xml
<step sequence="1" name="Database Design">
    设计表结构，提供 DDL。
</step>
```

#### 4. 示例 `<example>` / `<example_correct>` / `<example_wrong>`
**Few-Shot Prompting** 的核心。通过正反例对比，让 AI 瞬间明白什么是对的，什么是错的。

```xml
<example_correct>
    // Good: 使用 Lambda 表达式
    lambdaQuery().eq(User::getName, "Alice");
</example_correct>

<example_wrong>
    // Bad: 硬编码字符串
    query().eq("name", "Alice");
</example_wrong>
```

### 编写建议

1.  **语义化标签名**：标签名本身就是 Prompt 的一部分。使用 `<currency_handling>` 比使用 `<rule1>` 效果好得多。
2.  **英文优先**：虽然本文档是中文的，但为了 Token 效率和模型理解力，建议在 XML 标签名和属性中使用英文（如 `content` 尽量保持简练的英文或中英文混合，关键术语用英文）。
3.  **层级分明**：保持缩进，体现逻辑归属关系。

---



## 📋 标签定义参考表 (Tag Reference)



为了方便维护，我们将标签分为 **通用核心**、**特定文件专用** 和 **业务辅助** 三类。



### 1. 🌟 通用核心标签 (Universal Core)

这组标签可以在任何 `.md` 文件中使用，是构建规则的基础。


| 标签 (Tag)            | 作用说明 (Description)                                  | 属性 (Attributes)                                                                                                               | 适用场景         |
| ------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------ |
| `<rule>`            | **规则本体**。定义一条明确的准则。                                 | `id` (可选): 规则唯一标识 (如 SEC-01)<br>`level` (可选): 严重等级 (blocker, critical, major)<br>`type` (可选): 类型 (mandatory 强制, suggested 建议) | 所有规则文件       |
| `<example_correct>` | **正确示例 (Positive Case)**。展示符合规范的代码片段，强化 AI 的模仿能力。   | -                                                                                                                             | 任何具体的 Rule 下 |
| `<example_wrong>`   | **错误示例 (Negative Case)**。展示严禁使用的反模式 (Anti-pattern)。 | -                                                                                                                             | 任何具体的 Rule 下 |
| `<code_snippet>`    | **标准代码片段**。提供一段可直接复用的标准实现。                          | `lang` (可选): 语言 (java, xml)                                                                                                   | 任何技术/业务文档    |




### 2. 🤖 角色与红线专用 (Persona & Critical Rules)

仅用于 `AGENT.md` 和 `RULE.md`。


| 标签 (Tag)            | 作用说明 (Description)              | 属性 (Attributes) | 文件位置       |
| ------------------- | ------------------------------- | --------------- | ---------- |
| `<system_persona>`  | **角色容器**。包裹 AI 的人设、最高指令和交互风格。   | -               | `AGENT.md` |
| `<role>`            | **身份定义**。定义 AI 是谁 (如 Java 架构师)。 | -               | `AGENT.md` |
| `<prime_directive>` | **最高指令**。定义不可违背的元规则。            | -               | `AGENT.md` |
| `<critical_rules>`  | **红线容器**。包裹所有 Block 级别的全局规则。    | -               | `RULE.md`  |
| `<security>`        | **安全分组**。专门用于安全相关的红线规则。         | -               | `RULE.md`  |


### 3. 🔄 工作流专用 (Workflow)

仅用于 `skills/workflow/` 下的文件。

| 标签 (Tag)           | 作用说明 (Description)      | 属性 (Attributes)                                                | 文件位置              |
| ------------------ | ----------------------- | -------------------------------------------------------------- | ----------------- |
| `<workflow_steps>` | **SOP 容器**。包裹整个工作流步骤。   | -                                                              | `feature-dev/...` |
| `<step>`           | **步骤节点**。定义工作流中的一个特定阶段。 | `sequence` (必填): 执行顺序 (1, 2, 3...)<br>`name` (必填): 步骤名称 (简短英文) | `feature-dev/...` |


### 4. 🏗️ 架构与技术栈 (Architecture & Tech)

用于 `skills/architecture/` 和 `skills/tech-stack/`。


| 标签 (Tag)                     | 作用说明 (Description)                   | 属性 (Attributes)                                                                         | 文件位置             |
| ---------------------------- | ------------------------------------ | --------------------------------------------------------------------------------------- | ---------------- |
| `<architecture_constraints>` | **架构约束容器**。定义分层边界。                   | -                                                                                       | `layer-rules`    |
| `<layer>`                    | **分层定义**。描述某一层 (如 Web, Service) 的职责。 | `name` (必填): 层级名称<br>`responsibility`: 职责描述<br>`forbidden`: 禁止事项<br>`dependency`: 允许的依赖 | `layer-rules`    |
| `<library_rules>`            | **库规范容器**。定义第三方库的使用规则。               | -                                                                                       | `hutool-utils` 等 |
| `<api_rules>`                | **接口规范容器**。定义 Web 接口契约。              | -                                                                                       | `api-contract`   |
| `<mapping>`                  | **映射表容器**。用于批量列举对应关系。                | -                                                                                       | 各种工具类映射          |
| `<item>`                     | **映射项**。表格中的一行。                      | `func`: 功能描述<br>`class`: 对应类名<br>`example`: 使用示例                                        | `<mapping>` 内部   |



### 5. 💰 业务领域 (Business Logic)

用于 `skills/business/`。


| 标签 (Tag)           | 作用说明 (Description)    | 属性 (Attributes)                        | 文件位置           |
| ------------------ | --------------------- | -------------------------------------- | -------------- |
| `<business_logic>` | **业务逻辑容器**。包裹特定领域的知识。 | `domain` (必填): 领域名称 (如 payment, order) | `business/...` |
| `<formula>`        | **计算公式**。描述数学或逻辑计算规则。 | -                                      | 任何涉及计算的文档      |

