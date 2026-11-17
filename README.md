# DSH 接入第三方 AI 模型

> 让 **DeepSeek Harness**（DSH）用上 Claude、Gemini、GPT、国产模型等任意第三方 API。
> 不用等官方支持，改一处配置就行。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## 这是什么

DeepSeek Harness 默认只能用它自家的模型。但它的架构是「一切皆插件」，
自带的 `@deepseek-ai/dsh-llm-pi-ai` 插件可以接入**任何 OpenAI 兼容**或
**Anthropic 原生协议**的服务商。

本仓库把接入过程整理成**两份文档**：一份给人看，一份直接喂给 AI。

---

## 快速开始

### 如果你是「不懂电脑」的人

打开 👉 **[docs/操作手册.md](docs/操作手册.md)**

翻到「第二部分：给人看的操作步骤」，从第 1 步开始照着点。
每一步都写清了点什么、看到什么、填什么。

**遇到问题**：看手册「第五部分：出问题怎么办」，5 种常见症状都有对应解法。

### 如果你想让 AI 帮你弄

打开 👉 **[docs/给AI的指令.txt](docs/给AI的指令.txt)**

**全选复制**，粘贴到你的 AI 对话框（Claude / ChatGPT / DeepSeek 都行），发出去。
它会问你服务商地址和密钥，然后带你一步步做。

### 如果你自己懂点技术

看 **[docs/操作手册.md](docs/操作手册.md)** 的「第一部分」，
里面是完整的 YAML 配置模板和字段说明。

---

## 支持什么

| 协议 | `api` 字段填什么 | 典型服务商 |
|---|---|---|
| OpenAI 兼容 | `openai-completions` | 七牛云、OpenRouter、各类中转站、Ollama 本地 |
| OpenAI Responses | `openai-responses` | OpenAI 官方新版接口 |
| Anthropic 原生 | `anthropic-messages` | Anthropic 官方、APIYi Claude |

---

## 配置完你能得到

- 在 DSH 里自由切换不同厂商的模型
- 每个模型独立的上下文窗口 / 输出上限 / 思考强度
- 和 DSH 原生模型完全一样的使用体验（工具调用、MCP、子代理都不受影响）

---

## 三个必须显式声明的字段

**这是本仓库最有价值的部分**——漏了任何一个都会出问题，而且报错信息很不直观：

| 字段 | 漏了会怎样 | 正确写法 |
|---|---|---|
| `input` | 模型读不了图片，报 `does not declare image input` | `input: ["text", "image"]` |
| `contextWindow` / `maxTokens` | 上下文很快显示「已用 100%」，长对话被截断 | 按模型官方规格填，见[容量对照](docs/操作手册.md#附录各模型真实容量对照) |
| `reasoningEfforts` | 界面里看不到「思考强度」选项 | `reasoningEfforts: {"off":, low: low, medium: medium, high: high, max: max}` |

**还有一个 YAML 陷阱**：YAML 1.1 会把 `off:` 解析成布尔值 `false`，
必须写成 `"off":` 带引号，否则这个映射键会变成 `false`。

---

## 仓库结构

```
.
├── README.md                    # 你正在看的这个
├── docs/
│   ├── 操作手册.md              # 完整手册（给人和 AI）
│   └── 给AI的指令.txt           # 纯指令版（复制粘贴给 AI）
└── LICENSE
```

---

## 验证状态

| 内容 | 状态 |
|---|---|
| 改配置文件接入 Claude/Gemini（APIYi） | ✅ 2026-11 实测通过 |
| 读图 / 网页抓取 / 网络搜索 | ✅ 实测通过 |
| 图形界面「添加模型提供商」 | ⚠️ 有此按钮，未走完整流程 |
| 其他服务商（七牛云/StepFun/OpenRouter） | ⚠️ 参数来自公开文档，未逐一实测 |

**原理说明**：本仓库的内容是在真实环境中踩坑后整理的，
不是从文档抄的。三个「必须声明」的字段都是实际踩出来的。

---

## 相关链接

- [DeepSeek Harness 官方仓库](https://github.com/deepseek-ai/deepseek-harness)
- [Anthropic 模型规格](https://platform.claude.com/docs/en/models/overview)
- [Gemini 4 Argon 规格](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/4-argon)

---

## License

MIT
