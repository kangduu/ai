# Head towards AI

资深 Web 前端 → AI 工程师。

这是一份**案例库**，不是教程合集，也不是可安装的 SDK。每条知识都按前端熟悉的 I/O 契约沉淀：先看见喂进去什么、得到什么，再抽出可复用的产物。

## 怎么读

1. 打开一个案例，对照 **场景 → 输入 → 输出 → 产物**。
2. 需要索引时，再看 [知识点地图](docs/knowledge-map.md)。
3. 想补案例时，用 [案例模板](docs/case-template.md)，并遵守 [目录约定](docs/conventions.md)。

完整阅读顺序见 [学习路径](docs/learning-path.md)。前端概念如何映射到 AI 工程，见 [概念对照](docs/concepts.md)。

## 当前案例

| 案例 | 层 | 输入 | 输出 | 产物 |
| --- | --- | --- | --- | --- |
| [Git 提交信息](git/case.md) | 行动与编排 / Skill | 用户原话 + `git status` / `git diff` | 三种 Conventional Commits 文案 | [git/SKILL.md](git/SKILL.md) |

其余主题均为待补，见 [路线图](docs/roadmap.md)。没有真实跑过的结果，不编造输出。

## 知识点地图

六层是索引，不是教程大纲。`prompt` / `agent` / `skill` 只是其中子项。每条知识点只回答三件事：典型输入、典型输出、对应哪个案例。详见 [docs/knowledge-map.md](docs/knowledge-map.md)。

### 1. 模型基础

token、上下文窗口、采样参数、模型选型、多模态。典型输入是同一句话加上不同模型或参数；输出是差异对比。

状态：待补

### 2. 交互与控制

Prompt、结构化输出、Function Calling。典型输入是 system/user 原文或 JSON Schema；输出是模型回复、合法 JSON 或 tool call。

状态：待补

### 3. 知识与检索

Embedding、RAG、向量库。典型输入是文档切片加用户问题；输出是命中片段和带引用的回答。

状态：待补

### 4. 行动与编排

Agent、Skill、MCP、工作流、记忆。典型输入是目标加工具或触发语；输出是调用轨迹和最终结果。

状态：已有 [Git 提交信息](git/case.md)

### 5. 工程化落地

流式、成本与延迟、Eval、可观测性、安全护栏、前端接入。典型输入是产品约束或案例集；输出是可运行接入、分数或失败样本。

状态：待补

### 6. 进阶选修

微调时机、数据与偏好的产品含义、框架怎么选。不覆盖训练论文。

状态：待补

## 目录

```text
.
├── README.md
├── LICENSE
├── docs/                 项目文档：索引、模板、地图、约定
├── git/                  第一个 Skill 案例（暂留主题目录）
│   ├── case.md           场景 / 输入 / 输出
│   └── SKILL.md          可复用产物
└── ...                   llm/ prompt/ rag/ agent/ skill/ 等按约定随案例新增
```

主题目录在有真实案例之前不建空文件夹。约定见 [docs/conventions.md](docs/conventions.md)。

## 如何新增案例

新内容必须带 **输入原文** 和 **输出样例**，禁止只交概念文。复制 [docs/case-template.md](docs/case-template.md)，按 [docs/conventions.md](docs/conventions.md) 放到对应主题目录。

## License

[MIT](LICENSE) © 2026 Seven
