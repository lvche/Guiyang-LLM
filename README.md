# 🌶️ Guiyang-LLM: 贵阳方言版 Qwen2.5 大模型

<div align="center">

[![ModelScope](https://img.shields.io/badge/ModelScope-模型下载-blue?logo=modelscope)](https://modelscope.cn/models/lvchenghandsome/Qwen2.5-Guiyang-7B-Instruct)
[![Python](https://img.shields.io/badge/Python-3.10%2B-green)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellow)](./LICENSE)
[![Framework](https://img.shields.io/badge/Framework-LLaMA--Factory-purple)](https://github.com/hiyouga/LLaMA-Factory)

**基于 Qwen2.5-7B-Instruct 微调的贵阳方言垂直领域大模型**
<br>
*保护方言文化 · 探索 AI 本地化落地 · 赋予大模型"贵阳味"*

[在线体验 (ModelScope)](https://modelscope.cn/models/lvchenghandsome/Qwen2.5-Guiyang-7B-Instruct) | [项目背景](#-项目背景) | [快速开始](#-快速开始)

</div>

## 📖 项目背景 (Background)

**Guiyang-LLM** 是一个专注于理解和生成**贵阳方言**（Guiyang Dialect）的大语言模型。本项目基于阿里通义千问 **Qwen2.5-7B** 架构，使用了经过清洗的贵阳方言对话数据集进行 **LoRA 微调**，并完成了 **GGUF 量化**，使其能够轻松运行在个人电脑或边缘设备上。

该项目旨在解决通用大模型对特定地域方言理解能力不足的问题，同时也作为一次 **从数据清洗、SFT 微调到量化部署** 的全流程 AI 工程实践。

## 📥 模型下载 (Download)

由于 GitHub 文件大小限制，完整的模型权重（Safetensors）和量化文件（GGUF）已托管至 **魔塔社区 (ModelScope)**。

| 模型版本 | 描述 | 下载链接 |
| :--- | :--- | :--- |
| **Full Weights** | 完整的 PyTorch/Safetensors 权重，支持继续微调 | [👉 前往下载](https://modelscope.cn/models/lvchenghandsome/Qwen2.5-Guiyang-7B-Instruct/files) |
| **GGUF (Q4_K_M)** | 4-bit 量化版，推荐用于 Ollama 本地部署 (约 4.5GB) | [👉 前往下载](https://modelscope.cn/models/lvchenghandsome/Qwen2.5-Guiyang-7B-Instruct/files) |

## 🚀 快速开始 (Quick Start)

### 使用 Ollama 本地运行 (推荐)
这是最简单的方式，适合在本地电脑直接与模型对话。

1. **下载模型**：从上方链接下载 `guiyang-7b-q4.gguf` 文件。
2. **安装 Ollama**：请访问 [Ollama 官网](https://ollama.com/) 下载。
3. **创建模型**：
   在 GGUF 文件所在目录下创建一个名为 `Modelfile` 的文件，内容如下：
   ```dockerfile
   FROM ./guiyang-7b-q4.gguf
   
   # 创造力参数
   PARAMETER temperature 0.7
   PARAMETER top_p 0.8
   PARAMETER repeat_penalty 1.05

   # 对话模板 (ChatML)
   TEMPLATE """{{ if .System }}<|im_start|>system
   {{ .System }}<|im_end|>
   {{ end }}{{ if .Prompt }}<|im_start|>user
   {{ .Prompt }}<|im_end|>
   {{ end }}<|im_start|>assistant
   {{ .Response }}<|im_end|>"""

   # 停止词
   PARAMETER stop "<|im_start|>"
   PARAMETER stop "<|im_end|>"

   # 系统提示词 (人设注入)
   SYSTEM """你叫小c，是小吕开发的人工智能助手。你同时也是一个地道的贵阳人，说话风趣幽默，喜欢用贵阳方言（如“哪样”、“安逸”）和别人摆龙门阵。你的回答要符合逻辑，语气自然。"""
4. **运行**：
   ```dockerfile
   ollama create guiyang-7b -f Modelfile
   ollama run guiyang-7b
