# Doubao MCP Server

> 基于火山引擎豆包 API 的 MCP (Model Context Protocol) 服务器，为 AI 客户端提供强大的多模态生成能力

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.13+-blue.svg)
![MCP](https://img.shields.io/badge/mcp-1.9+-green.svg)
![Version](https://img.shields.io/badge/version-0.0.4-orange.svg)

---

## 项目介绍

Doubao MCP Server 是一个专业的基于火山引擎豆包 API 的 MCP 服务器，为 AI 客户端提供强大的多模态生成能力。该项目将火山引擎豆包的先进 AI 生成能力集成到支持 MCP 协议的各种 AI 客户端中，让用户能够在熟悉的开发环境中直接使用文生图、文生视频、图生视频等功能。

### 核心特性

- **文生图功能**: 根据文本描述生成高质量图片
- **文生视频功能**: 根据文本描述生成视频内容
- **图生视频功能**: 基于图片和文本描述生成动态视频
- **图片编码**: 支持本地图片文件转换为 base64 编码
- **多模型支持**: 支持多种豆包 AI 模型选择
- **环境变量配置**: 使用 DOUBAO_API_KEY 简化配置流程
- **MCP 协议**: 完全兼容 Model Context Protocol 1.9+
- **多客户端支持**: 支持 Cursor、Cherry Studio、Claude Desktop、Continue.dev

---

## 功能清单

| 功能名称 | 功能说明 | 技术栈 | 状态 |
|---------|---------|--------|------|
| 文生图 | 根据文本描述生成高质量图片 | 豆包 Seedream API | ✅ 稳定 |
| 文生视频 | 根据文本描述生成视频内容 | 豆包 Seedance API | ✅ 稳定 |
| 图生视频 | 基于图片和文本生成动态视频 | 豆包 Seedance API | ✅ 稳定 |
| 图片编码 | 本地图片转 base64 编码 | Python base64 | ✅ 稳定 |
| MCP 协议 | Model Context Protocol 服务 | FastMCP 1.9+ | ✅ 稳定 |
| 环境变量 | DOUBAO_API_KEY 配置 | os.getenv | ✅ 稳定 |

---

## 技术架构

| 技术 | 版本 | 用途 |
|------|------|------|
| Python | 3.13+ | 主要开发语言 |
| MCP | 1.9+ | Model Context Protocol |
| FastMCP | latest | MCP 服务器框架 |
| OpenAI SDK | 1.86+ | API 客户端 |
| requests | 2.31+ | HTTP 客户端 |

### 通信架构

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                            通信架构图                                            │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│   ┌──────────────────┐       ┌─────────────────────────┐       ┌─────────────┐ │
│   │  AI 客户端         │ ◄────► │   Doubao MCP Server    │ ◄────► │ 豆包 API    │ │
│   │ (Cursor/Claude)   │       │   STDIO                 │       │  火山引擎    │ │
│   └──────────────────┘       └─────────────────────────┘       └─────────────┘ │
│           │                            │                              │        │
│           ▼                            ▼                              ▼        │
│   AI 对话界面                MCP 协议通信              多模态内容生成     │
│   生成图片/视频请求            双向数据传输              返回生成结果      │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 安装说明

### 环境要求

- Python 3.13+
- 火山引擎豆包 API 密钥

### 安装依赖

**方式一：使用 pip 安装**

```bash
pip install doubao-mcp-server
```

**方式二：使用 uvx 安装（推荐）**

```bash
uvx doubao-mcp-server
```

---

## 使用说明

### 客户端配置

#### Cursor 配置

在 `~/.cursor/mcp.json` 文件中添加以下配置：

```json
{
  "mcpServers": {
    "doubao-mcp-server": {
      "command": "uvx",
      "args": [
        "doubao-mcp-server"
      ],
      "env": {
        "DOUBAO_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

#### Cherry Studio 配置

1. 打开 Cherry Studio
2. 进入 **设置 → MCP Servers → 添加服务器**
3. 配置参数：
   - **名称**: `doubao-mcp-server`
   - **描述**: `豆包 AI 生成服务`
   - **类型**: `STDIO`
   - **命令**: `uvx`
   - **参数**: `doubao-mcp-server`
   - **环境变量**: `DOUBAO_API_KEY=your-api-key-here`
4. 点击保存并启用

![Cherry Studio 配置示例](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165107667.png)

#### Claude Desktop 配置

在 `claude_desktop_config.json` 文件中添加：

```json
{
  "mcpServers": {
    "doubao-mcp-server": {
      "command": "uvx",
      "args": ["doubao-mcp-server"],
      "env": {
        "DOUBAO_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

#### Continue.dev 配置

在 `config.json` 文件中添加：

```json
{
  "mcpServers": [
    {
      "name": "doubao-mcp-server",
      "command": "uvx",
      "args": ["doubao-mcp-server"],
      "env": {
        "DOUBAO_API_KEY": "your-api-key-here"
      }
    }
  ]
}
```

---

## 配置说明

### 环境变量配置

| 变量名 | 说明 | 必需 |
|--------|------|------|
| `DOUBAO_API_KEY` | 豆包 API 密钥 | 必填 |

### API 密钥获取

1. 访问 [火山引擎控制台](https://console.volcengine.com/)
2. 注册并登录账户
3. 开通豆包大模型服务（需要分别授权开通各个模型）
4. 在 API 管理中创建 API 密钥

![开通管理](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250614201840722.png.png)

### 支持的模型

| 模型类型 | 模型名称 | 功能 |
|---------|---------|------|
| 文生图 | `doubao-seedream-3-0-t2i-250415` | 根据文本生成图片 |
| 图生视频 | `doubao-seedance-1-0-lite-i2v-250428` | 根据图片生成视频 |
| 文生视频 | `doubao-seedance-1-0-lite-t2v-250428` | 根据文本生成视频 |

---

## 可用工具

### 1. text_to_image

根据文本描述生成图片

**参数**：
- `prompt` (string): 图片描述提示词
- `size` (string, 可选): 图片尺寸，默认 "1024x1024"
- `model` (string, 可选): 模型名称，默认 "doubao-seedream-3-0-t2i-250415"

**支持的图片尺寸**: 512x512, 768x768, 1024x1024, 1024x1792, 1792x1024

### 2. text_to_video

根据文本描述生成视频

**参数**：
- `prompt` (string): 视频描述提示词
- `duration` (string, 可选): 视频时长（秒），默认 "5"
- `ratio` (string, 可选): 视频比例，默认 "16:9"
- `model` (string, 可选): 模型名称，默认 "doubao-seedance-1-0-lite-t2v-250428"

**支持的视频比例**: 16:9, 9:16, 1:1

### 3. image_to_video

根据图片和文本描述生成视频

**参数**：
- `prompt` (string): 视频描述提示词
- `image_base64` (string): 图片的 base64 编码字符串
- `duration` (string, 可选): 视频时长（秒），默认 "5"
- `ratio` (string, 可选): 视频比例，默认 "16:9"
- `model` (string, 可选): 模型名称，默认 "doubao-seedance-1-0-lite-i2v-250428"

### 4. encode_image_to_base64

将本地图片文件编码为 base64 字符串

**参数**：
- `image_path` (string): 图片文件路径

---

## 使用示例

### 文生图示例

```
请使用 text_to_image 工具生成一张"夕阳下的海边风景"图片
```

### 文生视频示例

```
请使用 text_to_video 工具生成一个"猫咪在花园里玩耍"的 5 秒视频
```

### 图生视频示例

```
首先使用 encode_image_to_base64 将图片编码，然后使用 image_to_video 生成视频
```

![文生图示例](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165711353.png)

![文生视频示例](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165906609.png)

---

## 项目结构

```
doubao_mcp_server/
├── doubao_mcp_server.py    # 主程序入口
├── __init__.py             # 包初始化文件
├── pyproject.toml          # 项目配置
├── requirements.txt        # 依赖列表（pip）
├── README.md               # 项目文档
└── .gitignore              # Git 忽略配置
```

---

## 开发指南

### 本地开发

```bash
# 克隆仓库
git clone https://github.com/wwwzhouhui/doubao_mcp_server.git
cd doubao_mcp_server

# 创建虚拟环境
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# 或
.venv\Scripts\activate     # Windows

# 安装依赖
pip install -e .

# 配置 API 密钥
export DOUBAO_API_KEY="your-api-key-here"

# 运行服务器
python -m doubao_mcp_server
```

### 调试模式

启用详细日志输出：

```bash
uvx doubao-mcp-server --verbose
```

---

## 常见问题

<details>
<summary>Q: API 密钥错误？</summary>

A:
1. 确保已从火山引擎控制台获取有效的 API 密钥
2. 检查 `DOUBAO_API_KEY` 环境变量是否正确设置
3. 确认已开通相应的豆包模型服务权限
</details>

<details>
<summary>Q: 网络连接问题？</summary>

A:
1. 检查网络连接和防火墙设置
2. 确认可以访问火山引擎 API 服务
3. 检查代理设置
</details>

<details>
<summary>Q: 模型不可用？</summary>

A:
1. 确认使用的模型名称正确
2. 在火山引擎控制台检查模型是否已开通
3. 检查 API 密钥是否有足够的配额
</details>

<details>
<summary>Q: 视频生成超时？</summary>

A:
1. 视频生成任务可能需要较长时间，请耐心等待
2. 检查 API 密钥配额是否充足
3. 确认网络连接稳定
</details>

<details>
<summary>Q: 生成的内容 URL 失效？</summary>

A:
1. 生成的内容 URL 有时效性，请及时保存
2. 下载生成的内容到本地存储
3. URL 有效期通常为 24 小时
</details>

<details>
<summary>Q: uvx 命令未找到？</summary>

A:
1. 安装 uv 包管理器：`curl -LsSf https://astral.sh/uv/install.sh | sh`
2. 或使用 pip 全局安装包
3. 检查 PATH 环境变量
</details>

<details>
<summary>Q: 图片尺寸不正确？</summary>

A:
1. 确认使用支持的尺寸：512x512, 768x768, 1024x1024, 1024x1792, 1792x1024
2. 尺寸格式必须为"宽x高"，如"1024x1024"
3. 不同模型可能支持的尺寸不同
</details>

<details>
<summary>Q: 视频比例不支持？</summary>

A:
1. 确认使用的比例：16:9, 9:16, 1:1
2. 不同模型可能支持不同的比例
3. 检查模型文档确认支持的比例
</details>

<details>
<summary>Q: base64 编码错误？</summary>

A:
1. 确保图片文件路径正确
2. 检查图片文件格式是否支持
3. 图片文件大小可能有限制
</details>

<details>
<summary>Q: 如何更换模型？</summary>

A:
1. 在调用工具时指定 `model` 参数
2. 确保模型名称正确
3. 确认已开通该模型的使用权限
</details>

---

## 技术交流群

欢迎加入技术交流群，分享你的使用心得和反馈建议：

![技术交流群](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20260303214419_166_292.jpg)

---

## 作者联系

- **微信**: laohaibao2025
- **邮箱**: 75271002@qq.com

![微信二维码](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Screenshot_20260123_095617_com.tencent.mm.jpg)

---

## 打赏

如果这个项目对你有帮助，欢迎请我喝杯咖啡 ☕

**微信支付**

![微信支付](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250914152855543.png)

---

## Star History

如果觉得项目不错，欢迎点个 Star ⭐

[![Star History Chart](https://api.star-history.com/svg?repos=wwwzhouhui/doubao_mcp_server&type=Date)](https://star-history.com/#wwwzhouhui/doubao_mcp_server&Date)

---

## License

MIT License

---

## 更新日志

### v0.0.4 (2025-06-29)

- ✅ 去掉 set_api_key mcp_tool 工具
- ✅ 使用 DOUBAO_API_KEY 环境变量参数赋值
- ✅ 简化用户配置流程

### v0.0.1 (2025-06-29)

- ✅ 初始版本发布
- ✅ 支持文生图、文生视频、图生视频功能
- ✅ 集成火山引擎豆包 API

---

## 贡献指南

欢迎提交 Issue 和 Pull Request 来改进这个项目！

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feature/amazing-feature`
3. 提交更改：`git commit -m 'Add amazing feature'`
4. 推送到分支：`git push origin feature/amazing-feature`
5. 提交 Pull Request

---

## 注意事项

- 视频生成任务可能需要较长时间完成，请耐心等待
- 确保 API 密钥有足够的配额
- 生成的内容 URL 有时效性，请及时保存
- 需要在火山引擎控制台分别开通各个模型的使用权限

---

**Enjoy creating multimedia content with Doubao AI! 🎨🎬✨**
