对于 React Web 前端开发，我建议你把 AI 定位为一个能够理解项目、参与设计、辅助编码、执行测试和维护文档的工程协作者，而不是简单的代码生成工具。

真正高效地使用 AI，核心不是写出多么复杂的 Prompt，而是建立一套稳定的 AI 辅助开发工作流（AI-Assisted Development Workflow）。

尤其是 React + TypeScript 项目，AI 可以承担很多工作：

- 需求分析：将模糊需求转化为技术方案。
- 架构设计：组件拆分、Hooks 设计、状态管理。
- 编码实现：页面、组件、API、工具函数。
- 代码审查：发现潜在 Bug、性能问题和设计缺陷。
- 测试：生成单元测试、集成测试。
- 文档：生成技术方案、接口文档、模块说明。
- 重构：识别重复逻辑，改善代码结构。

我建议重点建立下面六个能力：

1. 上下文管理：减少 Token 消耗，避免 AI 遗忘重要信息。
2. 项目知识管理：让 AI 快速理解项目，而不是每次重新介绍。
3. Prompt 工程：让 AI 明确任务、边界和交付标准。
4. 任务拆分：避免一次性让 AI 修改大量代码。
5. 文档工程：让 AI 输出结构清晰、可维护的工程文档。
6. 质量控制：让 AI 的输出能够验证，而不是只看起来正确。

# 一、首先理解 AI 编程的工作原理

使用 AI 开发时，最重要的是理解 Context Window（上下文窗口）。

可以把 AI 的上下文理解成一个临时的工作记忆。

一次 AI 编程任务的上下文组成

System Instructions

模型行为规则、工具使用规则

Project Context

项目架构、技术栈、编码规范、业务规则

Conversation History

之前的讨论、决策、修改记录

Current Task

本次需求、相关代码、约束条件

Tool Results

文件读取、终端输出、测试结果、错误日志

可用上下文容量

有限资源

上述信息共同占用上下文。内容过多时，可能导致重要信息被压缩、遗漏，或者模型难以聚焦当前任务。

这里需要纠正一个常见误区：

上下文越多，不代表 AI 的开发效果越好。

真正重要的是上下文的相关性、准确性和组织方式。

例如：

- 当前任务是开发一个 React Modal。
- 没必要给 AI 读取整个项目的所有页面。
- 只需要提供 Modal 相关组件、调用方式、设计规范和必要的状态管理代码。

这就是上下文优化。

# 二、如何节省上下文？（重点）

我认为这是 AI 编程最值得掌握的能力之一。

## 1. 不要每次都让 AI 重新理解项目

错误做法：

```
请先阅读整个项目，理解项目架构，
然后帮我实现一个用户管理页面。
```

问题：

- 阅读大量无关代码。
- 消耗大量 Token。
- AI 可能把注意力放在不重要的模块。
- 每次任务重复读取相同信息。

推荐做法：

建立项目知识库。

```
project/
├── AGENTS.md
├── README.md
├── docs/
│   ├── architecture.md
│   ├── coding-standards.md
│   ├── business-rules.md
│   └── module-index.md
├── src/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   ├── pages/
│   └── utils/
└── ...
```

每个文件负责不同内容：

| 文件                  | 主要作用         |
| ------------------- | ------------ |
| AGENTS.md           | AI 开发规则和项目入口 |
| architecture.md     | 项目架构         |
| coding-standards.md | 编码规范         |
| business-rules.md   | 业务规则         |
| module-index.md     | 模块位置和代码入口    |

这样 AI 只需要先读取项目索引，再根据任务读取相关文件。

## 2. 使用渐进式上下文加载

这是我最推荐的方式。

不要一次性给 AI 所有资料，而是分三层加载：

L1 · 全局上下文

始终提供

技术栈、项目结构、编码规范、重要约束。

L2 · 模块上下文

按任务加载

当前业务模块、相关组件、API、Hooks、状态管理。

L3 · 细节上下文

按需加载

具体代码、错误日志、测试用例、历史实现。

例如，你要开发一个 React 用户管理页面：

L1：

```
使用 React + TypeScript。
项目使用函数组件和 Hooks。
遵循项目现有 ESLint 规则。
```

L2：

```
请读取：
src/pages/users/
src/services/user.ts
src/hooks/useUsers.ts
src/types/user.ts
```

L3：

```
请重点分析：
useUsers.ts 中的数据请求逻辑。
user.ts 中的接口定义。
现有分页组件的调用方式。
```

这样可以避免无意义的上下文加载。

## 3. 不要把所有历史对话都保留在一个会话

建议采用：

| 场景       | 会话策略 |
| -------- | ---- |
| 一个独立 Bug | 一个会话 |
| 一个独立功能   | 一个会话 |
| 架构设计     | 独立会话 |
| 代码重构     | 独立会话 |
| 技术学习     | 独立会话 |
| 不相关的新需求  | 新会话  |

特别注意：

新会话不等于失去项目知识。

如果项目知识已经保存在 AGENTS.md 和 docs 中，新会话只需要重新加载必要的项目资料。

## 4. 使用摘要代替冗长历史

当一个任务持续很多轮时，可以让 AI 输出交接摘要。

例如：

```
请整理当前任务的开发状态，生成交接文档。

要求：
1. 已完成的功能
2. 当前项目结构
3. 已确定的技术方案
4. 关键技术决策及原因
5. 已修改的文件
6. 当前存在的问题
7. 尚未完成的任务
8. 下一步开发计划

要求：
只保留对后续开发有价值的信息。
删除已经解决的问题和无关讨论。
输出 Markdown。
```

保存为：

```
docs/handoffs/user-management.md
```

后续开启新会话时，只需要引用这份文档。

## 5. 避免无效输出

可以在项目规则中增加：

```
AI 输出规范：

1. 优先给出结论。
2. 不要重复用户已经提供的信息。
3. 不要输出与任务无关的代码。
4. 修改代码时只修改必要文件。
5. 不要无故重复完整项目结构。
6. 解释代码时重点说明设计原因。
7. 任务完成后简要总结修改内容。
8. 如果信息不足，先说明缺失信息，不要猜测。
```

## 6. 控制 AI 的文件读取范围

这是一个容易忽略但非常重要的细节。

不要这样要求：

```
请分析 src 目录下的所有文件。
```

而应该：

```
当前任务是开发用户列表分页功能。

请先：
1. 查看 src/pages/users 的目录结构。
2. 查找项目现有分页组件。
3. 查找已有的数据请求 Hook。
4. 查找相关 TypeScript 类型。

不要读取无关模块。
如果发现依赖其他模块，再按需扩展阅读范围。
```

一个实用原则：先定位，再阅读；先阅读，再修改。

这也符合 OpenAI 对 Codex 的建议：提供任务相关的文件、目录和约束，而不是给 Agent 一本庞大的项目说明书。

[image](https://www.google.com/s2/favicons?domain=https://learn.chatgpt.com\&sz=32)

ChatGPT Learn+1



# 三、如何让 AI 更好地理解 React 项目？

对于 React 开发，我建议把项目知识拆成五类。

| 知识类型 | 需要让 AI 理解的内容               |
| ---- | -------------------------- |
| 技术架构 | React、TypeScript、Vite、构建方式 |
| 组件体系 | 基础组件、业务组件、组件复用规则           |
| 状态管理 | Context、Redux、Zustand 等    |
| 数据层  | API 封装、请求拦截器、缓存策略          |
| 业务规则 | 模块职责、业务流程、权限规则             |

## 1. 建立 React 项目架构文档

建议维护：

```
docs/
├── architecture/
│   ├── overview.md
│   ├── directory-structure.md
│   ├── component-architecture.md
│   ├── state-management.md
│   └── data-flow.md
├── development/
│   ├── coding-standards.md
│   ├── component-guide.md
│   ├── hooks-guide.md
│   ├── api-guide.md
│   └── testing-guide.md
└── modules/
    ├── user-management.md
    ├── authentication.md
    └── dashboard.md
```

不需要一次性全部编写。

建议先让 AI 分析现有项目，逐步生成文档，再由你审核。

## 2. 建立组件开发规范

例如：

```
# React Component Guidelines

## Component Design

- 使用函数组件。
- 使用 TypeScript。
- Props 必须定义明确的类型。
- 优先使用组合模式。
- 避免不必要的组件抽象。
- 单个组件只负责一个明确的职责。

## Hooks

- 遵循 React Hooks Rules。
- 自定义 Hook 使用 use 前缀。
- 避免将业务逻辑重复写入多个组件。
- 只有在确实需要时才使用 useMemo 和 useCallback。

## State Management

- 优先使用局部状态。
- 多组件共享状态时再考虑提升状态。
- 全局状态遵循项目现有方案。
- 避免重复存储可推导的数据。

## Performance

- 避免不必要的重复渲染。
- 列表合理使用 key。
- 避免在 render 中创建昂贵计算。
- 性能优化应基于实际问题。
```

这里有一个关键点：

不要只告诉 AI 使用什么技术，还要告诉 AI 什么情况下使用这些技术。

例如，不要简单规定所有事件函数都必须使用 `useCallback`。这会造成过度优化。

## 3. 让 AI 遵循现有项目模式

我非常推荐下面这条 Prompt：

```
实现新功能时：

1. 优先寻找项目中相似的功能实现。
2. 分析其组件结构、Hooks、API 和类型定义。
3. 优先复用已有的实现方式。
4. 不要擅自引入新的技术方案。
5. 如果现有架构不适合当前需求，
   先说明原因和建议，再进行修改。
```

例如：

```
参考 src/pages/orders 中的实现方式，
开发 src/pages/users 用户管理模块。

保持现有的：
- 页面组件结构
- API 请求方式
- Loading 状态处理
- Error 状态处理
- 分页组件使用方式
```

这通常比让 AI 从零设计一套架构更加可靠。

# 四、如何让 AI 输出清晰、规范、易维护的文档？

这是另一个值得重点投入的方向。

我的建议是：不要简单地让 AI「写一份文档」，而是让 AI 按照固定的文档工程规范输出。

## 1. 首先确定文档类型

不同文档应该有不同结构。

| 文档类型     | 推荐结构               |
| -------- | ------------------ |
| 需求文档 PRD | 背景、目标、功能需求、验收标准    |
| 技术设计文档   | 背景、方案、架构、流程、接口、风险  |
| 组件文档     | 用途、Props、使用示例、注意事项 |
| API 文档   | 地址、方法、参数、响应、错误码    |
| 模块文档     | 职责、目录、依赖、业务流程      |
| 开发指南     | 环境、安装、配置、运行、调试     |
| 变更文档     | 修改内容、影响范围、兼容性、迁移方式 |

不要把所有内容都放进 README.md。

README 应该是项目入口，而不是整个项目的知识仓库。

## 2. 推荐技术设计文档模板

下面这份模板可以直接用于 React 项目。

# 技术设计文档

## 1. 文档信息

| 项目   | 内容                 |
| ---- | ------------------ |
| 功能名称 | 用户管理模块             |
| 技术栈  | React + TypeScript |
| 文档版本 | 1.0.0              |
| 状态   | Draft              |
| 更新时间 | YYYY-MM-DD         |

## 2. 背景与目标

### 2.1 背景

描述为什么需要开发此功能。

### 2.2 目标

明确需要解决的问题和预期结果。

### 2.3 非目标

明确本次不处理的功能。

## 3. 需求分析

### 3.1 功能需求

- 功能 A
- 功能 B
- 功能 C

### 3.2 业务规则

- 规则 A
- 规则 B

### 3.3 验收标准

- 验收条件 A
- 验收条件 B

## 4. 技术方案

### 4.1 整体架构

说明模块组成及职责。

### 4.2 组件设计

说明组件层级、Props 和组件职责。

### 4.3 状态管理

说明状态归属、状态流转及数据来源。

### 4.4 API 设计

说明接口、参数、响应及异常处理。

### 4.5 数据流

说明用户操作、数据请求、状态更新和页面渲染过程。

## 5. 文件结构

列出新增和修改的文件及其职责。

## 6. 核心实现

描述关键实现逻辑，避免复制大量源代码。

## 7. 异常与边界处理

说明 Loading、Error、Empty、权限及异常场景。

## 8. 测试方案

列出单元测试、集成测试及关键验收场景。

## 9. 风险与注意事项

说明兼容性、性能、安全及潜在风险。

## 10. 实施计划

列出实施步骤和依赖关系。

## 3. 让 AI 输出文档时，增加质量约束

推荐使用以下 Prompt：

```
请为当前功能生成技术设计文档。

要求：

一、内容规范
1. 使用 Markdown。
2. 使用清晰的多级标题。
3. 先描述结论，再解释原因。
4. 使用表格展示结构化信息。
5. 使用 Mermaid 描述复杂流程。
6. 使用代码块展示必要的代码示例。

二、准确性
1. 只记录已经确认的信息。
2. 不要编造接口、参数和业务规则。
3. 不确定的内容标记为 TBD。
4. 区分现状、设计方案和未来规划。

三、可维护性
1. 不要复制大量源代码。
2. 通过文件路径引用实际代码。
3. 说明模块之间的依赖关系。
4. 记录重要技术决策及原因。

四、最终检查
1. 检查文档是否存在内容重复。
2. 检查标题层级是否合理。
3. 检查代码示例是否与项目一致。
4. 检查文档与实际代码是否一致。
```

特别强调其中的三条：

- 不确定的内容标记为 TBD。
- 区分现状和设计方案。
- 文档必须与实际代码一致。

这可以有效避免 AI 生成看似专业、实际上存在大量虚构信息的文档。

## 4. 用 Mermaid 代替大量文字

例如，描述 React 页面数据请求流程：

\#chatgpt-mermaid-\_r_ph\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(13, 13, 13);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_ph\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ph\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ph\_ .error-icon{fill:rgb(243, 243, 243);}#chatgpt-mermaid-\_r_ph\_ .error-text{fill:rgb(13, 13, 13);stroke:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_ph\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_ph\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_ph\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_ph\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_ph\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ph\_ .marker.cross{stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ph\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_ph\_ p{margin:0;}#chatgpt-mermaid-\_r_ph\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .cluster-label text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .cluster-label span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_ph\_ .label text,#chatgpt-mermaid-\_r_ph\_ span{fill:rgb(13, 13, 13);color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .node rect,#chatgpt-mermaid-\_r_ph\_ .node circle,#chatgpt-mermaid-\_r_ph\_ .node ellipse,#chatgpt-mermaid-\_r_ph\_ .node polygon,#chatgpt-mermaid-\_r_ph\_ .node path{fill:rgb(222, 234, 251);stroke:rgb(83, 154, 248);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .rough-node .label text,#chatgpt-mermaid-\_r_ph\_ .node .label text,#chatgpt-mermaid-\_r_ph\_ .image-shape .label,#chatgpt-mermaid-\_r_ph\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_ph\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .rough-node .label,#chatgpt-mermaid-\_r_ph\_ .node .label,#chatgpt-mermaid-\_r_ph\_ .image-shape .label,#chatgpt-mermaid-\_r_ph\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_ph\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_ph\_ .root .anchor path{fill:rgb(143, 143, 143)!important;stroke-width:0;stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ph\_ .arrowheadPath{fill:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ph\_ .edgePath .path{stroke:rgb(143, 143, 143);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .flowchart-link{stroke:rgb(143, 143, 143);fill:none;}#chatgpt-mermaid-\_r_ph\_ .edgeLabel{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_ph\_ .edgeLabel p{background-color:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_ph\_ .edgeLabel rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_ph\_ .labelBkg{background-color:rgba(252, 252, 252, 0.5);}#chatgpt-mermaid-\_r_ph\_ .cluster rect{fill:rgb(243, 243, 243);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .cluster text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ .cluster span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(243, 243, 243);border:1px solid rgba(0, 0, 0, 0.1);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_ph\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ph\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_ph\_ .icon-shape,#chatgpt-mermaid-\_r_ph\_ .image-shape{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_ph\_ .icon-shape p,#chatgpt-mermaid-\_r_ph\_ .image-shape p{background-color:rgb(252, 252, 252);padding:2px;}#chatgpt-mermaid-\_r_ph\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_ph\_ .image-shape .label rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_ph\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_ph\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_ph\_ .node .neo-node{stroke:rgb(83, 154, 248);}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_ph\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_ph\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node .neo-line path{stroke:rgb(83, 154, 248);filter:none;}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_ph\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_ph\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ph\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_ph\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ph\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_ph\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_ph\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_ph\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_ph\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .node rect,#chatgpt-mermaid-\_r_ph\_ .node circle,#chatgpt-mermaid-\_r_ph\_ .node ellipse,#chatgpt-mermaid-\_r_ph\_ .node polygon,#chatgpt-mermaid-\_r_ph\_ .node path{fill:rgb(229, 243, 255);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_ph\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_ph\_ .mermaid-decision .label-container{fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_ph\_ .edgePaths .flowchart-link{stroke:rgb(143, 143, 143);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_ph\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ph\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}用户打开页面React ComponentuseUsers HookUser ServiceHTTP ClientBackend API返回数据更新状态React 重新渲染

描述组件之间的关系：

\#chatgpt-mermaid-\_r_po\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(13, 13, 13);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_po\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_po\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_po\_ .error-icon{fill:rgb(243, 243, 243);}#chatgpt-mermaid-\_r_po\_ .error-text{fill:rgb(13, 13, 13);stroke:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_po\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_po\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_po\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_po\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_po\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_po\_ .marker.cross{stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_po\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_po\_ p{margin:0;}#chatgpt-mermaid-\_r_po\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .cluster-label text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .cluster-label span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_po\_ .label text,#chatgpt-mermaid-\_r_po\_ span{fill:rgb(13, 13, 13);color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .node rect,#chatgpt-mermaid-\_r_po\_ .node circle,#chatgpt-mermaid-\_r_po\_ .node ellipse,#chatgpt-mermaid-\_r_po\_ .node polygon,#chatgpt-mermaid-\_r_po\_ .node path{fill:rgb(222, 234, 251);stroke:rgb(83, 154, 248);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .rough-node .label text,#chatgpt-mermaid-\_r_po\_ .node .label text,#chatgpt-mermaid-\_r_po\_ .image-shape .label,#chatgpt-mermaid-\_r_po\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_po\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .rough-node .label,#chatgpt-mermaid-\_r_po\_ .node .label,#chatgpt-mermaid-\_r_po\_ .image-shape .label,#chatgpt-mermaid-\_r_po\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_po\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_po\_ .root .anchor path{fill:rgb(143, 143, 143)!important;stroke-width:0;stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_po\_ .arrowheadPath{fill:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_po\_ .edgePath .path{stroke:rgb(143, 143, 143);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .flowchart-link{stroke:rgb(143, 143, 143);fill:none;}#chatgpt-mermaid-\_r_po\_ .edgeLabel{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_po\_ .edgeLabel p{background-color:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_po\_ .edgeLabel rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_po\_ .labelBkg{background-color:rgba(252, 252, 252, 0.5);}#chatgpt-mermaid-\_r_po\_ .cluster rect{fill:rgb(243, 243, 243);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .cluster text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ .cluster span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(243, 243, 243);border:1px solid rgba(0, 0, 0, 0.1);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_po\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_po\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_po\_ .icon-shape,#chatgpt-mermaid-\_r_po\_ .image-shape{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_po\_ .icon-shape p,#chatgpt-mermaid-\_r_po\_ .image-shape p{background-color:rgb(252, 252, 252);padding:2px;}#chatgpt-mermaid-\_r_po\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_po\_ .image-shape .label rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_po\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_po\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_po\_ .node .neo-node{stroke:rgb(83, 154, 248);}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_po\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_po\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_po\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node .neo-line path{stroke:rgb(83, 154, 248);filter:none;}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_po\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_po\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_po\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_po\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_po\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_po\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_po\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_po\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_po\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .node rect,#chatgpt-mermaid-\_r_po\_ .node circle,#chatgpt-mermaid-\_r_po\_ .node ellipse,#chatgpt-mermaid-\_r_po\_ .node polygon,#chatgpt-mermaid-\_r_po\_ .node path{fill:rgb(229, 243, 255);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_po\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_po\_ .mermaid-decision .label-container{fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_po\_ .edgePaths .flowchart-link{stroke:rgb(143, 143, 143);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_po\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_po\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}UserPageUserToolbarUserTablePaginationUserRowUserActions

对于架构、业务流程和数据流，图形往往比大段文字更容易理解。

# 五、如何设计高质量的 AI 开发 Prompt？

不要把 Prompt 理解为一句命令。

一个高质量的开发 Prompt 应该包含六个部分：

## AI 开发 Prompt 公式

1\. Role — 角色

你是具有丰富 React 和 TypeScript 经验的高级前端工程师。

2\. Goal — 目标

实现用户管理页面，支持查询、分页和删除。

3\. Context — 上下文

指定相关文件、现有实现和项目架构。

4\. Constraints — 约束

遵循现有组件规范，不引入新的依赖。

5\. Output — 输出

实现代码、测试、技术文档。

6\. Acceptance Criteria — 验收标准

功能正确、类型检查通过、Lint 通过、测试通过。

## 一个可以直接复用的开发 Prompt

# React 功能开发任务

## 1. 角色

你是一名资深 React + TypeScript 前端工程师。

## 2. 开发目标

实现：[具体功能描述]

## 3. 项目上下文

- 技术栈：React + TypeScript
- 项目规范：遵循仓库 AGENTS.md
- 相关模块：[填写目录或文件]
- 参考实现：[填写相似模块]

## 4. 开发约束

- 优先复用现有组件和 Hooks。
- 遵循现有架构和编码规范。
- 不随意修改无关文件。
- 不擅自引入第三方依赖。
- 不确定的业务规则先提出问题。
- 不要为了简单功能过度抽象。

## 5. 执行步骤

第一阶段：需求分析

- 理解需求。
- 检查相关代码。
- 识别依赖和潜在风险。

第二阶段：技术设计

- 说明组件设计。
- 说明状态管理。
- 说明数据流。
- 列出需要修改的文件。

第三阶段：代码实现

- 按照确认的方案实现。
- 保持代码风格一致。

第四阶段：验证

- 执行 TypeScript 检查。
- 执行 ESLint。
- 执行相关测试。
- 检查是否影响现有功能。

第五阶段：总结

- 修改了哪些文件。
- 实现了哪些功能。
- 做了哪些验证。
- 还有哪些问题。

## 6. 验收标准

只有满足以下条件才算完成：

- 功能符合需求。
- 没有明显的类型错误。
- Lint 检查通过。
- 相关测试通过。
- 文档与实现一致。

## 不同任务使用不同 Prompt

不要所有任务都使用同一套流程。

| 任务     | 最重要的要求            |
| ------ | ----------------- |
| 新功能开发  | 需求、设计、实现、验收       |
| Bug 修复 | 复现步骤、根因、最小修复、回归测试 |
| 代码重构   | 保持行为不变、明确重构边界     |
| 代码审查   | 找问题、给证据、说明风险      |
| 技术调研   | 比较方案、成本、优缺点       |
| 文档生成   | 信息准确、结构清晰、代码一致    |

# 六、AI 编程中必须注意的事项

这部分比 Prompt 技巧更重要。

## 1. 不要盲目相信 AI 生成的代码

React 开发尤其需要关注：

| 领域         | 常见问题              |
| ---------- | ----------------- |
| Hooks      | 违反 Hooks 调用规则     |
| useEffect  | 依赖项错误、重复请求、竞态问题   |
| State      | 状态冗余、状态不同步        |
| TypeScript | 滥用 any、类型断言掩盖问题   |
| Rendering  | 不必要的重复渲染          |
| Component  | 组件职责不清、过度抽象       |
| API        | 参数不匹配、异常处理遗漏      |
| Security   | XSS、敏感信息泄露、权限校验缺失 |

尤其需要注意：AI 能生成可以运行的代码，不代表它生成的是合理的工程代码。

## 2. 不要让 AI 一次性修改过多文件

建议把一个需求拆成可独立验证的任务。

例如开发用户管理模块：

```
Task 1：定义 User TypeScript 类型

Task 2：实现 User API Service

Task 3：实现 useUsers Hook

Task 4：实现 UserTable 组件

Task 5：实现 UserPage 页面

Task 6：实现分页、查询和删除

Task 7：编写测试并生成文档
```

每个任务完成后：

1. 查看 Git Diff。
2. 检查修改文件。
3. 运行必要的测试。
4. 确认没有破坏已有功能。
5. 再继续下一步。

## 3. 不要让 AI 擅自改变项目架构

特别注意这些行为：

- 擅自替换状态管理方案。
- 擅自更换 UI 组件库。
- 重复创建已有工具函数。
- 引入功能重复的 npm 包。
- 为简单功能创建复杂的抽象层。
- 未经确认修改公共组件接口。

推荐在项目规则中增加：

```
架构修改规则：

1. 优先遵循现有架构。
2. 修改公共组件前分析影响范围。
3. 引入新依赖前说明必要性。
4. 不随意修改全局配置。
5. 涉及架构调整时，先提交技术方案。
6. 不得擅自删除已有功能。
```

## 4. 安全与隐私

不要直接向 AI 提供：

- 生产环境密钥。
- API Token。
- 数据库密码。
- 用户个人敏感信息。
- 未脱敏的生产数据。

涉及认证、权限、文件上传、支付等功能时，必须进行人工安全审查。

## 5. 文档不能代替代码事实

AI 生成文档后，需要反向验证：

- 文档中的文件是否真实存在？
- 接口是否真实存在？
- 参数类型是否正确？
- 组件 Props 是否与代码一致？
- 流程图是否符合实际调用关系？

文档最好与代码修改放在同一个 PR 中审查。

# 七、结合 Codex / Cursor，建立一套实际工作流

考虑到你使用 React、TypeScript，并且已经接触 Codex CLI 和 Cursor，我建议采用以下方式。

## 1. 项目级 AI 配置

推荐目录：

```
your-project/
│
├── AGENTS.md
│
├── docs/
│   ├── README.md
│   ├── architecture/
│   ├── development/
│   ├── modules/
│   └── decisions/
│
├── prompts/
│   ├── feature-development.md
│   ├── bug-fix.md
│   ├── code-review.md
│   ├── refactoring.md
│   └── documentation.md
│
├── src/
└── package.json
```

注意这些文件的职责：

| 文件        | 职责             |
| --------- | -------------- |
| AGENTS.md | 告诉 AI 如何在项目中工作 |
| docs/     | 保存项目长期知识       |
| prompts/  | 保存可重复使用的任务模板   |
| src/      | 实际业务代码         |

这里有一个重要的工程实践：

AGENTS.md 不应该成为项目百科全书。

只保留高频、稳定、必须遵守的规则。具体架构和模块知识放在 docs 中，AGENTS.md 负责告诉 AI 去哪里查。

OpenAI 官方也建议保持 AGENTS.md 精简，通过项目知识库和按需加载提供详细上下文。

[image](https://www.google.com/s2/favicons?domain=https://learn.chatgpt.com\&sz=32)

ChatGPT Learn+2



## 2. 推荐 AGENTS.md 内容

下面是一份适合 React + TypeScript 项目的基础版本：

# AGENTS.md

## 1. Project Overview

This project is a React + TypeScript application.

Follow the existing project architecture and implementation patterns.

## 2. Technology Stack

- React
- TypeScript
- Vite
- ESLint
- [State Management]
- [UI Component Library]

## 3. Coding Standards

- Use functional components.
- Use TypeScript for all new code.
- Define explicit Props interfaces or types.
- Follow existing naming conventions.
- Avoid unnecessary abstractions.
- Avoid unnecessary dependencies.
- Follow React Hooks Rules.

## 4. Architecture Rules

- Reuse existing components and utilities.
- Follow existing API service patterns.
- Follow existing state management patterns.
- Do not introduce new architectural patterns without justification.
- Do not modify unrelated modules.

## 5. Development Workflow

Before implementation:

1. Understand the task.
2. Inspect relevant source files.
3. Identify existing implementation patterns.
4. Identify dependencies and risks.
5. Propose a plan for complex changes.

After implementation:

1. Review the Git diff.
2. Run TypeScript checks.
3. Run ESLint.
4. Run relevant tests.
5. Summarize changes and verification results.

## 6. Documentation

- Store project architecture in docs/architecture.
- Store development guidelines in docs/development.
- Store module documentation in docs/modules.
- Keep documentation consistent with source code.
- Mark unconfirmed information as TBD.

## 7. Project References

Read only the documentation relevant to the current task.

- docs/README.md: Documentation index
- docs/architecture/: Architecture references
- docs/development/: Development guidelines
- docs/modules/: Module specifications
- docs/decisions/: Architecture decision records

## 3. 推荐实际开发流程

## AI 辅助开发生命周期

1. 需求分析（Ask）

   AI 理解需求、检查代码、识别风险，不修改代码。
2. 技术设计（Plan）

   输出方案、文件修改清单、组件关系和验收标准。
3. 代码实现（Code）

   按照任务边界逐步修改代码。
4. 自动验证（Test）

   执行 TypeScript、ESLint、单元测试和构建。
5. 代码审查（Review）

   检查 Git Diff、边界条件、性能和安全问题。
6. 文档更新（Document）

   更新模块文档、技术决策和必要的使用说明。
7. 交付（Deliver）

   汇总修改内容、验证证据和遗留问题。

对于复杂任务，建议先使用 Ask 模式进行分析，再进入 Code 模式实现。OpenAI 的 Codex 实践指南也强调了先规划、任务适度拆分以及通过测试验证输出。

[image](https://www.google.com/s2/favicons?domain=https://cdn.openai.com\&sz=32)

OpenAI+1



# 八、进一步提升：建立 AI 知识资产体系

如果你长期使用 AI 开发，我建议不要只积累 Prompt，而是积累四种可复用资产。

| 资产           | 解决的问题        | 举例               |
| ------------ | ------------ | ---------------- |
| Instructions | AI 应该遵循什么规则  | AGENTS.md        |
| Knowledge    | AI 应该了解什么知识  | React 架构文档       |
| Prompts      | AI 应该如何完成任务  | Bug 修复模板         |
| Skills       | AI 如何执行重复工作流 | React 组件开发 Skill |

这四者不要混淆：

- Instructions 是规则。
- Knowledge 是事实。
- Prompts 是任务要求。
- Skills 是可复用的执行流程。

例如，你可以建立一个 `react-component-development` Skill，让 AI 每次开发组件时自动执行：

1. 分析现有组件。
2. 检查 Props 设计。
3. 确定状态归属。
4. 编写组件。
5. 编写测试。
6. 更新组件文档。
7. 执行验证。

Codex 支持通过 Skills 封装重复工作流，适合把成熟的开发经验沉淀下来。

[image](https://www.google.com/s2/favicons?domain=https://learn.chatgpt.com\&sz=32)

ChatGPT Learn+1



# 九、我建议你的学习和实践优先级

不要一开始就研究复杂的 Agent、MCP、多 Agent 协作。

先把基础工作流做好。

## AI 前端开发能力成长路线

1. 第一阶段：Prompt Engineering

   优先级最高

   学会写清楚需求、上下文、约束和验收标准。

   建议投入：3 天
2. 第二阶段：Context Engineering

   学会 AGENTS.md、项目索引、渐进式上下文加载和会话交接。

   建议投入：3 天
3. 第三阶段：AI Development Workflow

   建立需求分析、技术设计、编码、测试、Review 的闭环。

   建议投入：5 天
4. 第四阶段：Documentation Engineering

   建立 PRD、技术设计、模块文档、组件文档和决策记录。

   建议投入：3 天
5. 第五阶段：Skills & Automation

   将高频开发任务封装成 Skills，逐步提高自动化程度。

   建议投入：持续迭代

## 十、最后总结：最值得记住的 8 条原则

1. 不要让 AI 每次重新理解整个项目。
2. 不要一次性提供所有上下文，采用渐进式加载。
3. AGENTS.md 保持精简，详细知识放在 docs。
4. 一个会话尽量只处理一个明确的开发目标。
5. 复杂需求先分析、再设计、后编码。
6. AI 修改代码后必须进行测试和 Diff Review。
7. 文档必须以实际代码和已确认的业务事实为依据。
8. 把成熟的 Prompt 和工作流程沉淀成可复用资产。

我的核心建议：从 Prompt Engineering 逐步升级到 Context Engineering，再升级到 Agent Workflow Engineering。

这三个阶段实际上代表了 AI 开发能力的提升：

- Prompt Engineering：让 AI 知道做什么。
- Context Engineering：让 AI 知道应该基于什么做。
- Agent Workflow Engineering：让 AI 能够按照工程流程把事情做好。

## 下一步：建议直接建立你的 React AI 开发规范

我建议不要停留在理论学习，而是拿一个真实 React 项目来实践。

我可以进一步帮你建立一套完整的 React AI 开发工程规范（React AI Engineering Standard），包含：

## 建议建设的工程资产

选择需要建设的部分

7/7 项

项目级 AGENTS.md

AI 全局开发规则

React + TypeScript 编码规范

组件、Hooks、类型、性能规范

Context 管理规范

上下文分层、加载、压缩和交接

Prompt 模板库

需求、开发、调试、重构、Review

技术文档模板库

PRD、架构、组件、API、模块文档

React 开发 Skills

组件开发、Bug 修复、测试、文档生成

Codex + Cursor 使用规范

项目配置、协作方式和工作流

按所选内容开始建设

我更推荐最终形成一套可以直接放进 Git 仓库、在 Codex 和 Cursor 中持续复用的工程规范，而不是一堆孤立的 Prompt。

参考资料：

- [OpenAI：Codex Best Practices](https://learn.chatgpt.com/docs/learn/best-practices?utm_source=chatgpt.com)
- [OpenAI：AGENTS.md 配置指南](https://learn.chatgpt.com/docs/agent-configuration/agents-md?trk=article-ssr-frontend-pulse_little-text-block\&utm_source=chatgpt.com)
- [OpenAI：Harness Engineering](https://openai.com/index/harness-engineering/?utm_source=chatgpt.com)
- [VS Code：Context Engineering Guide](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide?utm_source=chatgpt.com)