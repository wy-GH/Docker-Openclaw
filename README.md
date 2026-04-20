# 🐳 Docker + OpenClaw 完整部署与使用指南

> [cite_start]**专为非计算机专业用户编写 · 每步操作均有详细说明** [cite: 4]

[cite_start]欢迎来到本项目！这份指南旨在帮助零基础、非计算机背景的用户，在自己的电脑或服务器上通过 Docker 顺利部署并使用 OpenClaw [cite: 2, 3, 4]。

无论你是想要打造个人 AI 助手，还是希望学习基础的容器部署知识，这份“保姆级”教程都能带你一步步完成目标。

### 📊 快速了解
* [cite_start]**门槛要求：** 完全无需编程基础 [cite: 5]
* [cite_start]**预计耗时：** 30 ~ 60 分钟 [cite: 5]
* [cite_start]**适用系统：** Windows / macOS / Linux [cite: 5]

---

## 📖 指南内容概览

[cite_start]完整的详细教程请查阅仓库中的文档，以下是教程覆盖的核心模块 [cite: 9]：

* [cite_start]**第一章：了解 Docker** —— 用生活中的例子听懂什么是 Docker，它和虚拟机有什么区别，以及解答“会不会拖慢电脑”的疑虑 [cite: 9]。
* [cite_start]**第二章：安装 Docker** —— 针对 Windows、macOS 和 Linux 三个平台提供保姆级图文/步骤说明 [cite: 9]。
* [cite_start]**第三章：学会使用 Docker** —— 掌握镜像、容器、仓库的核心概念，以及最常用的命令行操作 [cite: 9]。
* [cite_start]**第四章：部署 OpenClaw** —— 从创建目录、配置 `.env` 环境变量，到编写 `docker-compose.yml` 并最终启动服务 [cite: 9]。
* [cite_start]**第五章：日常维护** —— 涵盖软件更新、重要数据备份、以及如何清理 Docker 占用的磁盘空间 [cite: 9]。
* [cite_start]**第六章：故障排查** —— 遇到报错怎么办？教你配置国内镜像加速、查看日志以及解决常见问题 [cite: 9]。

---

## 📂 项目目录结构说明

[cite_start]部署完成后，你的本地 OpenClaw 项目将呈现如下结构，建议妥善保管并定期备份 `data` 目录 [cite: 231, 232]：

```text
[cite_start]openclaw/                          # 项目根目录 [cite: 232]
[cite_start]├── .env                           # 环境变量配置（密码等敏感信息） [cite: 232]
[cite_start]├── docker-compose.yml             # Docker 服务配置文件 [cite: 232]
[cite_start]├── config/                        # 应用配置目录 [cite: 232]
[cite_start]│   └── nginx.conf                 # Nginx 配置（如有需要） [cite: 232]
[cite_start]├── data/                          # 数据目录（重要，请定期备份） [cite: 232]
[cite_start]│   ├── db/                        # 数据库文件（PostgreSQL） [cite: 232]
[cite_start]│   └── uploads/                   # 用户上传的文件 [cite: 232]
[cite_start]└── logs/                          # 日志文件目录 [cite: 232]
```

------

# 🦞 OpenClaw 大模型 API 配置完全指南

> [cite_start]**面向非技术用户 · 零基础搞定大语言模型接入（v2026 版本）** [cite: 247]

在成功部署 OpenClaw 后，为它注入“灵魂”（配置大语言模型 API）是让它开始工作的关键一步。本仓库提供了详尽的保姆级图文指南，手把手教你接入全球最先进的 AI 模型。

---

## 📖 指南涵盖内容

无论你身处何地、使用什么设备，都能找到适合你的模型方案：

| 阵营分类        | 涵盖模型及服务商                                             | 特点说明                                                     |
| :-------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **🌍 国际顶尖**  | [cite_start]**OpenAI (GPT), Anthropic (Claude), Google (Gemini)** [cite: 248] | [cite_start]行业标杆，但国内通常需科学上网 [cite: 261, 287, 302] |
| **🇨🇳 国产主力** | [cite_start]**DeepSeek, 阿里通义千问, 腾讯混元, 智谱 GLM** [cite: 248] | [cite_start]**国内用户首选**，无需翻墙，高性价比甚至免费 [cite: 318, 337, 353, 363] |
| **🔀 聚合平台**  | [cite_start]**OpenRouter, SiliconFlow** [cite: 248]          | [cite_start]一个 API Key 即可调用数百种模型，省去多平台注册烦恼 [cite: 372, 373] |
| **🏠 本地部署**  | [cite_start]**Ollama** [cite: 248]                           | [cite_start]完全离线运行，数据不出本地，隐私保护最佳 [cite: 388, 389] |

---

## ⚙️ 核心配置原理：一图看懂

在 OpenClaw 中配置模型，你只需要理解这两个核心文件的分工：

* 🔑 **`~/.openclaw/agents/main/agent/auth-profiles.json` （钥匙串）**
    * [cite_start]**作用：** 专门存放各个服务商的 API Key [cite: 251]。
    * [cite_start]*注：v2026 版本起，配置字段已由 `token` 变更为 `key`* [cite: 277, 315]。
* 📋 **`~/.openclaw/openclaw.json` （点菜单）**
    * [cite_start]**作用：** 决定你当前主要使用哪个模型，以及主模型罢工时，备用模型是谁（Fallbacks）[cite: 251]。

[cite_start]*建议直接通过 OpenClaw 的 Control UI (浏览器界面 -> 左侧「配置」菜单) 进行可视化修改，无需手动寻找代码文件！* [cite: 252, 270, 271, 272]

---

## 💡 推荐的多模型组合方案 (Fallbacks)

[cite_start]OpenClaw 强大的故障自动切换功能，可以保证你的 AI 助理永不掉线 [cite: 404]：

[cite_start]**方案一：国内全网通（强烈推荐国内用户）** [cite: 406]
* [cite_start]**主模型 (Primary)：** `DeepSeek V3 / R1` （价格极低，能力顶尖）[cite: 318, 407]
* [cite_start]**备用模型 (Fallbacks)：** `阿里通义千问` -> `智谱 GLM` -> `Claude (通过 OpenRouter)` [cite: 407]

[cite_start]**方案二：国际旗舰优先（适合网络畅通用户）** [cite: 408]
* [cite_start]**主模型 (Primary)：** `Claude Sonnet` （日常逻辑处理极佳）[cite: 287, 409]
* [cite_start]**备用模型 (Fallbacks)：** `GPT-4o` -> `Gemini 2.5 Pro` -> `DeepSeek (国内节点兜底)` [cite: 409]

[cite_start]*(详细的 JSON 配置代码请参阅文档第九章)* [cite: 403, 407, 409]

---

## 🛠️ 常用排障命令速查

[cite_start]遇到 `No API key found` 或连不上网？打开终端，使用这些自带的诊断工具 [cite: 258, 425]：

```bash
# 查看所有模型的认证状态（是否亮绿灯）
openclaw models status

# 综合体检（自动检查配置文件格式、网络连通性等）
openclaw doctor

# 让系统尝试自动修复发现的配置错误
openclaw doctor --fix

# 查看实时运行日志，精准定位错误
openclaw logs --follow
```

