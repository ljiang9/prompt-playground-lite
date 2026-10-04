# prompt-playground-lite · 本地提示词调试台

一个**单个 HTML 文件、零第三方依赖**的 Prompt 调试小工具。用来本地调试 system prompt、模板变量、对比渲染结果，或直连任意 OpenAI 兼容接口做真实对话；**没有 API key、没有网络时也能完整跑通核心流程**（本地回声模式）。

![技术栈](https://img.shields.io/badge/纯前端-单个HTML-blue) ![依赖](https://img.shields.io/badge/依赖-无-brightgreen) ![许可证](https://img.shields.io/badge/许可证-MIT-lightgrey)

---

## 功能简介

- **系统提示词（system prompt）**：独立输入框，随每次请求一起发送。
- **模板 + 自动变量输入框**：在模板里写 `{{变量名}}`，页面会**自动为每个变量生成输入框**，填好后实时替换。
- **对话展示区**：气泡式展示「你 / AI」往来消息，带角色标注。
- **历史记录本地持久化**：配置、变量值、对话都存浏览器 `localStorage`，刷新不丢。
- **导出 JSON**：一键把配置 + 变量 + 对话导出为 `.json` 文件备份。
- **真实 API 调用**：可填 OpenAI 兼容的 `base_url` / `api_key` / `model`，用 `fetch` 请求 `/chat/completions`；**同时支持流式（SSE）与非流式**两种模式，可勾选切换。
- **无 key 本地回声**：只要任一项未填（base_url / key / model），自动进入「本地回声模式」——把变量填充进模板后回显给你，并附带 system prompt 预览，保证断网/无 key 下也能调试模板逻辑。

## 快速开始

无需安装、无需构建：

1. 下载 `index.html`；
2. **双击用浏览器打开**即可使用。

也可以本地起静态服务：

```bash
python3 -m http.server 8000
# 浏览器访问 http://localhost:8000/index.html
```

## 使用示例

### 无 key / 无网络（本地回声模式）

1. 打开页面，**留空 Base URL / API Key / 模型**；
2. 在「系统提示词」里写：`你是一个翻译助手。`
3. 在「提示词模板」里写：

   ```
   请把下面这句话翻译成英文：
   {{句子}}
   风格：{{风格}}
   ```

4. 页面会自动生成 `{{句子}}`、`{{风格}}` 两个输入框，分别填 `今天天气不错`、`正式`；
5. 点「发送 / 运行」，右侧对话区会回显渲染后的完整 user 消息和 system prompt——这就是你的提示词最终长什么样。

### 有 key（真实 API 模式）

1. 填写：
   - Base URL：`https://api.openai.com/v1`（或任意兼容接口，如本地代理 `http://localhost:11434/v1`）
   - API Key：你的 `sk-...`
   - 模型：`gpt-4o-mini`
2. 保持勾选「使用流式（SSE）请求」可实时看到逐字输出；取消勾选则一次性返回。
3. 点「发送 / 运行」，AI 回复会出现在右侧对话区。

> 安全提示：API Key 仅保存在你本机浏览器的 `localStorage` 中，不会上传到任何第三方；「导出 JSON」会把 key 一并导出，请勿外传该文件。

## 目录说明

```
prompt-playground-lite/
├── index.html    # 全部代码（HTML + CSS + JS 内联），双击即可运行
├── README.md     # 本说明
├── LICENSE       # MIT 许可证
└── .gitignore
```

## 无 API key / 无网络如何使用

本项目核心功能（模板编辑、变量自动生成、变量替换渲染、对话展示、本地历史、导出 JSON）**完全在浏览器本地完成，不依赖任何网络请求**。只有当你主动填了 base_url / api_key / model 三项后，才会向对应地址发起 `fetch` 调用。因此：

- **断网**：本地回声模式照常可用；
- **无 key**：自动走本地回声，把渲染结果回显，方便你离线打磨模板。

## 许可证

[MIT License](./LICENSE) © ljiang9
