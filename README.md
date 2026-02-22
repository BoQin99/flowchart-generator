# Flowchart Generator Skill

一个用于 Claude Code 的专业技能，使用 MCP draw.io 服务器生成专业的流程图。

## 功能特性

- 🎨 生成专业的 draw.io 流程图
- 🔄 实时预览，支持浏览器中的交互式编辑
- 📦 支持多种流程图类型（业务流程、系统架构、决策流程等）
- 🌍 支持中文文本和自定义样式
- 💾 可导出为 SVG、PNG、PDF 等多种格式

---

## 依赖说明

### MCP 依赖

此 skill 依赖于 **MCP draw.io 服务器**，用于生成和编辑流程图。

| 项目 | 信息 |
|------|------|
| **包名** | `@modelcontextprotocol/server-drawio` |
| **作者** | [Anthropic](https://www.anthropic.com/) |
| **仓库** | [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| **说明** | 官方 MCP 服务器集合，包含 draw.io 集成 |

### MCP draw.io 服务器功能

- 在浏览器中打开 draw.io 编辑器
- 通过 MCP 协议创建、编辑和管理图表
- 支持实时预览和交互式编辑
- 导出多种格式（PNG、SVG、PDF、drawio XML）

---

## 前置要求

在安装此 skill 之前，你需要确保：

### 1. 已安装 Claude Code

确保你已安装 [Claude Code CLI](https://claude.ai/code)

验证安装：
```bash
claude --version
```

### 2. 已安装 Node.js 和 npm

MCP 服务器需要 Node.js 运行环境。

**检查版本：**
```bash
node --version  # 需要 18.0 或更高版本
npm --version   # 需要 9.0 或更高版本
```

**安装 Node.js：**
- 访问 [nodejs.org](https://nodejs.org/) 下载并安装
- 或使用包管理器：`brew install node` (macOS), `apt install nodejs npm` (Ubuntu)

---

## MCP 服务器安装步骤

### 步骤 1：安装 MCP draw.io 服务器

有两种安装方式：

**方式 A：使用 npx（推荐，无需全局安装）**

配置时直接使用 `npx` 命令，无需预先安装：
```bash
npx -y @modelcontextprotocol/server-drawio
```

**方式 B：全局安装**

如果你希望全局安装：
```bash
npm install -g @modelcontextprotocol/server-drawio
```

### 步骤 2：配置 Claude Desktop MCP 服务器

找到 Claude Desktop 的配置文件：

**macOS:**
```bash
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```

**Linux:**
```bash
~/.config/Claude/claude_desktop_config.json
```

在配置文件中添加或更新 MCP 服务器配置：

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-drawio"]
    }
  }
}
```

**配置说明：**
- `"drawio"`: MCP 服务器名称，可在 skill 中通过 `mcp__drawio__*` 工具调用
- `"command": "npx"`: 使用 npx 运行 npm 包
- `"args"`: 传递的参数，`-y` 表示自动确认安装

### 步骤 3：重启 Claude Desktop

配置完成后，**必须重启 Claude Desktop 应用** 以加载 MCP 服务器。

### 步骤 4：验证 MCP 连接

重启后，在 Claude Desktop 中：
1. 打开 "MCP" 选项卡
2. 应该能看到 `drawio` 服务器
3. 状态应该显示为 "已连接"

---

## Skill 安装步骤

### 步骤 1：克隆仓库

```bash
# 克隆仓库
git clone https://github.com/BoQin99/flowchart-generator.git

# 进入目录
cd flowchart-generator
```

### 步骤 2：复制 Skill 文件

将 `SKILL.md` 文件复制到你的 Claude Code skills 目录：

```bash
# macOS/Linux
mkdir -p ~/.claude/skills/flowchart-generator
cp SKILL.md ~/.claude/skills/flowchart-generator/SKILL.md

# Windows PowerShell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\flowchart-generator"
Copy-Item SKILL.md -Destination "$env:USERPROFILE\.claude\skills\flowchart-generator\"
```

**重要：** 目录名称 `flowchart-generator` 必须与 SKILL.md 中的 `name` 字段一致。

### 步骤 3：重启 Claude Code

重启 Claude Code 或重新加载技能列表。

### 步骤 4：验证安装

在 Claude Code 中运行：

```
/flowchart-generator
```

如果安装成功，你应该看到 skill 的帮助信息。

---

## 快速开始

安装完成后，你可以这样使用：

```
创建一个用户登录流程图
```

或者：

```
/flowchart-generator 帮我设计一个支付处理系统的流程图
```

---

## 使用示例

### 示例 1：用户登录流程

```
创建一个用户登录流程图，包括：
1. 打开登录页面
2. 输入用户名和密码
3. 验证凭证
4. 验证成功：创建会话、跳转首页
5. 验证失败：显示错误、返回输入
```

### 示例 2：系统架构图

```
绘制一个微服务架构图，包含：
- API Gateway
- User Service
- Order Service
- Payment Service
- Database（每个服务一个）
- Redis 缓存层
```

### 示例 3：决策流程

```
创建一个贷款审批决策流程图：
- 检查信用评分
- 检查收入证明
- 检查债务收入比
- 根据条件决定批准/拒绝/需要人工审核
```

---

## 支持的流程图类型

| 类型 | 描述 |
|------|------|
| 业务流程图 | 描述业务操作的步骤 |
| 系统架构图 | 展示系统组件和关系 |
| 工作流程图 | 团队或个人的工作步骤 |
| 数据流图 | 展示数据在系统中的流动 |
| 决策流程图 | 基于条件的分支决策 |
| 时序图 | 展示组件之间的交互顺序 |
| 用户旅程图 | 展示用户在产品中的路径 |
| 网络拓扑图 | 展示网络设备连接关系 |

---

## 文件说明

| 文件 | 说明 |
|------|------|
| `SKILL.md` | Skill 定义文件，包含名称、描述和完整工作流程 |
| `README.md` | 本文件，项目说明和快速开始指南 |
| `docs/INSTALLATION.md` | 详细的安装指南 |
| `docs/EXAMPLES.md` | 更多使用示例 |

---

## 工作原理

1. **分析需求**：理解用户需要创建的流程图类型和内容
2. **文本预览**：先生成 ASCII 或 Markdown 格式的文本流程图
3. **用户确认**：确认设计是否符合预期
4. **生成图表**：调用 `mcp__drawio` MCP 服务器生成实际图表
5. **实时编辑**：在浏览器中预览和编辑图表
6. **导出保存**：支持多种格式导出

---

## 故障排除

### 问题 1：找不到 skill

**症状：** 在 Claude Code 中看不到 flowchart-generator skill

**解决方案：**
```bash
# 检查文件是否存在
ls -la ~/.claude/skills/flowchart-generator/SKILL.md

# 检查 skill 名称是否正确
head -3 ~/.claude/skills/flowchart-generator/SKILL.md

# 重启 Claude Code
```

### 问题 2：MCP draw.io 服务器未连接

**症状：** Claude Desktop MCP 选项卡中看不到 drawio，或状态显示未连接

**解决方案：**

1. 检查 Node.js 和 npm：
```bash
node --version
npm --version
```

2. 验证配置文件格式：
```bash
# macOS
cat ~/Library/Application Support/Claude/claude_desktop_config.json

# Windows
type %APPDATA%\Claude\claude_desktop_config.json
```

3. 手动测试 MCP 服务器：
```bash
npx -y @modelcontextprotocol/server-drawio
```

4. 确保已重启 Claude Desktop

### 问题 3：浏览器没有打开

**症状：** 调用 draw.io 工具后浏览器未弹出

**解决方案：**

1. 检查端口占用：
```bash
# macOS/Linux
lsof -i :6002

# Windows
netstat -ano | findstr :6002
```

2. 检查浏览器设置和默认浏览器

3. 查看 Claude Code 日志获取错误信息

### 问题 4：npx 命令失败

**症状：** 配置后 MCP 启动失败

**解决方案：**

确保 `node` 和 `npm` 在系统 PATH 中：
```bash
which node
which npm
```

如果使用全局安装方式：
```bash
npm install -g @modelcontextprotocol/server-drawio
```

然后修改配置为：
```json
{
  "mcpServers": {
    "drawio": {
      "command": "mcp-server-drawio"
    }
  }
}
```

---

## 更多文档

- 📖 [详细安装指南](docs/INSTALLATION.md)
- 💡 [更多使用示例](docs/EXAMPLES.md)
- 🔗 [MCP 服务器仓库](https://github.com/modelcontextprotocol/servers)
- 🔗 [draw.io 官网](https://www.drawio.com/)

---

## 参考资源

| 资源 | 链接 |
|------|------|
| Claude Code | https://claude.ai/code |
| MCP 协议文档 | https://modelcontextprotocol.io/ |
| MCP Servers 仓库 | https://github.com/modelcontextprotocol/servers |
| draw.io | https://www.drawio.com/ |
| Anthropic | https://www.anthropic.com/ |

---

## 贡献

欢迎提交 Issue 和 Pull Request！

提交 Issue 时请包含：
- 操作系统版本
- Node.js 和 npm 版本
- Claude Code 版本
- 错误信息和日志

---

## 许可证

MIT License

---

## 致谢

- [Claude Code](https://claude.ai/code) - AI 编程助手
- [draw.io](https://www.drawio.com/) - 开源流程图工具
- [Anthropic MCP Servers](https://github.com/modelcontextprotocol/servers) - MCP 服务器实现

---

**享受创建流程图的乐趣！** 🎉
