<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-8A2BE2" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="MIT License">
  <img src="https://img.shields.io/badge/feishu-wiki-blue" alt="Feishu Wiki">
</p>

<h1 align="center">🎙️ 面试复盘 · Interview Debrief</h1>

<p align="center">
  <b>Claude Code 面试复盘助手</b> —— 上传录音或转录文本，自动生成完整对话、复盘提纲与补强回答，同步保存到飞书知识库。<br>
  <i>An interview debrief skill for Claude Code — upload recordings or transcripts, auto-generate structured Q&A, evaluation, and improvement notes, saved locally or to Feishu wiki.</i>
</p>

---

## 📋 目录 / Table of Contents

- [功能亮点 / Features](#-功能亮点--features)
- [快速开始 / Quick Start](#-快速开始--quick-start)
- [使用教程 / Usage Guide](#-使用教程--usage-guide)
  - [场景一：上传录音文件](#场景一上传录音文件)
  - [场景二：已有转录文本](#场景二已有转录文本)
  - [场景三：事后补强复盘](#场景三事后补强复盘)
- [输出说明 / Outputs](#-输出说明--outputs)
- [飞书集成 / Feishu Integration](#-飞书集成--feishu-integration)
- [配置文件 / Configuration](#-配置文件--configuration)
- [注意事项 / Precautions](#-注意事项--precautions)
- [Q&A 常见问题](#-qa-常见问题)
- [更新日志 / Changelog](#-更新日志--changelog)

---

## ✨ 功能亮点 / Features

| 功能 | 说明 |
|------|------|
| **🎤 自动转录** | 本地 Whisper 引擎，支持 .wav / .m4a / .mp3 / .mp4，数据不上传 |
| **🧠 发言人识别** | 智能区分面试官 vs 候选人，对话自动标注 |
| **📄 多文件合并** | 多段录音按时间排序自动合并为一场 |
| **📝 三份输出** | 完整对话、问答整理、面试总评，总评同步输出到对话框 |
| **📊 多维表格归档** | 自动写入飞书多维表格，记录公司、岗位、轮次与文档链接 |
| **🏷️ 多轮命名** | 同公司多次面试自动追加轮次后缀（如"天气一面"→"天气二面"） |
| **📚 飞书知识库** | 复盘文档自动写入飞书知识库，含父子层级结构 |
| **📈 八股汇总** | 自动提取纯理论八股题，按日期追加形成复习库 |
| **📑 面试总览索引** | 维护全局索引，一键跳转所有历史面试记录 |
| **🔍 源码定向读取** | 根据面试技术点精准定位项目源码，补强回答有据可依 |

---

## 🚀 快速开始 / Quick Start

### 安装

```bash
# 安装 skill
npx skills add realwoolf/voice-debrief
```

### 前置依赖

```bash
# Apple Silicon Mac（推荐）
pip install mlx-whisper
brew install ffmpeg

# Windows / Linux / Intel Mac
pip install faster-whisper
# ffmpeg 安装方式见下
```

**ffmpeg 安装**：

| 平台 | 命令 |
|------|------|
| macOS | `brew install ffmpeg` |
| Ubuntu/Debian | `sudo apt install ffmpeg` |
| Windows | `winget install ffmpeg` 或前往 [ffmpeg.org](https://ffmpeg.org/download.html) 下载 |

### 最简使用

把录音文件路径发给 Claude，说：

```
帮我整理这场面试 D:\recordings\面试录音.mp3
```

已有 PDF 转录文本同理：

```
帮我整理这场面试 D:\transcripts\面试转录.pdf
```

---

## 📖 使用教程 / Usage Guide

### 场景一：上传录音文件

**流程**：录音 → 本地 Whisper 转录 → 生成完整对话 → 用户确认补强重点 → 生成提纲 → 填写详细版 → 保存到本地 + 飞书

```
帮我整理这场面试 D:\浏览器下载\标准录音 10.doc
```

> **提示**：文件名尽量包含日期，如 `2026-07-14_字节跳动_后端_一面.doc`，这样 skill 会自动提取时间信息。

### 场景二：已有转录文本

如果已有官方转录文本（如 ColorOS 转写的 `.doc` 文件），直接发送路径即可，skill 会自动解析：

- 自动识别 `.doc` 实际为 HTML 格式（ColorOS 转写特点）
- 提取 `<pre>` 标签内的对话内容
- 即使只有单方录音（全部标记为"讲话人 1"），也会从候选人回答中反向推断面试官问题

### 场景三：事后补强复盘

生成完整对话后，skill 会询问你：

> **这次面试有哪些点你觉得需要着重补强？**

你可以：
- **指定方向**：列出你想加强的问题（如"那个 RAG 多路召回的问题我没答好"）
- **全权委托**：回复"你自己来"，skill 会结合转录内容自动判断薄弱环节
- **事后补充**：如果之后有了提纲文件，直接发给 Claude，skill 会按提纲生成详细版

---

## 📂 输出说明 / Outputs

每场面试在输出目录下生成以下文件：

```
D:\all_about_job\面试复盘\
├── 2026-07-14_面试\
│   ├── 转录文本.html          # 原始转录，不做任何修改
│   ├── 完整对话.md             # 完整对话 + 面试技术点清单
│   ├── 复盘.md                 # 提纲（用户确认用）
│   └── 复盘_详细版.md          # 完整补强回答
├── 面试总览.md                 # 全局索引（自动追加）
└── 八股汇总.md                 # 纯理论八股复习库（自动追加）
```

### 详细版格式

每条补强回答包含三个维度：

| 维度 | 说明 |
|------|------|
| **原回答** | 你在面试中实际怎么答的 |
| **简历/项目实际内容** | 你简历上写的、项目里真正实现的 |
| **补强回答** | 优化后的完整回答，结合项目源码细节 |

> 所有引用的项目路径均来自面试技术点定向读取，**不会凭空捏造**。

---

## 🔗 飞书集成 / Feishu Integration

### 配置流程

首次使用保存到飞书时，skill 会引导完成飞书自建应用配置（约 5 分钟），之后浏览器自动完成 OAuth 授权。

### 知识库结构

飞书知识库"面试记录"中的文档结构：

```
📁 面试记录（知识库根目录）
├── 📄 2026-07-14面试-明悉科技          ← 精简版复盘（主文档）
│   ├── 📄 明悉科技 一面 — 复盘提纲     ← 子页面
│   └── 📄 明悉科技 一面 — 详细复盘     ← 子页面
├── 📄 2026-07-07面试-字节跳动
│   ├── 📄 字节跳动 二面 — 复盘提纲
│   └── 📄 字节跳动 二面 — 详细复盘
└── ...
```

### 多维表格

每场面试自动写入飞书多维表格，包含字段：

| 字段 | 说明 |
|------|------|
| 日期 | 面试日期 |
| 公司 | 面试公司名称 |
| 岗位 | 投递岗位 |
| 轮次 | 第几面 |
| 完整对话 | 链接到完整对话文档 |
| 回答补强 | 链接到详细复盘文档 |

---

## ⚙️ 配置文件 / Configuration

路径：`D:\all_about_job\面试复盘\config.yml`

```yaml
resume_path: /d/all_about_job/简历/Ageng应用开发.pdf
project_paths:
  - /d/all_code/Aagent_zhisaotong
  - /d/all_code/agent_data
output_dir: /d/all_about_job/面试复盘
feishu_bitable:
  app_token: "CwQzbmTwhacIkeseqobcRSwdnVb"
  table_id: "tblAI5D0d93SINvE"
```

| 字段 | 必填 | 说明 |
|------|------|------|
| `resume_path` | ✅ | 简历 PDF 路径 |
| `project_paths` | ✅ | 项目代码目录，用于定向读取源码 |
| `output_dir` | ✅ | 本地输出目录 |
| `feishu_bitable` | ❌ | 飞书多维表格配置（仅使用飞书时需要） |

---

## ⚠️ 注意事项 / Precautions

### 数据安全

| 事项 | 说明 |
|------|------|
| **🔒 纯本地转录** | Whisper 在本地运行，录音不上传任何第三方服务 |
| **🔒 纯本地分析** | 简历和项目代码仅在本地读取，不发送到外部 API |
| **🔒 飞书可控** | 保存到飞书需你主动授权，OAuth 流程透明可控 |

### 使用须知

1. **ColorOS 转录文件**：`.doc` 文件实际是 HTML 格式，skill 会自动处理。如果只有单方录音（全部标记为"讲话人 1"），属于正常现象。

2. **路径校验**：skill 启动时会校验 `config.yml` 中所有路径的有效性，确保简历和项目目录存在。

3. **非破坏性原则**：skill **不会修改**你的简历和项目文件，所有输出都在独立的 `output_dir` 中。

4. **飞书配置持久化**：配置文件路径为 `%USERPROFILE%\.claude\feishu_config.json`，如需重新授权可删除此文件。

5. **多轮面试**：同公司多次面试会自动追加轮次后缀，历史记录不会混乱。

6. **项目源码读取**：仅根据面试中提到的技术点（如"多路召回""Memory 机制"）定向读取相关源码，不会无差别扫描全部代码。

7. **术语纠错**：补强回答中会对候选人不标准的术语给出建议性纠正（如"主从架构"→"Orchestration 模式"、"SKU"→"Skill"），语气保持建设性。

### 已知限制

- **长录音处理**：超过 1 小时的录音处理时间较长（约 10-15 分钟），建议分段录制
- **单方录音**：ColorOS 转录可能只录到候选人单方声音，面试官问题需从回答中推断
- **飞书依赖**：飞书知识库功能需要网络连接和有效的 OAuth 授权

---

## ❓ Q&A 常见问题

**Q：录音文件有大小限制吗？**  
A：没有严格限制，但建议单段不超过 2 小时。超过 1 小时的处理时间会明显增加。

**Q：可以批量处理多场面试吗？**  
A：目前支持单场次处理。多场面试请依次发送。

**Q：飞书配置必须吗？**  
A：不是。可以选择仅保存到本地，跳过飞书配置。

**Q：如果录音质量不好怎么办？**  
A：Whisper 对噪音有一定容忍度，但建议在安静环境下录音。如果识别效果差，可以改用官方转录文本。

**Q：补强回答中的源码引用准确吗？**  
A：所有引用的项目路径均来自 Skill 对源码的**定向读取**。如果发现引用路径不存在，请在 issue 中反馈。

---

## 📝 更新日志 / Changelog

### v1.1.0 (2026-07)
- ✨ 新增飞书知识库父子层级支持
- ✨ 新增八股汇总自动提取
- ✨ 新增面试总览索引
- 🔧 优化 ColorOS 转录解析
- 🔧 优化多轮面试命名

### v1.0.0 (2026-06)
- 🎉 初始发布
- 支持录音转录 + 完整对话生成
- 支持复盘提纲 + 详细补强回答
- 支持飞书多维表格归档

---

<p align="center">
  <b>Made with ❤️ for job seekers</b><br>
  <i>If you find this project helpful, give it a ⭐!</i>
</p>