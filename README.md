# Doubao MCP Server

## 项目简介

Doubao MCP Server 是一个基于火山引擎豆包API的MCP（Model Context Protocol）服务器，为AI客户端提供强大的多模态生成能力。该项目的开发初衷是为了将火山引擎豆包的先进AI生成能力集成到支持MCP协议的各种AI客户端中，让用户能够在熟悉的开发环境中直接使用文生图、文生视频、图生视频等功能。

**核心功能特性：**

- **文生图**：根据文本描述生成高质量图片
- **文生视频**：根据文本描述生成视频内容
- **图生视频**：基于图片和文本描述生成动态视频
- **图片编码**：支持本地图片文件转换为base64编码
- **模型配置**：支持多种豆包AI模型选择

**支持的AI模型：**

- 文生图模型：`doubao-seedream-3-0-t2i-250415`
- 图生视频模型：`doubao-seedance-1-0-lite-i2v-250428`
- 文生视频模型：`doubao-seedance-1-0-lite-t2v-250428`


## 部署指南

### **环境依赖**

- Python >= 3.13
- 火山引擎豆包API密钥


### **安装方式**

**方式一：使用pip安装**

```bash
pip install doubao-mcp-server
```

**方式二：使用uvx安装（推荐）**

```bash
uvx doubao-mcp-server
```


### **客户端配置**

#### **Cursor配置**

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


#### **Cherry Studio配置**

1. 打开 Cherry Studio

2. 进入 **设置 → MCP Servers → 添加服务器**

3. 配置参数：
   - **名称**: `doubao-mcp-server`
   - **描述**: `豆包AI生成服务`
   - **类型**: `STDIO`
   - **命令**: `uvx`
   - **参数**: `doubao-mcp-server`
   - **环境变量**: `DOUBAO_API_KEY=your-api-key-here`
   
4. 点击保存并启用

   详细图解

   ![image-20250615165107667](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165107667.png)

![image-20250615165205135](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165205135.png)

配置好后，可以检查到工具有哪些

![image-20250615165249803](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165249803.png)

#### **Claude Desktop配置**

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


#### **Continue.dev配置**

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


### **API密钥获取**

1. 访问 [火山引擎控制台](https://console.volcengine.com/)

2. 注册并登录账户

3. 开通豆包大模型服务（需要分别授权开通各个模型）

   火山引擎模型，需要分别授权开通，点击开通管理

   ![image-20250614201840722](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250614201840722.png.png)

4. 在API管理中创建API密钥

   API 管理

   ![image-20250614201758573](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250614201758573.png.png)

## 可用工具

### **1. set_api_key**

设置豆包API密钥

- `api_key` (string): 豆包API密钥


### **2. text_to_image**

根据文本描述生成图片

- `prompt` (string): 图片描述提示词
- `size` (string, 可选): 图片尺寸，默认"1024x1024"
- `model` (string, 可选): 模型名称

**支持的图片尺寸**: 512x512, 768x768, 1024x1024, 1024x1792, 1792x1024

### **3. text_to_video**

根据文本描述生成视频

- `prompt` (string): 视频描述提示词
- `duration` (string, 可选): 视频时长（秒），默认"5"
- `ratio` (string, 可选): 视频比例，默认"16:9"
- `model` (string, 可选): 模型名称

**支持的视频比例**: 16:9, 9:16, 1:1

### **4. image_to_video**

根据图片和文本描述生成视频

- `prompt` (string): 视频描述提示词
- `image_base64` (string): 图片的base64编码字符串
- `duration` (string, 可选): 视频时长（秒），默认"5"
- `ratio` (string, 可选): 视频比例，默认"16:9"
- `model` (string, 可选): 模型名称


### **5. encode_image_to_base64**

将本地图片文件编码为base64字符串

- `image_path` (string): 图片文件路径


## 使用示例

### **文生图示例**

```
请使用text_to_image工具生成一张"夕阳下的海边风景"图片
```


### **文生视频示例**

```
请使用text_to_video工具生成一个"猫咪在花园里玩耍"的5秒视频
```


### **图生视频示例**

```
首先使用encode_image_to_base64将图片编码，然后使用image_to_video生成视频
```

  图解案例（文生生图）

  ![image-20250615165711353](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165711353.png)

文生视频

![image-20250615165906609](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615165906609.png)

生成的视频

![image-20250615170034340](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250615170034340.png)

## 注意事项

- 视频生成任务可能需要较长时间完成，请耐心等待
- 确保API密钥有足够的配额
- 生成的内容URL有时效性，请及时保存


## 故障排除

### **常见问题**

1. **API密钥错误**: 确保API密钥正确且有效
2. **网络连接问题**: 检查网络连接和防火墙设置
3. **模型不可用**: 确认使用的模型名称正确

### **调试模式**

启用详细日志输出：

```bash
uvx doubao-mcp-server --verbose
```

##  更新说明

 2025年6月29日-version 0.0.1：初始版本发布，支持文生图、文生视频、图生视频功能。

 2025年6月29日-version 0.0.4：去掉set_api_key mcp_tool 工具，方便用户使用，使用DOUBAO_API_KEY 参数赋值

## 项目信息

**许可证**: MIT License

**作者**: wwzhouhui - [75271002@qq.com](mailto:75271002@qq.com)

**版本**: v0.0.1

- 初始版本发布
- 支持文生图、文生视频、图生视频功能
- 集成火山引擎豆包API

**贡献**: 欢迎提交Issue和Pull Request来改进这个项目